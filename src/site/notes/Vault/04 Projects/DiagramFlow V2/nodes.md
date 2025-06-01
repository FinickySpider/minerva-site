---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/nodes/"}
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
Refer to **glossary.md** for fast lookup plus examples for each minor type.
