---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/examples/"}
---

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
