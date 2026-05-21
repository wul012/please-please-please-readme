# English + Computer Science Lessons 31-36

This document continues the English + CS study notes based on my four main projects:

- `aiproj`
- `nodeproj`
- `mini_kv`
- `javaproject`

These lessons focus on engineering governance, safety, maintainability, and distributed-system preparation.

---

# Lesson 31: Credential Resolver

**Project:** `nodeproj` / `javaproject` / `mini_kv`  
**Concept:** credential resolver

A **credential resolver** is a component that decides how credentials should be found or used.

In a safe system, a credential resolver should not automatically read production secrets during a dry run. It may only record a decision, a plan, or a non-participation receipt.

## Simple idea

```text
operation needs credential context
    ->
resolver checks policy
    ->
resolver records decision
    ->
real secret access stays blocked
```

## Useful English

| Term | Meaning |
| --- | --- |
| credential | information used for authentication |
| resolver | a component that finds or decides something |
| secret | sensitive information such as a token or password |
| decision record | a record explaining a decision |
| access control | rules that decide who can access what |

## Good English sentence patterns

```text
The credential resolver records a decision without reading production secrets.
```

```text
Credential resolution must respect access-control boundaries.
```

```text
In nodeproj, credential resolver records help explain why secret access stays disabled.
```

## CS explanation

Credentials are sensitive.

A careless system may turn a planning step into real secret access. A safer system separates:

```text
credential decision
credential plan
credential readiness
real credential access
```

That separation protects production systems.

## Practice

Write 2 sentences about `nodeproj` using these words:

```text
credential resolver
secret
decision record
```

---

# Lesson 32: Remediation Gate

**Project:** `aiproj`  
**Concept:** remediation gate

A **remediation gate** is a quality gate that blocks progress until problems are fixed.

In `aiproj`, remediation gates help prevent weak benchmark, scorecard, or readiness results from being treated as acceptable.

## Simple idea

```text
run quality check
    ->
find issues
    ->
gate blocks promotion
    ->
developer fixes issues
    ->
run check again
```

## Useful English

| Term | Meaning |
| --- | --- |
| remediation | fixing a problem |
| gate | a condition that must pass before moving forward |
| issue | a problem that needs attention |
| blocker | a problem that stops progress |
| promotion | moving a model or artifact to a better status |

## Good English sentence patterns

```text
The remediation gate prevents unsafe promotion.
```

```text
The system exposes issue text so the developer knows what to fix.
```

```text
In aiproj, remediation gates make model evaluation more disciplined.
```

## CS explanation

A quality gate is useful only if it can explain failure.

Bad gate:

```text
failed
```

Better gate:

```text
failed because benchmark history is missing
failed because scorecard evidence is incomplete
failed because the plan digest is inconsistent
```

This makes the system easier to debug and improve.

## Practice

Write 2 sentences about `aiproj` using these words:

```text
remediation gate
issue
promotion
```

---

# Lesson 33: Batch Review

**Project:** `aiproj`  
**Concept:** batch review

A **batch review** is a review of multiple runs, results, or candidates together.

In `aiproj`, batch review helps compare training runs, promotion candidates, portfolio results, and benchmark summaries.

## Simple idea

```text
many training runs
    ->
batch comparison
    ->
review summary
    ->
decision or promotion
```

## Useful English

| Term | Meaning |
| --- | --- |
| batch | a group processed together |
| review | careful checking or evaluation |
| candidate | one possible option |
| comparison | looking at differences |
| summary | a short explanation of results |

## Good English sentence patterns

```text
Batch review helps compare multiple candidates at the same time.
```

```text
The review summary should propagate into the promotion decision.
```

```text
In aiproj, batch review connects training results with release decisions.
```

## CS explanation

Single results can be misleading.

A batch review lets you compare several candidates under the same rules.

This is useful when you need to decide:

```text
Which run is better?
Which model should be promoted?
Which result is risky?
Which evidence is missing?
```

## Practice

Write 2 sentences about `aiproj` using these words:

```text
batch review
candidate
comparison
```

---

# Lesson 34: Module Split

**Project:** `nodeproj` / `aiproj` / `javaproject` / `mini_kv`  
**Concept:** module split

A **module split** means moving a large file or mixed responsibility into smaller, clearer files.

Your projects often split large builders, renderers, formatters, routes, tests, and artifact helpers into smaller modules.

## Simple idea

```text
one huge file
    ->
hard to read
    ->
split into focused modules
    ->
easier to test and maintain
```

## Useful English

| Term | Meaning |
| --- | --- |
| module | a file or unit of code with a clear role |
| split | divide into smaller parts |
| responsibility | what a piece of code is supposed to do |
| maintainability | how easy code is to maintain |
| refactor | improve code structure without changing behavior |

## Good English sentence patterns

```text
The module split improves maintainability without changing the public contract.
```

```text
Large files become easier to test after responsibilities are separated.
```

```text
In nodeproj, renderer and type splits reduce the risk of large-file decay.
```

## CS explanation

Large files are not always bad, but they become risky when they mix many responsibilities.

For example:

```text
types
rendering
validation
business logic
route registration
test fixtures
```

If all of these live in one file, changes become harder to understand.

## Practice

Write 2 sentences about your project using these words:

```text
module split
responsibility
maintainability
```

---

# Lesson 35: Preflight Gate

**Project:** `nodeproj` / `javaproject`  
**Concept:** preflight gate

A **preflight gate** is a check that must pass before a more important operation can start.

In `nodeproj`, preflight gates help decide whether a sandbox connection packet is ready for review or rehearsal.

## Simple idea

```text
prepare packet
    ->
run preflight gate
    ->
if safe, continue
    ->
if unsafe, stop and report issues
```

## Useful English

| Term | Meaning |
| --- | --- |
| preflight | a check before starting |
| gate | a condition that controls progress |
| packet | a structured group of data |
| validation | checking whether something is valid |
| reject | refuse to accept or continue |

## Good English sentence patterns

```text
The preflight gate rejects incomplete packets.
```

```text
A good preflight check explains why the operation is blocked.
```

```text
In nodeproj, preflight gates help keep sandbox connections safe.
```

## CS explanation

Preflight checks are common in aviation, deployment, networking, and distributed systems.

They reduce risk by checking requirements before the real action starts.

Without preflight:

```text
start operation
    ->
discover missing context too late
```

With preflight:

```text
check context first
    ->
start only if safe
```

## Practice

Write 2 sentences about `nodeproj` using these words:

```text
preflight gate
packet
validation
```

---

# Lesson 36: Sharding Preparation

**Project:** `mini_kv` / `nodeproj`  
**Concept:** sharding preparation

**Sharding preparation** means building the engineering foundations needed before a system can safely split data across multiple nodes.

Your projects have not implemented real sharding yet, but they are building useful preparation: command contracts, adapter boundaries, evidence records, dry runs, and upstream verification.

## Simple idea

```text
before sharding:
    command contracts
    adapter boundaries
    readiness checks
    evidence records
    failure classification

after that:
    shard map
    routing
    replicas
    rebalancing
```

## Useful English

| Term | Meaning |
| --- | --- |
| sharding | splitting data across multiple nodes |
| preparation | work done before the main implementation |
| routing | deciding where a request should go |
| replica | a copy of data or service |
| rebalance | moving data to balance load |

## Good English sentence patterns

```text
The project is preparing for sharding, but it has not implemented shard routing yet.
```

```text
Adapter boundaries and evidence records are useful before multi-node routing.
```

```text
In mini_kv, command contracts make future sharding easier to design.
```

## CS explanation

Sharding is more than a hash function.

A real sharded system needs:

```text
which shard owns the key
how requests are routed
how replicas are checked
how failures are classified
how migrations are verified
how evidence is recorded
```

That is why preparation matters.

## Practice

Write 2 sentences about `mini_kv` or `nodeproj` using these words:

```text
sharding
routing
preparation
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
In mini_kv, sharding preparation helps the project separate contracts before routing keys across nodes.
```
