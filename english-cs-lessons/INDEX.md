# English + CS Lessons Index

This index summarizes every lesson file in this folder.

Each row lists:

```text
file
lesson range
main topic
the six practice words / concepts in that file
```

Use this file when you want to quickly choose what to review.

---

## File index

| File | Lessons | Main topic | Six practice words / concepts |
| --- | ---: | --- | --- |
| [`README.md`](README.md) | 1-6 | Basic project architecture and core CS foundations | control plane; write-ahead log; evaluation suite; contract-preserving refactor; idempotency; snapshot |
| [`lessons-07-12.md`](lessons-07-12.md) | 7-12 | Evidence, safety gates, AI model internals, and builder structure | digest; release gate; dry run; runtime evidence; causal self-attention; builder pattern |
| [`lessons-13-18.md`](lessons-13-18.md) | 13-18 | Command handling, tokens, upstream services, contracts, protocols, and checkpoints | command dispatch table; tokenization; upstream service; API contract; RESP protocol; model checkpoint |
| [`lessons-19-24.md`](lessons-19-24.md) | 19-24 | CI, test reliability, concurrency, atomicity, and data compatibility | CI workflow; flaky test; race condition; mutex; atomic operation; schema version |
| [`lessons-25-30.md`](lessons-25-30.md) | 25-30 | Operations evidence and implementation-readiness language | dry run; evidence ledger; upstream echo; non-participation boundary; implementation readiness; benchmark history |
| [`lessons-31-36.md`](lessons-31-36.md) | 31-36 | Governance, quality gates, review flow, refactoring, and sharding preparation | credential resolver; remediation gate; batch review; module split; preflight gate; sharding preparation |
| [`lessons-37-42.md`](lessons-37-42.md) | 37-42 | Planning, historical evidence, verification, read-only safety, and regression protection | implementation plan; historical evidence; test suite split; echo verification; read-only boundary; regression hardening |
| [`lessons-43-48.md`](lessons-43-48.md) | 43-48 | Safe connection workflows, receipt chains, operator review, and operational gates | sandbox connection; receipt chain; operator packet; quality gate; scorecard; runbook |
| [`lessons-49-54.md`](lessons-49-54.md) | 49-54 | Compatibility, boundary checks, historical review, echo markers, and readiness packets | compatibility; boundary check; historical review; echo marker; archived evidence; readiness packet |
| [`lessons-55-60.md`](lessons-55-60.md) | 55-60 | Controlled workflows, evidence tracing, regression tests, promotion gates, and stability trends | controlled workflow; evidence trace; regression test; promotion gate; boundary evidence; stability trend |
| [`lessons-61-66.md`](lessons-61-66.md) | 61-66 | Layering, event-driven design, transactions, lookup speed, caching, and data formats | layered architecture; event-driven design; transaction boundary; index; cache; serialization |
| [`lessons-67-72.md`](lessons-67-72.md) | 67-72 | Observability, fault tolerance, flow control, concurrency, queues, and model evaluation | observability; fault tolerance; rate limiting; thread pool; message queue; model evaluation |
| [`lessons-73-78.md`](lessons-73-78.md) | 73-78 | Everyday engineering tools, configuration, errors, documentation, and technical debt | command-line interface; configuration; environment variable; error taxonomy; documentation index; technical debt |

---

## Project focus map

| Project | Best lesson files |
| --- | --- |
| `nodeproj` | `README.md`, `lessons-07-12.md`, `lessons-25-30.md`, `lessons-31-36.md`, `lessons-37-42.md`, `lessons-43-48.md`, `lessons-49-54.md`, `lessons-55-60.md`, `lessons-61-66.md`, `lessons-67-72.md`, `lessons-73-78.md` |
| `mini_kv` | `README.md`, `lessons-13-18.md`, `lessons-19-24.md`, `lessons-25-30.md`, `lessons-37-42.md`, `lessons-43-48.md`, `lessons-49-54.md`, `lessons-55-60.md`, `lessons-61-66.md`, `lessons-67-72.md`, `lessons-73-78.md` |
| `javaproject` | `README.md`, `lessons-07-12.md`, `lessons-25-30.md`, `lessons-31-36.md`, `lessons-37-42.md`, `lessons-43-48.md`, `lessons-49-54.md`, `lessons-55-60.md`, `lessons-61-66.md`, `lessons-67-72.md`, `lessons-73-78.md` |
| `aiproj` | `README.md`, `lessons-13-18.md`, `lessons-25-30.md`, `lessons-31-36.md`, `lessons-37-42.md`, `lessons-43-48.md`, `lessons-49-54.md`, `lessons-55-60.md`, `lessons-61-66.md`, `lessons-67-72.md`, `lessons-73-78.md` |

---

## Suggested learning order

If you are new to the projects:

```text
README.md
    ->
lessons-07-12.md
    ->
lessons-13-18.md
    ->
lessons-19-24.md
```

If you want to explain recent engineering progress:

```text
lessons-25-30.md
    ->
lessons-31-36.md
    ->
lessons-37-42.md
    ->
lessons-43-48.md
    ->
lessons-49-54.md
    ->
lessons-55-60.md
    ->
lessons-61-66.md
    ->
lessons-67-72.md
    ->
lessons-73-78.md
```

If you want to practice interview-style English:

```text
control plane
dry run
upstream echo
credential resolver
preflight gate
read-only boundary
regression hardening
sandbox connection
receipt chain
scorecard
stability trend
layered architecture
observability
model evaluation
technical debt
```

---

## One-sentence summary

These lessons turn the four projects into a technical-English study path: from basic architecture and storage concepts to evidence governance, safe operations, AI evaluation, and distributed-system preparation.
