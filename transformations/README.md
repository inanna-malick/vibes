# VIBES Transformations

Practical paths for improving code from any starting point. Each transformation shows concrete steps with rationale.

## Transformation Principles

### 1. Order Matters
Always follow: **Stabilize Errors → Untangle Dependencies → Build Conceptual Model**

Trying to add conceptual elegance to unstable, tangled code is like decorating a house on fire.

### 2. Incremental Progress
Every step should be shippable. Moving from `<🙈🌀🌊>` to `<🔬🎀💠>` happens through many small improvements, not one big rewrite.

### 3. Preserve Behavior
Transformations maintain existing functionality while improving structure. Tests are your safety net.

## Common Transformation Paths

### The Chaos Taming Path
`<🙈🌀🌊>` → `<🙈🌀💧>` → `<🙈🪢💧>` → `<🔍🪢💧>` → `<🔍🪢💠>`

Most legacy code follows this path:
1. Add error handling (stop the bleeding)
2. Break circular dependencies
3. Introduce conceptual boundaries
4. Harden with types

### The Quick Win Path
For when you need immediate improvement:
- `<*️⃣*️⃣🌊>` → `<*️⃣*️⃣💧>` - Just add error boundaries
- `<*️⃣🌀*️⃣>` → `<*️⃣🧶*️⃣>` - Break one circular dependency
- `<🙈*️⃣*️⃣>` → `<👓*️⃣*️⃣>` - Add one clear abstraction

### The Greenfield Path
When building new:
1. Start with `<🔍🎀💠>` as target
2. Accept `<🔍🪢💧>` for MVP
3. Iterate toward crystalline

## Transformation Catalog

### By Starting Point

**From Noise** `<🙈*️⃣*️⃣>`
- [String Typing to Domain Types](./from-noise/strings-to-types.md)
- [God Object to Modules](./from-noise/god-object-breakup.md)
- [Magic Numbers to Named Constants](./from-noise/magic-to-meaning.md)

**From Entanglement** `<*️⃣🌀*️⃣>`
- [Circular Deps to Dependency Injection](./from-entanglement/circular-to-di.md)
- [Callback Hell to Promises](./from-entanglement/callbacks-to-promises.md)
- [Event Soup to Event Sourcing](./from-entanglement/events-to-sourcing.md)

**From Cascading Errors** `<*️⃣*️⃣🌊>`
- [Try-Catch All to Result Types](./from-ocean/catch-to-result.md)
- [Null Checks to Option Types](./from-ocean/null-to-option.md)
- [Runtime Config to Parse-Time](./from-ocean/runtime-to-parse.md)

### By Goal

**Achieving Conceptual Clarity** `→🔍/🔬`
- [Building Domain Models](./to-clarity/domain-models.md)
- [Creating DSLs](./to-clarity/dsl-design.md)
- [Type-Driven Design](./to-clarity/type-driven.md)

**Achieving Independence** `→🎀`
- [Extracting Pure Functions](./to-independence/pure-functions.md)
- [Removing Hidden Dependencies](./to-independence/explicit-deps.md)
- [Modularization Strategies](./to-independence/modularization.md)

**Achieving Safety** `→💠`
- [Parse Don't Validate](./to-crystal/parse-dont-validate.md)
- [Making Invalid States Unrepresentable](./to-crystal/invalid-states.md)
- [Phantom Types for Safety](./to-crystal/phantom-types.md)

## Step-by-Step Examples

### Example: Config System Evolution

Starting point: Environment variables everywhere
```bash
DATABASE_URL=${DATABASE_URL:-localhost}
TIMEOUT=${TIMEOUT:-30}
DEBUG=${DEBUG:-false}
```
Rating: `<🙈🌀🌊>`

[See full transformation →](./examples/config-evolution.md)

Final state: Type-safe configuration
```typescript
const Config = z.object({
  database: DatabaseConfig,
  timeout: z.number().min(1).max(300),
  debug: z.boolean()
}).parse(loadConfig());
```
Rating: `<🔍🪢💠>`

## Transformation Patterns

### The Stabilization Pattern
When facing `<*️⃣*️⃣🌊>`:
1. Identify error propagation paths
2. Add boundaries at module edges
3. Convert exceptions to explicit errors
4. Ensure errors can't cascade

### The Untangling Pattern  
When facing `<*️⃣🌀*️⃣>`:
1. Map all dependencies
2. Identify cycles
3. Extract interfaces
4. Inject dependencies

### The Conceptual Modeling Pattern
When facing `<🙈*️⃣*️⃣>`:
1. Identify core domain concepts
2. Create types for each concept
3. Make operations explicit
4. Build around invariants

## Tools for Transformation

- **Refactoring**: Small, safe changes
- **Strangler Fig**: Gradual replacement
- **Branch by Abstraction**: Parallel development
- **Feature Flags**: Risk mitigation

## Success Metrics

A transformation succeeds when:
- Tests still pass
- Code is easier to understand
- New features are easier to add
- LLMs can work with it effectively
- Team velocity increases

## Anti-Patterns in Transformation

- **Big Bang Rewrite**: Usually fails
- **Perfectionism**: Perfect is enemy of better
- **Skipping Steps**: Order matters
- **No Tests**: Flying blind

## Getting Started

1. Rate your current code using [VIBES notation](../notation/)
2. Identify biggest pain axis
3. Find relevant transformation
4. Follow step-by-step guide
5. Measure improvement

Remember: Every journey from chaos to clarity happens one step at a time.
