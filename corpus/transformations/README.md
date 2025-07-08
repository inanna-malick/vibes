# VIBES Transformations

This directory contains step-by-step transformations showing how to improve code from toxic patterns to crystalline excellence.

## Structure

Each transformation file shows:
- Starting state with VIBES rating
- Each intermediate step with rationale
- Final state with improved rating
- Alternative paths when applicable
- Common pitfalls to avoid

## Transformation Catalog

### Complete Journeys
- [global-to-modular.md](./global-to-modular.md) - `<🙈🌀🌊>` → `<🔍🎀🧊>` - Global state to modular design
- [callback-to-async.md](./callback-to-async.md) - `<👓🌀💧>` → `<🔍🪢💠>` - Callback hell to typed async
- [stringly-to-strongly.md](./stringly-to-strongly.md) - `<🙈🧶🌊>` → `<🔬🪢💠>` - String typing to strong types

### Axis-Specific Improvements
- [stabilize-errors.md](./stabilize-errors.md) - Focus on 🌊→💧→🧊→💠 progression
- [untangle-dependencies.md](./untangle-dependencies.md) - Focus on 🌀→🧶→🪢→🎀 progression
- [build-conceptual-model.md](./build-conceptual-model.md) - Focus on 🙈→👓→🔍→🔬 progression

### Quick Wins
- [add-error-boundaries.md](./add-error-boundaries.md) - 🌊→💧 in one step
- [extract-pure-functions.md](./extract-pure-functions.md) - 🧶→🪢 refactoring
- [introduce-types.md](./introduce-types.md) - 👓→🔍 with TypeScript/Python

## Transformation Principles

### The Standard Order
1. **Stabilize Errors First** (🌊→💧→🧊→💠)
2. **Untangle Dependencies** (🌀→🧶→🪢→🎀)
3. **Build Conceptual Model** (🙈→👓→🔍→🔬)

### When to Deviate
See [transformation order exceptions](../guides/transformation-order.md#exceptions) for valid alternative sequences.

### Key Insight
"Minimize intermediate states worse than starting point" - Sometimes the standard order creates temporarily more dangerous code.

## Using These Transformations

### As Learning Tools
Study the rationale at each step to understand why changes improve VIBES ratings.

### As Templates
Adapt the patterns to your specific codebase and constraints.

### As Checklists
Use intermediate steps as milestones in larger refactoring projects.

## Navigation

- [Back to Corpus](../README.md)
- [View Patterns](../patterns/)
- [Transformation Order Guide](../../guides/transformation-order.md)
