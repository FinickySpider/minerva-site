---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/edges/"}
---

# Edge Operators

Edges connect nodes and define runtime behaviour.

| Operator | Name                | Typical Use |
|----------|---------------------|-------------|
| `->`     | Sequence            | Standard step |
| `<->`    | Bidirectional       | Hand‑shake protocols |
| `-/>`    | Error               | Exception or rollback |
| `~>`     | Async               | Fire‑and‑forget call |
| `=>`     | Fork (fan‑out)      | Parallel branches |
| `==`     | Merge (fan‑in)      | Wait‑all join |
| `^>`     | Trigger / Event     | Reactive edge |
| `*->`    | Loop / Retry        | Iteration |

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
