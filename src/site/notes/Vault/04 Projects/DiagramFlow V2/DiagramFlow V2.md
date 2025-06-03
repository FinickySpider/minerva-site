---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/diagram-flow-v2/","tags":["gardenEntry"]}
---



# DiagramFlow v2 Documentation

Welcome to **DiagramFlow v2**, a plain‑text language for describing *any* architectural or logic flow with absolute clarity and zero visual noise.  
This documentation is divided into standalone pages—one per concept—so you can jump straight to the piece you need.

## Why DiagramFlow?
* **Human‑first clarity:** Perfect for pasting into LLM chats or code reviews.  
* **Expressive:** Covers sequential, parallel, async, error, event, guard, metadata, side‑effect, and sub‑flow patterns.  
* **Lightweight:** No external tooling required.  
* **Composable:** Flows can reference other flows, enabling large systems to be modelled in layers.

## Getting Started
1. Open [[#Syntax Overview]] for a five‑minute crash course.  
2. Browse [[#NODE Types]], [[#Edge Operators]], and [[#Attributes and Metadata]] for deep dives.  
3. Scan [[#Canonical Patterns]] for ready‑made flow templates.  
4. Copy‑paste snippets from [[#End‑to‑End Examples]] to kick‑start your own docs.

## Directory / Page Index
| Page                                                    | Purpose                                                       |
| ------------------------------------------------------- | ------------------------------------------------------------- |
| [[Vault/04 Projects/DiagramFlow V2/Original/syntax_overview\|syntax_overview]]                                     | All core grammar in one spot                                  |
| [[Vault/04 Projects/DiagramFlow V2/Original/nodes\|nodes]]                                               | Every **NODE** type with rules and examples                   |
| [[Vault/04 Projects/DiagramFlow V2/Original/edges\|edges]]                                               | Every edge operator (`->`, `<->`, etc) fully documented       |
| [[Vault/04 Projects/DiagramFlow V2/Original/attributes\|attributes]]                                          | Metadata keywords like `DESC`, `ROLE`, `DURATION`             |
| [[Vault/04 Projects/DiagramFlow V2/Original/patterns\|patterns]]                                            | Canonical design patterns: loops, forks, merges, retries, etc |
| [[Vault/04 Projects/DiagramFlow V2/Original/examples\|examples]] | End‑to‑end samples you can copy                               |
| [[Vault/04 Projects/DiagramFlow V2/Original/glossary\|glossary]]                                            | Rapid lookup for any keyword or acronym                       |


---


# Syntax Overview

This page gives you the *minimum viable knowledge* to read or write DiagramFlow in five minutes.

## Anatomy of a File
```txt
// comments begin with //
NODE Login       TYPE: Input   DESC: "User enters email"
NODE Validate    TYPE: Logic   CONDITION: "RFC‑5322"
Login -> Validate
```

1. **Comments** start with `//`.
2. NODE `<Identifier>` declares a node.
3. **Edge operators** (`->`, `<->`, etc) describe flow.
4. **Attributes** (`TYPE:`, `DESC:`, etc) extend the line with metadata.

## Key Building Blocks

| Element  | Purpose                         | Mandatory? |
|----------|---------------------------------|------------|
| `NODE`   | Declare a graph vertex          | Yes |
| Edge op  | Connect two nodes               | Yes for flow |
| Attribute| Qualify a node or edge          | No |

## Quick Edge Reference

| Operator | Meaning |
|----------|---------|
| `->`     | One‑way sequential flow |
| `<->`    | Bidirectional exchange |
| `-/>`    | Error or exception route |
| `~>`     | Async, non‑blocking call |
| `=>`     | Fork to multiple branches |
| `==`     | Parallel merge (wait‑all) |
| `^>`     | Triggered event edge |
| `*->`    | Loop / repeat |

## Example Mini‑Flow

```txt
NODE Start      TYPE: Start
NODE InputCreds TYPE: Input
NODE Auth       TYPE: Process
Start -> InputCreds -> Auth
```



---

# NODE Types

Every flow is built from NODES. Each node line follows:

```txt
NODE <Identifier>  TYPE: <NodeType>  [KEY: value ...]
```

Below, each `TYPE` gets its own section with **Purpose**, **Usage**, **Do / Do Not**, and **Examples**.

---

## Start
* **Purpose:** Single entry point of a flow.
* **Usage:** Exactly one per top‑level file.
* **Do:** Keep it simple, link it to the first real step.
* **Do Not:** Add business logic in the Start node.

```txt
NODE Start TYPE: Start DESC: "User session begins"
```

---

## End
*Ends terminate flows.* Multiple End nodes are allowed.

```txt
NODE EndSuccess TYPE: End DESC: "Everything OK"
NODE EndFail    TYPE: End DESC: "Unrecoverable error"
```

---

## Process
Performs deterministic work.

| Good | Bad |
|------|-----|
| `AuthUser`, `HashPassword` | `DoThings`, `Process1` |

```txt
NODE AuthUser TYPE: Process DESC: "Validate credentials"
```

---

## Decision
Evaluates a Boolean or multi‑branch condition.

```txt
NODE IsValid? TYPE: Decision CONDITION: "Email format passes?"
Auth -> IsValid? [yes] -> Continue
Auth -> IsValid? [no]  -> Error
```

---

## Logic
Encapsulates complex or multi‑step rules but returns a single outcome.

```txt
NODE CalcTax TYPE: Logic DESC: "Jurisdictional tax math"
```

---

## State
Represents a passive UI or system state.

```txt
NODE Dashboard TYPE: State
```

---

## Input / Output
Human or external data entry / exit.

```txt
NODE UploadForm TYPE: Input
NODE ExportCSV  TYPE: Output
```

---

## External
Calls to services you do **not** own.

```txt
NODE StripeAPI TYPE: External ROLE: "Payment"
```

---

## Fork / Merge
*Fork* splits, *Merge* joins.

```txt
NODE F TYPE: Fork
NODE J TYPE: Merge
F => [StepA, StepB]
StepA -> J
StepB -> J
```

---

## Parallel
Waits for *all* inbound branches (shorthand for Merge ==).

```txt
NODE SyncAll TYPE: Parallel
Branch1 -> SyncAll
Branch2 -> SyncAll
```

---

## Event / Timer / Guard / Error / SideEffect / SubFlow / Data / Action
Refer to [[#Glossary]] for fast lookup plus examples for each minor type.



# Attributes and Metadata

Attributes qualify nodes or edges. They follow the pattern `KEY: value`.

| Attribute | Applies To | Meaning |
|-----------|------------|---------|
| `TYPE:`       | Node  | Declares node category |
| `DESC:`       | Node  | Human description |
| `CONDITION:`  | Node (Decision) | Underlying logic in prose or pseudo code |
| `ROLE:`       | Node | Responsible service / actor |
| `TRIGGER:`    | Node (Event) | What sets the event off |
| `DURATION:`   | Node (Timer) | Timeout length (`"30s"`, `"5m"`) |
| `INIT:`       | Node (Data) | Initial value |
| `REF:`        | Node (SubFlow) | File or flow name |
| `INIT:`       | Edge loop     | Starting counter |
| `{...}`       | Edge | Side‑effect or annotation |

## Good Practices
* Keep values **short**; move long prose into comments.  
* Prefer ISO‑8601 durations (`PT30S`) if you need machine precision.  
* Use `ROLE:` to clarify microservice boundaries.

## Example
```txt
NODE RateLimiter TYPE: Guard CONDITION: "Max 5 req/min" ROLE: "API‑Gateway"
```



# Edge Operators

Edges connect nodes and define runtime behaviour.

| Operator | Name            | Typical Use           |
| -------- | --------------- | --------------------- |
| `->`     | Sequence        | Standard step         |
| `<->`    | Bidirectional   | Hand‑shake protocols  |
| `-/>`    | Error           | Exception or rollback |
| `~>`     | Async           | Fire‑and‑forget call  |
| `=>`     | Fork (fan‑out)  | Parallel branches     |
| `==`     | Merge (fan‑in)  | Wait‑all join         |
| `^>`     | Trigger / Event | Reactive edge         |
| `*->`    | Loop / Retry    | Iteration             |

## Syntax
```txt
<FromNode> [Condition] {Event} OPERATOR <ToNode>
```

### Condition `[ ... ]`
Branches by value, e.g. `[yes]`, `[> 3]`, `[timeout]`.

### Event / Effect `{ ... }`
Logs, metrics, hooks, e.g. `{onFail}`, `{emit:OrderPaid}`.

## Examples

### Error Path
```txt
PayInvoice -/> ErrorPanel {onPaymentError}
```

### Fork and Join
```txt
Fork1 => [FetchUser, FetchOrders]
FetchUser -> Join ==
FetchOrders -> Join ==
```

### Loop with Guard
```txt
Counter *-> RetryAuth [< 5]
```

### Async Call
```txt
SendEmail ~> MailServer
```

**Do Not** mix `=>` and `==` edges on the same line; keep them explicit for readability.



# Canonical Patterns

Cut‑paste starting points for common scenarios.

## Retry with Back‑off
```txt
NODE Attempt  TYPE: Process
NODE Delay    TYPE: Timer DURATION: "30s"
NODE Counter  TYPE: Data INIT: 0
Attempt  [fail] -/> Delay
Counter *-> Attempt [< 3] {onRetry}
```

## Parallel Micro‑services
```txt
NODE Forker   TYPE: Fork
NODE MergeAll TYPE: Parallel
Forker => [Auth, Billing, Notify]
Auth    -> MergeAll
Billing -> MergeAll
Notify  -> MergeAll
```

## Two‑factor Authentication SubFlow
```txt
NODE SubFlow2FA TYPE: SubFlow REF: "TwoFactorFlow"
```
Open [[#End‑to‑End Examples]] to see this pattern fully expanded.

## Saga / Compensation
```txt
NODE BookTrip      TYPE: Process
NODE CancelTrip    TYPE: Process
BookTrip -/> CancelTrip {compensate}
```

*See also*: Async Messaging, Timeout Escalation, Circuit Breaker in this same file.




# End‑to‑End Examples

## 1. Basic Web Login
```txt
NODE Start       TYPE: Start
NODE LoginForm   TYPE: Input
NODE Auth        TYPE: Process
NODE IsActive?   TYPE: Decision CONDITION: "Active?"
NODE Dashboard   TYPE: State
NODE ErrorPage   TYPE: Error
Start -> LoginForm -> Auth
Auth -> IsActive?
IsActive? [yes] -> Dashboard
IsActive? [no]  -> ErrorPage
```

## 2. Payment with Retry and Timeout
```txt
NODE PayAPI       TYPE: External
NODE RetryTimer   TYPE: Timer DURATION: "15s"
NODE Counter      TYPE: Data INIT: 0
PayAPI [success] -> End
PayAPI [fail] -/> RetryTimer
Counter *-> PayAPI [< 3]
RetryTimer -> PayAPI
```

## 3. Distributed Order Saga
```txt
NODE CreateOrder     TYPE: Process
NODE ChargePayment   TYPE: External
NODE ReserveStock    TYPE: External
NODE ShipGoods       TYPE: External
NODE CancelOrder     TYPE: Process
CreateOrder => [ChargePayment, ReserveStock]
ChargePayment -/> CancelOrder {refund}
ReserveStock -/> CancelOrder {releaseHold}
[ChargePayment, ReserveStock] == ShipGoods
ShipGoods -> End
```





# Glossary

| Term                  | Definition                                              | See Also     |
| --------------------- | ------------------------------------------------------- | ------------ |
| **Node**              | Vertex in the flow graph                                | [[#NODE Types]]    |
| **Edge**              | Connection between nodes                                | [[#Edge Operators]]    |
| **Guard**             | Condition that must be satisfied for an edge to fire    | [[#Edge Operators]]    |
| **Event Edge (`^>`)** | Link triggered on an external or internal event         | [[#Edge Operators]]    |
| **Fork (`=>`)**       | Fan‑out that starts multiple branches                   | [[#Canonical Patterns]] |
| **Merge (`==`)**      | Fan‑in that waits for all branches                      | [[#Canonical Patterns]] |
| **Async Edge (`~>`)** | Non‑blocking call that does not delay the parent flow   | [[#Edge Operators]]    |
| **SubFlow**           | Reference to another flow, similar to a function call   | [[#NODE Types]]    |
| **SideEffect**        | Node whose primary role is external effect (email, log) | [[#NODE Types]]    |
| **Timer**             | Node that waits a specified duration before firing      | [[#NODE Types]]    |
