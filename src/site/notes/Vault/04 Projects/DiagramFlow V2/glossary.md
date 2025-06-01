---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/glossary/"}
---

# Glossary

| Term                  | Definition                                              | See Also      |
| --------------------- | ------------------------------------------------------- | ------------- |
| **Node**              | Vertex in the flow graph                                | `nodes.md`    |
| **Edge**              | Connection between nodes                                | `edges.md`    |
| **Guard**             | Condition that must be satisfied for an edge to fire    | `edges.md`    |
| **Event Edge (`^>`)** | Link triggered on an external or internal event         | `edges.md`    |
| **Fork (`=>`)**       | Fan‑out that starts multiple branches                   | `patterns.md` |
| **Merge (`==`)**      | Fan‑in that waits for all branches                      | `patterns.md` |
| **Async Edge (`~>`)** | Non‑blocking call that does not delay the parent flow   | `edges.md`    |
| **SubFlow**           | Reference to another flow, similar to a function call   | `nodes.md`    |
| **SideEffect**        | Node whose primary role is external effect (email, log) | `nodes.md`    |
| **Timer**             | Node that waits a specified duration before firing      | `nodes.md`    |
