# VIBES Axis Interactions

Emergent patterns arise when axes combine. Understanding these interactions helps predict system behavior and guide improvements.

## Critical Combinations

### 🙈 + 🌀 = Maximum Hallucination Risk
**Pattern**: No conceptual model + circular dependencies
**Result**: LLMs generate plausible-sounding nonsense
**Example**: Stringly-typed configuration with circular imports
**Impact**: Each retry compounds confusion

### 🔬 + 🎀 = Ideal Refactoring Target
**Pattern**: Perfect conceptual model + independent components  
**Result**: Safe, predictable transformations
**Example**: Pure functional modules with clear types
**Impact**: LLMs can restructure without breaking invariants

### 👓 + 💠 = Safety-Critical Sweet Spot
**Pattern**: Clear purpose + compile-time safety
**Result**: Good enough for critical systems
**Example**: Strongly-typed medical device firmware
**Impact**: Human-verifiable correctness without excessive abstraction

### 🙈 + 🧶 + 🌊 = The Toxic Trinity
**Pattern**: No model + complex dependencies + cascading errors
**Result**: Debugging requires archaeology and prayer
**Example**: Legacy enterprise systems, Kubernetes YAML
**Impact**: LLMs fail repeatedly, humans suffer constantly

## Stability Patterns

### Stable Under Load
- `<🔍🪢🧊>` - Clear model prevents drift
- `<👓🎀💠>` - Isolation contains changes
- `<🔬🪢💠>` - Types guide evolution

### Fragile Combinations  
- `<🔬🌀💧>` - Beautiful chaos (clever but unmaintainable)
- `<🙈🎀🧊>` - False security (isolated confusion)
- `<👓🧶🌊>` - Brittle complexity (one path through minefield)

## Transformation Dynamics

### Natural Progressions
Some combinations naturally lead to others:

`<🙈🌀🌊>` → `<🙈🧶💧>` → `<👓🪢🧊>` → `<🔍🪢💠>`

Each step enables the next:
1. Contain errors to see the system
2. Untangle to understand flow  
3. Build model once flow is clear
4. Add types once model is stable

### Impossible Jumps
Some transformations require intermediate steps:

Cannot go `<🙈🌀🌊>` → `<🔬🎀💠>` directly
- Must stabilize before abstracting
- Must understand before modeling

### Regression Risks
Watch for combinations that degrade over time:

`<🔍🪢💠>` → `<🔍🧶💧>` (feature creep)
`<🔬🎀🧊>` → `<🔬🧶💧>` (optimization coupling)

## Domain-Specific Patterns

### Web Applications
Common: `<🔍🧶💧>` - Frameworks with magic
Target: `<🔍🪢🧊>` - Explicit pipelines

### Data Pipelines  
Common: `<👓🪢💧>` - Clear flow, runtime errors
Target: `<🔍🪢💠>` - Typed schemas throughout

### Infrastructure
Common: `<🙈🧶🌊>` - String-based chaos
Target: `<👓🎀🧊>` - Isolated, validated components

### ML Systems
Common: `<🔬🌀💧>` - Beautiful math, tangled runtime
Target: `<🔬🪢🧊>` - Clear experiment pipelines

## Interaction Heuristics

### When Expressive Dominates
High expressive (🔍/🔬) can compensate for moderate context issues:
- `<🔬🧶💧>` still workable if model is clear
- `<🔍🧶🧊>` acceptable with good abstractions

### When Context Dominates  
Poor context (🌀/🧶) undermines everything:
- `<🔬🌀💠>` - Types can't save circular logic
- `<🔍🌀🧊>` - Model breaks with cycles

### When Errors Dominate
Bad error surface (🌊/💧) masks other qualities:
- `<🔬🎀🌊>` - Can't see the beauty through explosions
- `<🔍🪢🌊>` - Clear flow hidden by cascading failures

## Measurement Impact

Different combinations affect different metrics:

**Token Efficiency**:
- Best: `<🔬🎀💠>` - One shot success
- Worst: `<🙈🌀🌊>` - Infinite retries

**Debug Time**:
- Best: `<Any/🎀/💠>` - Problems isolated  
- Worst: `<Any/🌀/🌊>` - Everything connects

**Maintenance Cost**:
- Best: `<🔍/🪢/🧊>` - Clear, stable, testable
- Worst: `<🙈/🧶/💧>` - Unclear, coupled, fragile

## The Meta-Pattern

The interaction patterns themselves demonstrate good VIBES:
- **Expressive**: Multiple ways to combine axes meaningfully (🔍)
- **Context**: Each axis assessment independent (🎀)
- **Error**: Invalid combinations obvious at assessment time (🧊)

## Navigation

- [Back to Notation](./README.md)
- [Assessment Protocol](./assessment-protocol.md)
- [View Patterns](../corpus/patterns/)
