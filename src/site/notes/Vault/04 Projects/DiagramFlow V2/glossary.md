---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/glossary/"}
---

# Glossary

| Term                  | Definition                                              | See Also     |
| --------------------- | ------------------------------------------------------- | ------------ |
| **Node**              | Vertex in the flow graph                                | [[Vault/04 Projects/DiagramFlow V2/nodes\|nodes]]    |
| **Edge**              | Connection between nodes                                | [[Vault/04 Projects/DiagramFlow V2/edges\|edges]]    |
| **Guard**             | Condition that must be satisfied for an edge to fire    | [[Vault/04 Projects/DiagramFlow V2/edges\|edges]]    |
| **Event Edge (`^>`)** | Link triggered on an external or internal event         | [[Vault/04 Projects/DiagramFlow V2/edges\|edges]]    |
| **Fork (`=>`)**       | Fan‑out that starts multiple branches                   | [[Vault/04 Projects/DiagramFlow V2/patterns\|patterns]] |
| **Merge (`==`)**      | Fan‑in that waits for all branches                      | [[Vault/04 Projects/DiagramFlow V2/patterns\|patterns]] |
| **Async Edge (`~>`)** | Non‑blocking call that does not delay the parent flow   | [[Vault/04 Projects/DiagramFlow V2/edges\|edges]]    |
| **SubFlow**           | Reference to another flow, similar to a function call   | [[Vault/04 Projects/DiagramFlow V2/nodes\|nodes]]    |
| **SideEffect**        | Node whose primary role is external effect (email, log) | [[Vault/04 Projects/DiagramFlow V2/nodes\|nodes]]    |
| **Timer**             | Node that waits a specified duration before firing      | [[Vault/04 Projects/DiagramFlow V2/nodes\|nodes]]    |
