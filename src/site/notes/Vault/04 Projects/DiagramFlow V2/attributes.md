---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/attributes/"}
---

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
