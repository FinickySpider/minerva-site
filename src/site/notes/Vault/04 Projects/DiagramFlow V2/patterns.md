---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/patterns/"}
---

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
Open [[Vault/04 Projects/DiagramFlow V2/examples\|examples]] to see this pattern fully expanded.

## Saga / Compensation
```txt
NODE BookTrip      TYPE: Process
NODE CancelTrip    TYPE: Process
BookTrip -/> CancelTrip {compensate}
```

*See also*: Async Messaging, Timeout Escalation, Circuit Breaker in this same file.
