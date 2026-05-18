# Project Collection Overview

This document is a short entry point for readers who want to understand the four main projects before opening each repository.

The collection shows a multi-language learning and engineering path across AI, systems programming, backend services, and operations governance.

## Quick map

| Project | Language / Stack | Main idea | Best signal |
| --- | --- | --- | --- |
| `aiproj` | Python / PyTorch | Build, train, inspect, and evaluate a small GPT model from scratch | AI engineering discipline and evaluation workflow |
| `mini_kv` | C++20 / CMake / TCP | Build a Redis-like key-value engine with WAL, snapshots, TCP, and runtime evidence | Systems programming and runtime correctness |
| `javaproject` | Java / Maven | Backend release approval, rehearsal, and managed-audit evidence flows | Enterprise-style backend refactoring and contract preservation |
| `nodeproj` | Node.js / TypeScript / Fastify | Control plane and dashboard connecting Java and mini-kv with safety gates | Full-stack operations architecture and cross-service governance |

## How the projects relate

The projects can be read in two groups.

### Group 1: AI learning and evaluation

`aiproj` is the AI project. It starts with a MiniGPT-style language model and expands into training, generation, inspection, evaluation suites, benchmark reports, model cards, release readiness, and maturity evidence.

It is useful for readers who want to see how a small model project can grow into a more disciplined AI engineering workflow.

### Group 2: Local operations platform

`nodeproj`, `javaproject`, and `mini_kv` form a local multi-service operations practice system.

```text
Node control plane / dashboard
        |
        | coordinates safe local operations
        v
Java order platform        C++ mini-kv storage/runtime service
```

The key idea is not just connecting services. The key idea is controlling risk:

- Keep probes and actions disabled by default.
- Use dry-run command packages before real execution.
- Preserve evidence, digests, archives, and verification records.
- Keep credential, database, restore, production, and managed-audit boundaries explicit.
- Let each service have a clear role and no hidden authority over another service.

## Suggested reading order

For a quick portfolio review:

1. Read this file.
2. Open each repository's `START_HERE.md`.
3. Read only the top part of each large `README.md`.
4. Look at the latest version explanation and screenshot/evidence folder.
5. Inspect tests or recent commits if you want proof of behavior.

For technical depth:

1. Start with `mini_kv` to understand the C++ runtime and command contracts.
2. Read `javaproject` to understand Java release approval and rehearsal evidence.
3. Read `nodeproj` to see how the control plane coordinates the two upstream systems.
4. Read `aiproj` separately as the AI/evaluation engineering track.

## Project summaries

### `aiproj`: MiniGPT From Scratch

A PyTorch practice project for building, training, inspecting, and evaluating a small GPT-style language model.

Highlights:

- Character/BPE tokenizers
- Causal self-attention model core
- Training and generation scripts
- Evaluation suites and benchmark scorecards
- Dataset cards, model cards, release gates, and maturity reports
- Versioned screenshots and detailed code explanations

Latest focus: eval-suite coverage readiness, so prompt suites can be judged before they are used for checkpoint-quality claims.

### `mini_kv`: C++ Redis-like key-value engine

A C++20 practice project for building a small in-memory KV service with runtime correctness evidence.

Highlights:

- Thread-safe in-memory store
- WAL and snapshot support
- TCP server and client
- RESP compatibility
- Runtime JSON evidence such as `INFOJSON`, `STATSJSON`, `SMOKEJSON`, `CHECKJSON`, and `EXPLAINJSON`
- CMake/CTest validation

Latest focus: table-driven command dispatch, replacing a long command-name `if` chain with a dispatch table and enum switch while keeping external behavior unchanged.

### `javaproject`: Java order operations backend

A Java backend project focused on release approval, rehearsal, and managed-audit evidence chains.

Highlights:

- Release approval rehearsal flows
- Managed-audit receipt chains
- Input normalization and response contract preservation
- Read-only rehearsal and sandbox boundaries
- Maven compile/test/package workflows

Latest focus: refactoring the release approval rehearsal builder chain into normalized request, rehearsal sections, and managed-audit receipt-chain contexts while preserving response and digest contracts.

### `nodeproj`: OrderOps Node control plane

A Node.js / TypeScript control plane and dashboard that coordinates the Java and mini-kv systems.

Highlights:

- Fastify service and browser dashboard
- Health and upstream overview endpoints
- SSE event stream
- Dry-run operation flow
- Approval request and decision ledgers
- Evidence reports, archive records, digest verification, handoff packages, release readiness, and deployment approval records
- Production/sandbox readiness planning with explicit disabled-by-default boundaries

Latest focus: CI-stable historical evidence fallback and the next roadmap for sandbox command verification, upstream echo verification, and sandbox connection precheck.

## What makes the collection strong

The strongest signal is not one single feature. It is the pattern across all repositories:

- Every project has a learning goal.
- Every major step has documentation.
- Recent work shows refactoring, tests, evidence, and version discipline.
- The local operations projects have clear safety boundaries instead of pretending a demo is production-ready.
- The AI project avoids overclaiming model quality and adds readiness checks around evaluation suites.

## What could be improved next

The main weakness is presentation density. Many READMEs and version notes are very detailed, which is good for auditability but hard for first-time readers.

Recommended next improvements:

1. Add a one-minute demo screenshot or GIF to each project.
2. Add a short architecture diagram to `nodeproj`.
3. Add a top-level README to `javaproject` if one is missing.
4. Add a small “best files to inspect” section to each `START_HERE.md`.
5. Link each `START_HERE.md` from the top of each main `README.md` later if desired.

## One-sentence pitch

This collection demonstrates a self-directed engineering path from AI model building to C++ systems programming and multi-service operations governance, with unusually strong versioned documentation and evidence discipline.
