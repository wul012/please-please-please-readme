# English + Computer Science Lessons 13-18

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

# Lesson 13: Command Dispatch Table

**Project:** `mini_kv`  
**Concept:** command dispatch table

A **command dispatch table** is a table that maps command names to the code that should handle them.

In `mini_kv`, the v106 update changes command routing from a long `if` chain into a dispatch table plus an enum-based `switch`. This makes command handling easier to read, audit, and extend.

## Simple idea

```text
client command:
SET key value
    ↓
dispatch table looks up "SET"
    ↓
system finds CommandDispatchVerb::Set
    ↓
switch runs the SET handler
```

Instead of checking many conditions one by one:

```text
if command == "PING"
if command == "SET"
if command == "GET"
if command == "DEL"
...
```

The system uses a clearer table:

```text
"PING" -> Ping handler
"SET"  -> Set handler
"GET"  -> Get handler
"DEL"  -> Del handler
```

## Useful English

| Term | Meaning |
| --- | --- |
| dispatch | send work to the correct handler |
| command | an instruction given to a program |
| handler | code that processes a specific request |
| table-driven | controlled by a table instead of many conditions |
| routing | deciding where a request should go |

## Good English sentence patterns

```text
The dispatch table maps each command to the correct handler.
```

```text
Table-driven command routing makes the code easier to audit.
```

```text
In mini_kv, the command dispatch table replaces a long if-chain.
```

## CS explanation

When a program supports many commands, command routing can become messy.

For example, a key-value server may support:

```text
PING
SET
GET
DEL
EXPIRE
TTL
KEYS
INFOJSON
STATSJSON
HEALTH
```

If every command is handled by a long `if` chain, the code becomes harder to scan and maintain.

A dispatch table improves this by putting the command list in one clear place.

This helps developers answer:

```text
Which commands are supported?
Which handler does each command use?
Is a command read-only or write-related?
Where should I add a new command?
```

The key difference is:

```text
long if-chain -> logic is spread through many conditions
dispatch table -> command mapping is centralized
```

This pattern is common in interpreters, servers, CLI tools, databases, and protocol parsers.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
dispatch table
command handler
routing
```

---

# Lesson 14: Tokenization

**Project:** `aiproj`  
**Concept:** tokenization

**Tokenization** means splitting text into smaller units called **tokens** before sending it into a language model.

In `aiproj`, tokenization is important because MiniGPT cannot directly understand raw text. It first converts text into tokens, then converts tokens into token IDs that the model can process.

## Simple idea

```text
raw text:
I love machine learning

        ↓

tokens:
["I", "love", "machine", "learning"]

        ↓

token IDs:
[12, 45, 301, 982]

        ↓

model input
```

The model does not really see words. It sees numbers that represent tokens.

## Useful English

| Term | Meaning |
| --- | --- |
| tokenization | splitting text into tokens |
| token | a small unit of text |
| tokenizer | a tool that converts text into tokens |
| vocabulary | the set of tokens a model knows |
| token ID | a number representing a token |

## Good English sentence patterns

```text
Tokenization converts raw text into model-readable units.
```

```text
The tokenizer maps each token to a token ID.
```

```text
In aiproj, tokenization is the first step before training the MiniGPT model.
```

## CS explanation

A language model needs numbers as input, not raw text.

So before training or generation, the text must be converted:

```text
text -> tokens -> token IDs -> embeddings -> model
```

Different tokenizers may split text differently.

For example:

```text
"unbelievable"
```

could become:

```text
["un", "believable"]
```

or:

```text
["un", "believ", "able"]
```

or even:

```text
["u", "n", "b", "e", ...]
```

The tokenizer affects:

```text
model vocabulary
training speed
memory usage
generation quality
handling of Chinese or English text
```

In MiniGPT-style projects, tokenization is one of the first important design choices.

The key difference is:

```text
raw text -> easy for humans
token IDs -> usable by the model
```

## Practice

Write 2 sentences about `aiproj` using these words:

```text
tokenization
tokenizer
token ID
```

---

# Lesson 15: Upstream Service

**Project:** `nodeproj`  
**Concept:** upstream service

An **upstream service** is another service that your system depends on or calls.

In `nodeproj`, the Node service depends on two important upstream services:

```text
Java order platform
C++ mini_kv service
```

The Node project acts as the control plane, while Java and `mini_kv` provide important backend capabilities.

## Simple idea

```text
Node control plane
        ↓
calls upstream service
        ↓
Java order platform

Node control plane
        ↓
calls upstream service
        ↓
mini_kv server
```

If an upstream service is unavailable, the Node service must handle that situation safely.

## Useful English

| Term | Meaning |
| --- | --- |
| upstream service | a service that another system depends on |
| dependency | something your system needs |
| unavailable | not working or not reachable |
| fallback | a backup behavior when something fails |
| integration | connecting multiple systems together |

## Good English sentence patterns

```text
The Node service depends on Java and mini_kv as upstream services.
```

```text
If an upstream service is unavailable, the control plane should fail safely.
```

```text
In nodeproj, upstream integration is protected by safety checks and dry-run workflows.
```

## CS explanation

Modern software systems are often made of many services.

One service may not do everything itself. Instead, it may call other services:

```text
frontend -> backend API -> database
Node control plane -> Java service -> order logic
Node control plane -> mini_kv -> storage/runtime evidence
```

This creates a dependency.

If the upstream service is healthy, the request can continue.

If the upstream service fails, the caller must decide what to do:

```text
return an error
retry later
use cached data
show degraded status
block dangerous actions
```

In `nodeproj`, this is very important because the Node service should not blindly trust or call upstream systems. It should check status, collect evidence, and keep safety boundaries.

The key difference is:

```text
local logic -> code inside the current service
upstream service -> another service called by the current service
```

Good backend systems must handle upstream failure carefully.

## Practice

Write 2 sentences about `nodeproj` using these words:

```text
upstream service
dependency
fallback
```

---

# Lesson 16: API Contract

**Project:** `javaproject`  
**Concept:** API contract

An **API contract** is the agreement between a service and its users about how the API should behave.

It usually includes:

```text
request format
response format
field names
HTTP status codes
headers
error messages
```

In `javaproject`, API contracts are important because release approval and rehearsal responses must stay stable even when the internal Java code is refactored.

## Simple idea

```text
client sends request
        ↓
Java API receives request
        ↓
Java API returns response

API contract says:
- what request fields are expected
- what response fields are returned
- what behavior should stay stable
```

If the contract changes unexpectedly, other services may break.

## Useful English

| Term | Meaning |
| --- | --- |
| API contract | the expected behavior and shape of an API |
| request | data sent to an API |
| response | data returned by an API |
| stable | not changing unexpectedly |
| backward compatible | new changes still work with old clients |

## Good English sentence patterns

```text
The API contract defines how clients should call the service.
```

```text
A stable response format helps other services depend on the API safely.
```

```text
In javaproject, refactoring must preserve the API contract.
```

## CS explanation

In backend development, an API is not only code. It is also a promise.

For example, if an API returns this field:

```json
{
  "status": "approved"
}
```

other services may depend on the field name `status`.

If you suddenly change it to:

```json
{
  "decisionStatus": "approved"
}
```

your own code may still compile, but other services may fail.

That is why API contracts must be protected during refactoring.

A good backend project asks:

```text
Did the response shape change?
Did the header behavior change?
Did the digest calculation change?
Did the error format change?
Will old clients still work?
```

The key difference is:

```text
internal implementation -> can change during refactoring
API contract -> should remain stable for clients
```

This is why `javaproject` often emphasizes contract-preserving refactoring.

## Practice

Write 2 sentences about `javaproject` using these words:

```text
API contract
response format
backward compatible
```

---

# Lesson 17: RESP Protocol

**Project:** `mini_kv`  
**Concept:** RESP protocol

**RESP** means **REdis Serialization Protocol**. It is a simple protocol used by Redis clients and servers to communicate.

In `mini_kv`, RESP support allows the server to understand Redis-style client requests, not only plain text commands.

## Simple idea

```text
plain text command:
SET name Alice

RESP command:
*3
$3
SET
$4
name
$5
Alice
```

Both mean:

```text
set key "name" to value "Alice"
```

But RESP has a structured format that is easier for clients and servers to parse reliably.

## Useful English

| Term | Meaning |
| --- | --- |
| protocol | rules for communication between systems |
| serialization | converting data into a format that can be sent or stored |
| parser | code that reads and understands structured input |
| client | the program sending requests |
| server | the program receiving and handling requests |

## Good English sentence patterns

```text
RESP is a protocol that defines how Redis clients communicate with servers.
```

```text
The parser converts RESP input into commands that the server can execute.
```

```text
In mini_kv, RESP support improves compatibility with Redis-style clients.
```

## CS explanation

A protocol is like a shared language between programs.

If the client and server do not agree on the format, they cannot communicate correctly.

For example, a server needs to know:

```text
Where does the command start?
How many arguments are there?
How long is each argument?
Is this input valid or invalid?
```

RESP solves this by encoding commands in a structured way.

For example:

```text
*3
```

means there are **3 parts** in the command.

```text
$3
SET
```

means the next string has length **3**, and the string is `SET`.

This helps the server avoid guessing where each argument begins or ends.

The key difference is:

```text
plain text command -> easy for humans to type
RESP command -> easier for programs to parse safely
```

This is important for databases, network services, protocol parsers, and client-server systems.

## Practice

Write 2 sentences about `mini_kv` using these words:

```text
RESP protocol
parser
client-server communication
```

---

# Lesson 18: Model Checkpoint

**Project:** `aiproj`  
**Concept:** model checkpoint

A **model checkpoint** is a saved version of a model’s parameters during or after training.

In `aiproj`, checkpoints are important because the MiniGPT model can be trained, saved, loaded, evaluated, compared, and used for generation later.

## Simple idea

```text
training starts
    ↓
model learns from data
    ↓
save checkpoint
    ↓
continue training or evaluate model
    ↓
load checkpoint later for generation
```

A checkpoint lets you keep the model’s learned state instead of starting from zero every time.

## Useful English

| Term | Meaning |
| --- | --- |
| checkpoint | a saved model state |
| parameter | a learned number inside a model |
| load | read saved data back into the program |
| resume training | continue training from a saved point |
| evaluate | test how well the model performs |

## Good English sentence patterns

```text
A checkpoint saves the model’s learned parameters.
```

```text
The system can load a checkpoint to resume training or run evaluation.
```

```text
In aiproj, checkpoints make model comparison and generation easier.
```

## CS explanation

Training a model can take time. If you do not save checkpoints, you may lose all progress when the program stops.

A checkpoint usually stores things like:

```text
model weights
optimizer state
training step
configuration
tokenizer information
```

After saving a checkpoint, you can use it for different purposes:

```text
resume training
generate text
evaluate performance
compare two model versions
archive experiment evidence
```

This is especially important in AI projects because one model version may be better than another. You need saved checkpoints to compare them fairly.

The key difference is:

```text
training script -> creates or updates the model
checkpoint -> saves the model state for later use
```

Without checkpoints, model development is hard to reproduce.

With checkpoints, the project can build an experiment history.

## Practice

Write 2 sentences about `aiproj` using these words:

```text
checkpoint
load
evaluate
```
