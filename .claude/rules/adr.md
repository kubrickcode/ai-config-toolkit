# Architecture Decision Records (ADR)

## Check Existing ADRs

Before making architectural or design decisions, scan `docs/decisions/` for existing ADRs. Read only **file names and frontmatter** (status, title) to find relevant ones — do NOT read full ADR contents unless a match is found. If a relevant ADR exists:

- Follow it unless there's a strong reason to change
- If changing: create a new ADR that explicitly supersedes the old one
- Never silently contradict a past ADR

## When to Suggest Writing an ADR

Evaluate using the **PRICE** criteria. If **any** apply, suggest ADR creation:

| Criterion         | Signal                                                        | Example                                       |
| ----------------- | ------------------------------------------------------------- | --------------------------------------------- |
| **P**olicy        | Team convention, process decision, coding standard            | "Use cursor-based pagination"                 |
| **R**eversibility | Hard or costly to undo                                        | DB schema, language choice, auth strategy     |
| **I**mpact        | Affects multiple modules, teams, or many files                | API design pattern, state management approach |
| **C**onstraint    | External business rule, regulation, SLA, 3rd party limitation | "Payments must complete within 100ms"         |
| **E**xception     | Goes against convention, counterintuitive choice              | "Intentionally not using ORM here"            |

When detected: briefly state which PRICE criteria apply, then suggest using the `/adr` skill.

## Do NOT Suggest ADR For

- Routine implementation choices (which variable name, loop vs map)
- Decisions already captured in an existing ADR
- Trivial, easily reversible changes
- Standard patterns that match existing project conventions
