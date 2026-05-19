# English + Computer Science Lessons 19-24

This document continues the English + CS study notes based on my four main projects:

- `aiproj`
- `nodeproj`
- `mini_kv`
- `javaproject`

Each lesson follows the same pattern:

```text
Project
Concept
Simple idea
Useful English
Good sentence patterns
CS explanation
Practice task
```

---

# Lesson 19: CI Workflow

**Project:** `mini_kv`  
**Concept:** CI workflow

A **CI workflow** is an automated process that builds and tests a project whenever code is pushed or changed.

**CI** means **Continuous Integration**.

In `mini_kv`, CI is important because the project has CMake builds, CTest tests, runtime behavior, WAL logic, command parsing, and TCP server/client behavior. Automated checks help make sure new changes do not break old behavior.

## Simple idea

```text
developer pushes code
        ↓
GitHub Actions starts CI workflow
        ↓
configure project
        ↓
build project
        ↓
run tests
        ↓
report pass or fail
```

## Useful English

| Term | Meaning |
| --- | --- |
| CI workflow | an automated build-and-test process |
| continuous integration | regularly checking that code still works after changes |
| build | compile or prepare the project |
| test suite | a group of tests |
| regression | a bug where old behavior breaks after a new change |

## Good English sentence patterns

```text
The CI workflow builds the project and runs the test suite automatically.
```

```text
Continuous integration helps detect regressions early.
```

```text
In mini_kv, CI protects command parsing, WAL behavior, and runtime contracts.
```

## CS explanation

When a project becomes larger, manual testing is not enough.

For example, after changing command dispatch, you need to know whether these parts still work:

```text
SET and GET commands
WAL replay
snapshot restore
TCP client-server behavior
RESP parsing
runtime JSON commands
```

A CI workflow runs checks automatically, so you do not need to remember every test manually.

Without CI:

```text
change code
    ↓
forget to run some tests
    ↓
push broken behavior
```

With CI:

```text
change code
    ↓
automated build and tests
    ↓
find problems earlier
```

The key difference is:

```text
manual testing -> depends on developer memory
CI workflow -> runs automatically and consistently
```

This is important in real software engineering because every new change can accidentally break existing behavior.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
CI workflow
test suite
regression
```

---

# Lesson 20: Flaky Test

**Project:** `mini_kv` / `nodeproj`  
**Concept:** flaky test

A **flaky test** is a test that sometimes passes and sometimes fails, even when the code has not changed.

This can happen because of timing, network issues, file system behavior, random data, concurrency, or environment differences.

In your projects, flaky tests could appear in areas like:

```text
TCP server tests
timeout tests
concurrent request tests
GitHub Actions CI
file or WAL tests
upstream service checks
```

## Simple idea

```text
same code
same test

first CI run:
test passes

second CI run:
test fails

third CI run:
test passes again
```

That means the test may be **flaky**, not necessarily that the main code is always broken.

## Useful English

| Term | Meaning |
| --- | --- |
| flaky test | a test that passes and fails inconsistently |
| deterministic | always producing the same result |
| non-deterministic | not always producing the same result |
| timeout | a failure caused by taking too long |
| race condition | a bug caused by unpredictable execution order |

## Good English sentence patterns

```text
A flaky test can fail even when the code has not changed.
```

```text
The CI failure may be caused by a timeout or race condition.
```

```text
In mini_kv, network and concurrency tests should be checked for flaky behavior.
```

## CS explanation

Good tests should be **deterministic**.

That means:

```text
same input
same code
same environment
        ↓
same result
```

But some tests depend on unstable things:

```text
real time
network ports
thread scheduling
file system timing
external services
random values
```

For example, a TCP server test may fail if the client connects before the server is fully ready.

```text
start server
        ↓
client connects too early
        ↓
connection fails
        ↓
test fails randomly
```

A better test waits until the server is ready:

```text
start server
        ↓
wait for health check
        ↓
client connects
        ↓
test becomes more stable
```

The key difference is:

```text
real bug -> code behavior is wrong
flaky test -> test result is unstable
```

But flaky tests still matter. They reduce trust in CI because people cannot tell whether a red build means a real bug or just instability.

## Practice

Write 2 sentences about your CI using these words:

```text
flaky test
timeout
race condition
```

---

# Lesson 21: Race Condition

**Project:** `mini_kv` / `nodeproj`  
**Concept:** race condition

A **race condition** is a bug that happens when the result depends on the unpredictable timing or order of multiple operations.

In your projects, race conditions may appear in systems involving:

```text
threads
network requests
TCP clients
server startup
shared state
concurrent operations
```

For example, `mini_kv` has a thread-safe in-memory store and TCP server behavior, so concurrency safety matters.

## Simple idea

```text
Thread A reads value = 10
Thread B reads value = 10

Thread A adds 1 and writes 11
Thread B adds 1 and writes 11

expected result: 12
actual result: 11
```

Both threads used the old value, so one update was lost.

## Useful English

| Term | Meaning |
| --- | --- |
| race condition | a bug caused by unpredictable operation order |
| concurrency | multiple tasks running at the same time |
| thread-safe | safe to use from multiple threads |
| shared state | data used by multiple parts of a program |
| synchronization | controlling timing or access between tasks |

## Good English sentence patterns

```text
A race condition can happen when multiple threads access shared state.
```

```text
Thread-safe code uses synchronization to prevent inconsistent results.
```

```text
In mini_kv, concurrency control helps protect the key-value store from race conditions.
```

## CS explanation

Concurrency makes programs faster and more responsive, but it also introduces risk.

If two operations access the same data at the same time, the final result may depend on timing.

This is dangerous because the bug may not happen every time.

```text
Run 1: pass
Run 2: pass
Run 3: fail
Run 4: pass
```

That makes race conditions hard to debug.

Common ways to prevent race conditions include:

```text
mutex locks
atomic operations
message queues
single-writer design
database transactions
```

The key difference is:

```text
normal bug -> happens consistently
race condition -> may only happen under certain timing
```

In `mini_kv`, thread safety is important because multiple clients may access the key-value store. In `nodeproj`, race conditions can also appear in request handling, duplicate submissions, or in-memory ledgers.

## Practice

Write 2 sentences about your project using these words:

```text
race condition
shared state
thread-safe
```

---

# Lesson 22: Mutex

**Project:** `mini_kv`  
**Concept:** mutex

A **mutex** is a synchronization tool that allows only one thread to access shared data at a time.

The word **mutex** means **mutual exclusion**.

In `mini_kv`, a mutex is useful because multiple clients or threads may try to read or write the key-value store at the same time. Without protection, shared data may become inconsistent.

## Simple idea

```text
Thread A wants to update the store
        ↓
Thread A locks the mutex
        ↓
Thread A changes the data
        ↓
Thread A unlocks the mutex
        ↓
Thread B can now access the store
```

The mutex protects the critical section.

## Useful English

| Term | Meaning |
| --- | --- |
| mutex | a lock that allows one thread to access shared data at a time |
| lock | prevent other threads from entering a protected section |
| unlock | release the lock |
| critical section | code that accesses shared data and must be protected |
| mutual exclusion | only one thread can enter at a time |

## Good English sentence patterns

```text
A mutex protects shared state from concurrent access.
```

```text
The thread locks the mutex before entering the critical section.
```

```text
In mini_kv, mutexes help keep the key-value store thread-safe.
```

## CS explanation

A mutex helps prevent race conditions.

Imagine two threads modify the same value:

```text
Thread A: SET count 1
Thread B: SET count 2
```

If both threads access the store at the same time, the final state may become unpredictable.

A mutex makes the access orderly:

```text
one thread enters
other threads wait
first thread finishes
next thread enters
```

This protects shared data, but it also has a cost. If the mutex is held for too long, other threads must wait, and the system may become slower.

So good design tries to keep the critical section short.

The key difference is:

```text
without mutex -> faster but unsafe shared access
with mutex -> safer but may add waiting
```

In storage systems like `mini_kv`, mutexes are important because correctness is more important than randomly fast but unsafe behavior.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
mutex
critical section
thread-safe
```

---

# Lesson 23: Atomic Operation

**Project:** `mini_kv`  
**Concept:** atomic operation

An **atomic operation** is an operation that is completed as one indivisible unit.

That means it either finishes completely or does not happen at all. Other threads should not see a half-finished state.

In `mini_kv`, atomic behavior is important for write commands such as:

```text
SET
DEL
EXPIRE
SETNXEX
```

If a write operation changes the store and the WAL, the system should avoid a broken middle state.

## Simple idea

```text
not atomic:

write WAL record
        ↓
crash happens here
        ↓
store update is missing or inconsistent

atomic idea:

WAL append + store mutation
        ↓
treated as one safe operation
        ↓
system avoids half-finished state
```

## Useful English

| Term | Meaning |
| --- | --- |
| atomic operation | an operation that happens as one complete unit |
| indivisible | cannot be split into smaller visible steps |
| consistency | data stays correct and valid |
| partial update | an update that finishes only partly |
| all-or-nothing | either fully succeeds or fully fails |

## Good English sentence patterns

```text
An atomic operation prevents partial updates.
```

```text
The system should keep WAL append and store mutation consistent.
```

```text
In mini_kv, atomic write behavior helps protect data correctness.
```

## CS explanation

Atomicity is one of the most important ideas in storage systems and databases.

Imagine a write operation has two steps:

```text
1. write the change to WAL
2. update the in-memory store
```

If the system crashes between these two steps, the state may become confusing.

A well-designed system tries to make the operation safe, predictable, and recoverable.

Atomicity helps answer this question:

```text
Did the operation happen or not?
```

Without atomicity, the answer may be unclear:

```text
maybe the WAL changed
maybe the store changed
maybe only part of the operation finished
```

The key difference is:

```text
normal multi-step operation -> may expose partial state
atomic operation -> appears as one complete operation
```

In `mini_kv`, this matters because users expect commands like `SET` and `DEL` to behave reliably, even when WAL, recovery, and concurrency are involved.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
atomic operation
partial update
consistency
```

---

# Lesson 24: Schema Version

**Project:** `mini_kv` / `nodeproj`  
**Concept:** schema version

A **schema version** is a number or label that tells you which structure a data object follows.

In your projects, many JSON evidence records, runtime reports, and API responses may contain structured fields. A schema version helps other code understand how to read those fields safely.

## Simple idea

```json
{
  "schema_version": 1,
  "status": "ok"
}
```

```json
{
  "schema_version": 2,
  "status": "ok",
  "diagnostics": []
}
```

The schema version tells the reader which format to expect.

## Useful English

| Term | Meaning |
| --- | --- |
| schema | the structure or shape of data |
| schema version | a version number for a data format |
| field | one named piece of data |
| compatible | able to work together |
| migration | changing data from one format to another |

## Good English sentence patterns

```text
A schema version helps clients understand the structure of a response.
```

```text
The system can add new fields while keeping old clients compatible.
```

```text
In mini_kv, schema versions make runtime evidence easier to parse safely.
```

## CS explanation

When a project grows, its data format may change.

At first, a response may only include:

```json
{
  "status": "ok"
}
```

Later, you may add more fields:

```json
{
  "status": "ok",
  "warnings": [],
  "diagnostics": [],
  "digest": "abc123"
}
```

This is useful, but it can break older clients if they expect the old format.

A schema version helps the client decide:

```text
If schema_version is 1, read the old format.
If schema_version is 2, read the new format.
```

The key difference is:

```text
data fields -> actual information
schema version -> tells which format the information follows
```

In systems like `mini_kv` and `nodeproj`, schema versions are important because evidence records must be machine-readable and stable over time.

## Practice

Write 2 sentences about your project using these words:

```text
schema version
field
compatible
```
