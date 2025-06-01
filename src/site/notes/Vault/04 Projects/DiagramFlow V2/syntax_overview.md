---
{"dg-publish":true,"permalink":"/vault/04-projects/diagram-flow-v2/syntax-overview/"}
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
2. **NODE <Identifier>** declares a node.
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
Save, share, done.
