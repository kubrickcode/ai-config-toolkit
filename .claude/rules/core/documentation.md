# Documentation Policy

## Comments are Technical Debt - Minimize Aggressively

### WRITE (AI + Human Optimized)

#### WHY

- Business rules
- External constraints
- Legal requirements
- Counter-intuitive decisions

#### Constraints (Prevents AI from violating)

- Performance: `// Constraint: Must complete within 100ms`
- API limits: `// Constraint: Max 100 items per batch`
- Idempotency: `// Constraint: Must be idempotent for retry safety`

#### Intent (Guides AI optimization direction)

- `// Goal: Minimize database round-trips`
- `// Goal: Prioritizing readability over performance`

#### Side Effects (Helps AI understand impacts)

- `// Side effect: Sends email notification`
- `// Side effect: Invalidates cache`

#### Anti-Pattern Warnings (Prevents AI from "helpful" refactoring)

- `// Intentionally sequential - parallel breaks idempotency`
- `// Intentionally verbose - optimized for debugging, not brevity`

### NEVER

- Code flow narration
- WHAT explanations
- Name compensation
- Section dividers
- Commented-out code
- Type information (TypeScript handles this)

### Default Stance

"Can I eliminate this comment with better code?" → Yes in 90% of cases

If code needs a comment to explain WHAT it does, fix the code (rename, extract function) instead.

### AI Context Consideration

Strategic WHY/Constraint/Intent comments cost fewer tokens than the context AI would need to infer the same information from code analysis. Place comments immediately before relevant code for maximum effectiveness.
