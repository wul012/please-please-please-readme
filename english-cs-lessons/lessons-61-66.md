# English + Computer Science Lessons 61-66

This document continues the English + CS study notes based on my four main projects:

- `aiproj`
- `nodeproj`
- `mini_kv`
- `javaproject`

These lessons focus on general software engineering concepts that appear across backend systems, storage engines, AI projects, and control planes.

---

# Lesson 61: Layered Architecture

**Project:** `javaproject` / `nodeproj`  
**Concept:** layered architecture

**Layered architecture** means organizing a system into clear layers, where each layer has a different responsibility.

In `javaproject`, controllers, services, builders, repositories, and tests form different layers. In `nodeproj`, routes, services, renderers, types, and dashboard code also form layers.

## Simple idea

```text
API layer
    ->
service layer
    ->
domain or evidence logic
    ->
storage or external dependency
```

## Useful English

| Term | Meaning |
| --- | --- |
| layer | one level of a system |
| responsibility | what a component should do |
| separation | keeping things apart |
| dependency | something a component needs |
| architecture | the high-level structure of a system |

## Good English sentence patterns

```text
Layered architecture separates API code from business logic.
```

```text
Each layer should have a clear responsibility.
```

```text
In javaproject, service and builder layers make release approval logic easier to maintain.
```

## CS explanation

Layering helps prevent one file or class from doing everything.

For example, a controller should not also contain database logic, rendering logic, and complex validation logic.

Good layering makes the project easier to test, refactor, and explain.

## Practice

Write 2 sentences about `javaproject` using these words:

```text
layer
responsibility
architecture
```

---

# Lesson 62: Event-Driven Design

**Project:** `javaproject` / `nodeproj`  
**Concept:** event-driven design

**Event-driven design** means the system reacts to events instead of only calling functions directly.

In backend systems, events can represent things like order creation, approval decisions, notification failures, or audit records.

## Simple idea

```text
something happens
    ->
event is created
    ->
handler reacts
    ->
system records or publishes result
```

## Useful English

| Term | Meaning |
| --- | --- |
| event | a record that something happened |
| handler | code that responds to an event |
| publish | send an event to another system |
| subscribe | listen for events |
| asynchronous | not happening at the same exact time |

## Good English sentence patterns

```text
Event-driven design makes the system react to important state changes.
```

```text
An event handler should process one kind of event clearly.
```

```text
In javaproject, order and approval events can be handled asynchronously.
```

## CS explanation

Event-driven systems are useful when one action should trigger other work.

For example, after an order is created, the system may publish an event so another component can send a notification.

This reduces tight coupling between components.

## Practice

Write 2 sentences about `javaproject` using these words:

```text
event
handler
asynchronous
```

---

# Lesson 63: Transaction Boundary

**Project:** `javaproject` / `mini_kv`  
**Concept:** transaction boundary

A **transaction boundary** defines which operations belong to one safe unit of work.

In `javaproject`, transaction boundaries protect order and approval updates. In `mini_kv`, WAL and mutation logic help protect storage changes.

## Simple idea

```text
start transaction
    ->
perform related changes
    ->
commit if all succeed
    ->
rollback if something fails
```

## Useful English

| Term | Meaning |
| --- | --- |
| transaction | a safe unit of work |
| commit | save the changes |
| rollback | undo the changes |
| boundary | the limit of a unit |
| consistency | data stays valid |

## Good English sentence patterns

```text
A transaction boundary keeps related changes consistent.
```

```text
If one step fails, the system should roll back the transaction.
```

```text
In javaproject, transaction boundaries protect order state changes.
```

## CS explanation

Transactions are important because many operations have multiple steps.

Without a transaction boundary, one step may succeed while another step fails.

That can leave the system in a confusing state.

## Practice

Write 2 sentences about `javaproject` using these words:

```text
transaction
rollback
consistency
```

---

# Lesson 64: Index

**Project:** `mini_kv` / `aiproj`  
**Concept:** index

An **index** is a structure that helps find information faster.

In storage systems, indexes help find records. In `aiproj`, index files can help find training runs, promotion records, or benchmark history entries.

## Simple idea

```text
without index:
    scan everything

with index:
    look up the key
    jump to the result
```

## Useful English

| Term | Meaning |
| --- | --- |
| index | a structure for faster lookup |
| lookup | search for a value |
| scan | check many items one by one |
| key | value used to find data |
| query | request for information |

## Good English sentence patterns

```text
An index makes lookups faster.
```

```text
Without an index, the system may need to scan many records.
```

```text
In aiproj, promotion indexes make training results easier to find.
```

## CS explanation

Indexes trade extra storage for faster queries.

They are common in databases, search engines, file systems, and reporting tools.

The basic idea is to prepare a faster path for finding data.

## Practice

Write 2 sentences about `aiproj` using these words:

```text
index
lookup
query
```

---

# Lesson 65: Cache

**Project:** `nodeproj` / `aiproj`  
**Concept:** cache

A **cache** stores a result so the system can reuse it later instead of recomputing or refetching it.

In `nodeproj`, cached evidence or keys can improve performance. In `aiproj`, cached artifacts can make repeated reporting faster.

## Simple idea

```text
first request:
    compute result
    store in cache

next request:
    read from cache
    return faster
```

## Useful English

| Term | Meaning |
| --- | --- |
| cache | temporary storage for reuse |
| hit | cache contains the needed value |
| miss | cache does not contain the needed value |
| expire | become invalid after time |
| invalidate | remove or mark cached data as old |

## Good English sentence patterns

```text
A cache can reduce repeated work.
```

```text
Cache invalidation is important when data changes.
```

```text
In nodeproj, cached metadata must not hide stale safety information.
```

## CS explanation

Caching improves speed, but it adds correctness risk.

If cached data is old, the system may make decisions from stale information.

That is why cache invalidation and expiration rules matter.

## Practice

Write 2 sentences about `nodeproj` using these words:

```text
cache
expire
stale
```

---

# Lesson 66: Serialization

**Project:** `mini_kv` / `nodeproj` / `aiproj`  
**Concept:** serialization

**Serialization** means converting data into a format that can be stored or transmitted.

JSON, CSV, Markdown, and binary formats are all examples of serialized data.

## Simple idea

```text
in-memory object
    ->
serialize to JSON
    ->
write to file or send over network
    ->
later deserialize back into object
```

## Useful English

| Term | Meaning |
| --- | --- |
| serialization | converting data to a storable or sendable format |
| deserialize | read serialized data back into objects |
| format | the shape or representation of data |
| encode | convert into a format |
| parse | read and understand a format |

## Good English sentence patterns

```text
Serialization makes structured data easier to store and transmit.
```

```text
The system parses JSON evidence before verifying it.
```

```text
In mini_kv, runtime evidence is serialized into JSON responses.
```

## CS explanation

Programs often use rich objects in memory, but files and network messages need bytes or text.

Serialization bridges that gap.

It is used in APIs, databases, logs, evidence records, and model artifacts.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
serialization
format
parse
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
In aiproj, an index helps the system find promotion records faster.
```
