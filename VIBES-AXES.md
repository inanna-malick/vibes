# VIBES Axis Definitions

## The Three Axes of LLM Affordances

VIBES measures what actions tools afford to LLMs across three fundamental dimensions that capture how LLMs experience and process code patterns.

## 1. Expressive Power: 🙈→👓→🔍→🔬

**Operational Definition**: How well a system affords valid expressions while preventing invalid states.

### 🙈 Noise (No Framework)
- **Definition**: No conceptual framework exists. Pattern affords any input equally.
- **Characteristics**: 
  - Any input might be valid
  - No patterns guide usage
  - Trial and error required
- **Example**: Stringly-typed APIs that accept any JSON

### 👓 Purpose-Oriented (Clear Purpose)
- **Definition**: System affords clear purpose but still permits invalid states.
- **Characteristics**:
  - Intended usage is obvious
  - Wrong usage still compiles
  - Documentation required
- **Example**: `add_floats(2.0, 2.0)` - purpose clear, but could pass strings

### 🔍 Conceptually Coherent (Guided Framework)
- **Definition**: Strong conceptual model affords valid patterns naturally.
- **Characteristics**:
  - Multiple valid approaches exist
  - All lead to correct usage
  - Mistakes become obvious
- **Example**: Builder pattern with required fields

### 🔬 Crystalline (Perfect Constraint)
- **Definition**: Pattern affords general purpose use while making invalid states impossible.
- **Characteristics**:
  - Types enforce correctness
  - Multiple paths, same result
  - Errors impossible by design
- **Example**: Parser combinators producing only valid ASTs

## 2. Context Flow: 🌀→🧶→🪢→🎀

**Operational Definition**: Does the pattern afford order-independent traversal?

### 🌀 Entangled (Circular)
- **Definition**: Pattern affords only circular dependencies with feedback loops.
- **Characteristics**:
  - A affects B affects A
  - Order changes everything
  - State unpredictable
- **Example**: Spreadsheet with circular references

### 🧶 Coupled (Complex Graph)
- **Definition**: Pattern affords complex acyclic dependencies, hidden mutations.
- **Characteristics**:
  - Multiple dependency paths
  - Side effects propagate
  - No cycles but complex
- **Test**: Can state changes affect others without explicit flow? → 🧶
- **Example**: React components with shared context

### 🪢 Pipeline (Linear)
- **Definition**: Pattern affords linear dependencies, clear flow.
- **Characteristics**:
  - Single traversal path
  - Immutable during flow
  - Clear data direction
- **Example**: `data |> validate |> transform |> save`

### 🎀 Independent (Parallel)
- **Definition**: Pattern affords complete independence between components.
- **Characteristics**:
  - Any access order works
  - Components isolated
  - Parallel processing safe
- **Example**: `(name, age, email)` tuple fields

## 3. Error Surface: 🌊→💧→🧊→💠

**Operational Definition**: When does the pattern afford error detection?

### 🌊 Ocean (Cascading Runtime)
- **Definition**: Pattern affords errors that cascade unpredictably at runtime.
- **Characteristics**:
  - One failure causes many
  - Debug paths unclear
  - System-wide effects
- **Example**: Global state mutation causing distant failures

### 💧 Liquid (Handled Runtime)
- **Definition**: Pattern affords runtime errors but contains them.
- **Characteristics**:
  - Try-catch boundaries
  - Explicit error values
  - Local failure impact
- **Example**: `Result<User, Error>` in Rust

### 🧊 Ice (Initialization)
- **Definition**: Pattern affords error detection at startup/initialization.
- **Characteristics**:
  - Fail fast at boot
  - Config validation
  - Dependency checks
- **Example**: Spring dependency injection validation

### 💠 Crystal (Compile Time)
- **Definition**: Pattern affords making invalid states unrepresentable.
- **Characteristics**:
  - Type system prevents errors
  - Compiler enforces rules
  - Runtime errors impossible
- **Rule**: Invalid states cannot be expressed in the language itself
- **Example**: `divide :: Int -> NonZeroInt -> Int`

## Navigation

- [Back to RFC Root](../README.md)
- [Formal Syntax](../notation/formal-syntax.md)
- [Assessment Guide](../guides/assessment.md)
- [Pattern Examples](../corpus/patterns/)
