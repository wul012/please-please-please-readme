# 阅读前请先看这里

本项目集中的每个项目，几乎都包含较为详细的讲解 README，并且会在 `a/` 文件夹下保留每次运行时的截图记录。

不过需要提前说明：这些讲解内容并不是从最基础的概念开始写起的。它们更侧重于项目过程、代码逻辑、运行结果和关键知识点的说明。如果缺少相关基础知识，阅读时可能会觉得某些地方跳得比较快。

## Node 示例

例如，`wul012/nodeproj/73-operation-approval-execution-gate-archive-v69.md` 中讲到 v69 的执行门禁归档路由入口，以及为什么创建归档时要重新走完整证据链。

v69 新增了三个入口：

```text
POST /api/v1/operation-approval-requests/:requestId/execution-gate-archive
GET  /api/v1/operation-approval-execution-gate-archives
GET  /api/v1/operation-approval-execution-gate-archives/:archiveId?format=markdown
```

整体意思是：v69 提供创建归档、查询归档列表、查询归档详情或 Markdown 三个接口；创建归档时不会直接拿散字段拼记录，而是重新生成 `report`、`verification`、`handoff bundle` 和 `execution gate preview`，再把结果固化为归档记录。

这样可以保证 archive record 没有绕过 v66-v68 已经建立的 request、decision、evidence、bundle、gate preview 链路。

### POST 创建归档

```text
POST /api/v1/operation-approval-requests/:requestId/execution-gate-archive
```

这是创建归档接口。

`POST` 表示创建资源，路径中的 `:requestId` 是审批请求 ID。它的语义是：为某个 approval request 创建一条 execution gate archive record。

创建时，它可能会找到该 request 对应的 decision 和 evidence report，然后生成 gate preview 并归档。

### GET 查询归档列表

```text
GET /api/v1/operation-approval-execution-gate-archives
```

这是归档列表接口。

`GET` 表示查询。它的语义是：查询所有或分页查询 execution gate archive records。

返回内容可能包括：

- `archiveId`
- `requestId`
- `decisionId`
- `state`
- `createdAt`
- `hardBlockerCount`
- `warningCount`
- `archiveDigest`

### GET 查询归档详情或 Markdown

```text
GET /api/v1/operation-approval-execution-gate-archives/:archiveId?format=markdown
```

这是查询单条归档详情接口。

路径中的 `:archiveId` 是归档记录 ID。可选 query 参数 `format=markdown` 表示希望以 Markdown 格式返回。

如果不传 `format=markdown`，接口可能返回 JSON；如果传了 `format=markdown`，则可能返回适合审计、人读、复制到工单中的报告文本。

### 创建归档时重新走证据链

创建归档时，路由会重新走同一条证据链：

```js
const report = await evidenceService.createReport(...);
const verification = createOperationApprovalEvidenceVerification(report);
const bundle = createOperationApprovalHandoffBundle(report, verification);
return createOperationApprovalExecutionGatePreview(bundle);
```

第一步：

```js
const report = await evidenceService.createReport(...);
```

先创建 approval evidence report，也就是审批证据报告。

`report` 应该包含：

- `request`
- `decision`
- `summary`
- `upstreamEvidence`
- `nextActions`
- `digest`

这是 v65/v66 之前已经形成的核心证据报告。

第二步：

```js
const verification = createOperationApprovalEvidenceVerification(report);
```

对 `report` 做 verification，也就是复算 digest、summary、upstream evidence 一致性等检查。

这样做的目的包括：

- 确认 `report` 自洽。
- 确认 `summary` 和原始 evidence 对得上。
- 确认 `digest` 没错。

第三步：

```js
const bundle = createOperationApprovalHandoffBundle(report, verification);
```

生成 handoff bundle，也就是交接包。

handoff bundle 会把 `report` 和 `verification` 整合成给审批或执行前复核使用的结构。

它可能包含：

- approval request
- approval decision
- summary
- verification
- Markdown
- 上游 digest/schema 证据

第四步：

```js
return createOperationApprovalExecutionGatePreview(bundle);
```

再基于 handoff bundle 生成 execution gate preview。

这是 v68 的职责：判断当前 request、decision 和 evidence bundle 是否可以进入执行前复核，但仍然不执行真实动作。

### 为什么不能绕过这条链

创建 archive 时重新走完整链路，是为了避免绕过已有的安全与证据逻辑。

如果 v69 直接接受前端传来的：

- `summary`
- `decision`
- `hardBlockers`
- `warnings`
- `digest`

就可能出现这些风险：

- 前端传入假的 `summary`。
- 跳过 verification。
- 跳过 handoff bundle。
- 跳过 gate preview 判断。
- 归档出一条不可信记录。

现在重新走：

```text
request -> decision -> evidence report -> verification -> handoff bundle -> gate preview -> archive
```

就能保证 archive record 是基于同一套规则生成的。

一句话总结：v69 新增创建归档、查询归档列表、查询归档详情或 Markdown 三个入口；创建归档时会重新生成 `report`、`verification`、`handoff bundle` 和 `execution gate preview`，再固化成 archive record，避免绕过 v66-v68 已经建立的审批请求、决策、证据、交接包和执行门禁预览链路。

## Python 示例

例如，`wul012/aiproj/代码讲解记录/11-v5-prediction-evaluation.md` 中有这样一段流程说明：

```text
checkpoint + data
 -> 采样 batch
 -> 计算平均 cross entropy loss
 -> loss 转 perplexity
 -> 写 eval_report.json
```

这段内容讲的是模型评估脚本的基本流程。

核心意思是：加载 checkpoint 中已经训练好的模型，在一批验证数据上计算平均 loss，再把 loss 转换成 perplexity，最后将评估结果写入 `eval_report.json`。

## 流程拆解

### checkpoint + data

评估通常需要两类输入：

- `checkpoint`：已经训练好的模型参数。
- `data`：用于评估的文本数据。

通常还需要使用与 checkpoint 对应的 tokenizer，否则 token id 可能无法正确对应。

### 采样 batch

从评估数据中取出一批样本进行计算。

`batch` 指的是一小组样本，而不是一次只处理一条数据。例如：

```text
batch_size = 32
```

表示一次评估 32 段文本。

### 计算平均 cross entropy loss

`cross entropy loss` 通常译作“交叉熵损失”。

在语言模型中，它衡量的是：模型预测下一个 token 时，给正确答案分配的概率是否足够高。

- 如果正确 token 的概率高，loss 就低。
- 如果正确 token 的概率低，loss 就高。

平均 cross entropy loss，就是模型在大量 token 上预测错误程度的平均值。

### loss 转 perplexity

`perplexity` 通常译作“困惑度”，一般这样计算：

```text
perplexity = exp(loss)
```

可以直观理解为：模型在预测下一个 token 时，平均像是在多少个候选 token 之间犹豫。

例如：

```text
perplexity = 10
```

可以粗略理解为：模型平均像是在 10 个差不多可能的 token 中选择。

一般来说：

```text
loss 越低 -> perplexity 越低 -> 模型效果越好
```

### 写入 eval_report.json

最后，脚本会把评估结果写入 JSON 文件，例如：

```json
{
  "loss": 2.31,
  "perplexity": 10.08,
  "num_batches": 100,
  "batch_size": 32
}
```

`eval_report.json` 适合用于：

- 对比不同模型版本。
- 记录训练和评估效果。
- 做自动化检查。
- 归档实验结果。

## 阅读建议

遇到不熟悉的概念时，建议多查、多搜索，再回到文档中继续阅读。由于项目文档本身已经比较多，这里不会再把所有基础知识逐一展开。

先理解背景，再看代码和截图，阅读体验会顺很多。

一句话总结：这个流程就是加载训练好的 checkpoint，用评估数据计算平均交叉熵损失，再把 loss 转换成困惑度 perplexity，最后把结果保存到 `eval_report.json`，方便判断模型质量。
