# VIBES Assessment Guide

How to consistently assess VIBES ratings for any code pattern or system.

## Quick Assessment

For each axis, ask the core question:

### Expressive Power
**Question**: How well does the system guide toward valid expressions?
- No guidance, anything goes → 🙈
- Clear purpose but wrong usage possible → 👓  
- Multiple good paths, all lead to success → 🔍
- Invalid states literally impossible → 🔬

### Context Flow  
**Question**: Does traversal order change meaning?
- Yes, with circular dependencies → 🌀
- Yes, complex interdependencies → 🧶
- No, single linear path → 🪢
- No dependencies at all → 🎀

### Error Surface
**Question**: When are invalid states first detected?
- Runtime, cascading failures → 🌊
- Runtime, but contained → 💧
- Startup/initialization → 🧊
- Cannot be constructed → 💠

## Detailed Assessment Process

### Step 1: Gather Context
Before rating, understand:
- What is this code trying to accomplish?
- Who uses it (humans, LLMs, both)?
- What constraints exist (performance, safety, legacy)?
- What's the maintenance context?

### Step 2: Assess Each Axis

#### Expressive Power Assessment
1. Try to write something incorrectly
2. Does the system guide you to correctness?
3. Count the conceptually different valid approaches
4. Check if invalid expressions are possible

**Key Test**: Can you explain the system's conceptual model in one sentence?
- No coherent model → 🙈
- "It's for X" → 👓
- "It works by Y" → 🔍  
- "It ensures Z through Types" → 🔬

#### Context Flow Assessment
1. Trace data flow through the system
2. Identify all dependencies
3. Check for circular references
4. Test if component order matters

**Key Test**: Draw the dependency graph
- Cycles exist → 🌀
- Multiple paths between nodes → 🧶
- Single directed path → 🪢
- Disconnected nodes → 🎀

#### Error Surface Assessment  
1. Identify all possible failure modes
2. Determine when each is detected
3. Check error propagation paths
4. Test invalid input handling

**Key Test**: When does invalid input fail?
- Sometime during execution → 🌊/💧
- At startup/configuration → 🧊
- Won't compile/parse → 💠

### Step 3: Handle Edge Cases

#### Between 🧶 and 🪢
Ask: "Can changing component A affect component B without explicit data flow?"
- Yes (through shared state, events, etc.) → 🧶
- No (only through passed data) → 🪢

#### Between 💧 and 🧊
Ask: "When is the FIRST moment this error is detected?"
- During normal operation → 💧
- At system startup → 🧊

#### Between 🔍 and 🔬  
Ask: "Do different valid expressions have identical behavior?"
- No, subtle differences → 🔍
- Yes, provably the same → 🔬

### Step 4: Document Rating

Include:
- Overall rating: `<🔍🪢💠>`
- Rationale for each axis
- Edge cases considered
- Context dependencies
- Confidence level

## Common Patterns by Domain

### Web Frameworks
Often `<🔍🧶💧>`:
- Clear models (MVC, components)
- Hidden magic coupling
- Runtime type errors

### Data Science  
Often `<👓🪢💧>`:
- Purpose clear (analysis)
- Pipeline processing
- Runtime data errors

### Systems Programming
Target `<👓🎀💠>`:
- Explicit over magic
- Isolated components
- Compile-time safety

### Functional Programming
Target `<🔬🪢💠>`:
- Mathematical models
- Composition pipelines  
- Type-level guarantees

## Assessment Mistakes to Avoid

### Optimism Bias
Don't rate based on intended use, rate actual constraints:
- "It's meant to be used correctly" → Still 🙈 if anything goes
- "Developers should know better" → Still 🌊 if errors cascade

### Pessimism Bias  
Don't penalize unfamiliar patterns:
- Complex ≠ Bad context flow
- Many features ≠ Low expressiveness
- Dynamic ≠ Poor error surface

### Context Blindness
Remember ratings can be context-dependent:
- Library API vs application code
- Generated vs hand-written
- Team expertise levels

## Quick Reference Card

```
Expressive: Can I go wrong? How many right ways?
🙈 Anything goes
👓 Clear purpose  
🔍 Guided by model
🔬 Constrained to correctness

Context: What depends on what?
🌀 Circular mess
🧶 Complex web
🪢 Linear chain
🎀 Independent

Errors: When do mistakes surface?
🌊 Runtime cascade  
💧 Runtime contained
🧊 Startup time
💠 Compile time
```

## Navigation

- [Back to Guides](./README.md)
- [Assessment Protocol](../notation/assessment-protocol.md)
- [Pattern Examples](../corpus/patterns/)
