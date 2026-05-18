# English + Computer Science Lessons from My Projects

This folder collects short English + CS lessons based on my four main projects:

- `aiproj`
- `nodeproj`
- `mini_kv`
- `javaproject`

Each lesson follows the same pattern:

```text
Project
Concept
Simple explanation
Useful English
Good sentence patterns
CS explanation
Practice task
```

The goal is to improve technical English while also understanding real computer science concepts from my own repositories.

---

# Lesson 1: Control Plane

**Project:** `nodeproj`  
**Concept:** control plane

A **control plane** is a service that does not directly do all the work itself. Instead, it coordinates other systems, checks safety, records evidence, and decides whether an operation is allowed.

In `nodeproj`, the Node service works like a control plane for the Java order platform and `mini_kv`.

## Simple idea

```text
user request
    ↓
Node control plane
    ↓
check safety / dry run / evidence
    ↓
Java service or mini_kv
```

## Useful English

| Term | Meaning |
| --- | --- |
| control plane | a system that manages and coordinates other systems |
| upstream service | another service that your system calls or depends on |
| safety boundary | a rule that prevents dangerous actions |
| dry run | a test run that does not really change anything |
| evidence | proof or records showing what happened |

## Good English sentence patterns

```text
This project acts as a control plane for multiple local services.
```

```text
The system uses dry-run operations to avoid unsafe changes.
```

```text
The Node service coordinates Java and C++ components while preserving safety boundaries.
```

## CS explanation

A control plane is common in cloud systems, deployment platforms, and distributed systems.

The control plane usually manages policy, routing, configuration, approval, and observability. The actual work may be done by other services.

In your project, Node is responsible for coordination and safety. Java and `mini_kv` are upstream systems with their own responsibilities.

## Practice

Write 2 sentences about `nodeproj` using these words:

```text
control plane
upstream service
safety boundary
```

---

# Lesson 2: Write-Ahead Log

**Project:** `mini_kv`  
**Concept:** write-ahead log, or WAL

A **write-ahead log** is a file that records changes before the database or storage engine applies them to memory or disk.

In `mini_kv`, WAL helps the key-value store recover data after a restart or crash.

## Simple idea

```text
client sends SET key value
        ↓
write operation to WAL first
        ↓
apply change to in-memory store
        ↓
if program crashes, replay WAL to recover data
```

## Useful English

| Term | Meaning |
| --- | --- |
| write-ahead log | a log that records changes before applying them |
| recovery | restoring data after failure |
| replay | reading old log records and applying them again |
| durability | ability to keep data even after crash |
| mutation | a change to data, such as SET or DEL |

## Good English sentence patterns

```text
The write-ahead log improves durability by recording mutations before applying them.
```

```text
If the server crashes, the system can replay the WAL to recover the latest state.
```

```text
In mini_kv, WAL support makes the key-value store more reliable.
```

## CS explanation

Without WAL, data only in memory may disappear after a crash.

With WAL, every important write operation is saved first. Later, when the program restarts, it can read the log and rebuild the store.

This is a common idea in databases, storage engines, Redis-like systems, and distributed systems.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
write-ahead log
recovery
durability
```

---

# Lesson 3: Evaluation Suite

**Project:** `aiproj`  
**Concept:** evaluation suite

An **evaluation suite** is a group of test prompts or tasks used to check how well a model performs.

In `aiproj`, the MiniGPT model is not only trained. It is also evaluated with fixed prompt suites, benchmark scorecards, and readiness evidence.

The v226 update focuses on **eval-suite coverage readiness**, meaning the project checks whether the evaluation prompts are broad enough before using them to judge model quality.

## Simple idea

```text
trained model
    ↓
fixed prompt suite
    ↓
model outputs
    ↓
evaluation report
    ↓
coverage readiness check
```

## Useful English

| Term | Meaning |
| --- | --- |
| evaluation suite | a set of test cases used to evaluate a model |
| benchmark | a standard test used to compare performance |
| coverage | how many important areas are included |
| prompt | input text given to a language model |
| readiness | whether something is ready to be used seriously |

## Good English sentence patterns

```text
The evaluation suite helps measure the model's performance on fixed prompts.
```

```text
Coverage readiness checks whether the benchmark is broad enough for comparison.
```

```text
In aiproj, evaluation is treated as evidence rather than a simple score.
```

## CS explanation

A model can look good on a few easy prompts, but that does not prove it is generally strong.

So an evaluation suite should include different task types, difficulty levels, and tags. If the suite is too small, the result may be misleading.

`aiproj` separates two ideas:

```text
the model can run this test
```

and

```text
this test is good enough to compare model quality
```

That is a mature AI engineering habit.

## Practice

Write 2 sentences about `aiproj` using these words:

```text
evaluation suite
coverage
benchmark
```

---

# Lesson 4: Contract-Preserving Refactor

**Project:** `javaproject`  
**Concept:** contract-preserving refactor

A **refactor** means changing the internal structure of code without changing what the code does from the outside.

A **contract-preserving refactor** means the API response, field names, digest values, headers, or expected behavior stay the same, even though the internal code becomes cleaner.

In `javaproject`, recent work splits a complex release approval rehearsal builder into clearer parts, while keeping the response and digest contracts unchanged.

## Simple idea

```text
before:
one large builder class
    ↓
hard to read and maintain

after:
normalized request
+ rehearsal sections
+ managed-audit receipt chain
    ↓
same external behavior
but cleaner internal structure
```

## Useful English

| Term | Meaning |
| --- | --- |
| refactor | improve code structure without changing behavior |
| contract | the expected external behavior of an API or function |
| preserve | keep something unchanged |
| response field | a value returned by an API |
| digest | a hash-like value used to verify evidence or data |

## Good English sentence patterns

```text
This refactor improves the internal structure while preserving the external contract.
```

```text
The API response remains unchanged after the builder chain is split.
```

```text
In javaproject, contract-preserving refactoring reduces complexity without breaking existing clients.
```

## CS explanation

In backend systems, many other services may depend on your API.

So you cannot randomly change:

```text
field names
response shapes
HTTP headers
digest calculation rules
error formats
```

Even if your code is messy, users or other services may already rely on the current behavior.

A good refactor improves the inside but keeps the outside stable.

That is why “contract-preserving” is important: it means the code becomes easier to maintain, but other systems do not break.

## Practice

Write 2 sentences about `javaproject` using these words:

```text
refactor
contract
preserve
```

---

# Lesson 5: Idempotency

**Project:** `nodeproj`  
**Concept:** idempotency

**Idempotency** means that doing the same operation multiple times has the same result as doing it once.

In `nodeproj`, idempotency is useful because users or clients may accidentally submit the same request twice, but the system should avoid creating duplicate dangerous operations.

## Simple idea

```text
first request:
POST create operation intent
Idempotency-Key: abc123
        ↓
system creates intent #1

duplicate request:
POST create operation intent
Idempotency-Key: abc123
        ↓
system returns the same intent #1
instead of creating intent #2
```

## Useful English

| Term | Meaning |
| --- | --- |
| idempotency | repeated requests produce the same result |
| duplicate request | the same request sent more than once |
| replay | returning or reusing a previous result |
| operation intent | a recorded plan to perform an operation |
| safe retry | trying again without causing duplicate side effects |

## Good English sentence patterns

```text
Idempotency prevents duplicate requests from creating duplicate operations.
```

```text
The system uses an Idempotency-Key to support safe retries.
```

```text
In nodeproj, duplicate intent creation can replay the original result instead of creating a new intent.
```

## CS explanation

Network requests are not always reliable.

Sometimes a client sends a request, but the response is lost. The client may not know whether the server succeeded, so it sends the request again.

Without idempotency:

```text
one user action
    ↓
two HTTP requests
    ↓
two operation records
    ↓
possible safety problem
```

With idempotency:

```text
one user action
    ↓
two HTTP requests with same key
    ↓
one operation record
    ↓
safe retry
```

This is very important in payment systems, order systems, deployment systems, and approval workflows.

## Practice

Write 2 sentences about `nodeproj` using these words:

```text
idempotency
duplicate request
safe retry
```

---

# Lesson 6: Snapshot

**Project:** `mini_kv`  
**Concept:** snapshot

A **snapshot** is a saved copy of the system’s current state at a specific moment.

In `mini_kv`, the key-value store supports snapshots, WAL, recovery, and runtime evidence. A snapshot helps the system save the current data state so it can restore faster later.

## Simple idea

```text
current memory state:
key1 -> value1
key2 -> value2
key3 -> value3
        ↓
save snapshot
        ↓
snapshot file stores this state
        ↓
later, load snapshot to restore data
```

## Useful English

| Term | Meaning |
| --- | --- |
| snapshot | a saved copy of current state |
| restore | load saved data back into the system |
| state | current data or condition of a system |
| persistence | saving data so it survives restart |
| recovery | rebuilding the system after failure |

## Good English sentence patterns

```text
A snapshot stores the current state of the key-value database.
```

```text
The system can restore data from a snapshot after restart.
```

```text
In mini_kv, snapshots and WAL work together to improve recovery.
```

## CS explanation

WAL and snapshot solve related but different problems.

A **WAL** records every write operation:

```text
SET a 1
SET b 2
DEL a
```

A **snapshot** stores the full current state:

```text
b -> 2
```

If the system only uses WAL, recovery may become slow because it must replay many old operations.

If the system uses snapshots, it can load the latest snapshot first, then replay only newer WAL records.

So the recovery process can become:

```text
load latest snapshot
        ↓
replay newer WAL records
        ↓
recover final state
```

This is common in databases, Redis-like systems, storage engines, and distributed systems.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
snapshot
restore
recovery
```

---

# Study method

For each lesson:

1. Read the concept aloud in English.
2. Copy the three sentence patterns.
3. Write two original sentences about your own project.
4. Correct grammar and vocabulary.
5. Re-explain the CS concept in your own words.

Suggested sentence pattern:

```text
In [project], [concept] helps the system [do something important].
```

Example:

```text
In mini_kv, snapshots help the system restore data faster after a restart.
```
