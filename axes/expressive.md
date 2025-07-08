# Expressive Power 🙈→👓→🔍→🔬

Measures how well a system orients users toward valid expressions while making invalid states unrepresentable.

## The Scale

### 🙈 Noise (No Conceptual Framework)
Everything is possible, nothing is guided. No coherent model exists to orient users.

**Examples**:
- Stringly-typed APIs accepting any format
- Configuration through environment variables with no schema
- Magic constants scattered throughout code

**Why it's 🙈**: Users must guess valid inputs through trial and error.

### 👓 Purpose-Oriented (Clear Purpose, Weak Constraints)
Clear purpose but invalid states still expressible. The "happy path" is visible but not enforced.

**Examples**:
- `add_floats(2.0, 2.0)` - correct usage clear, but might accept strings
- RESTful APIs with documented conventions but no enforcement
- Functions with boolean flag parameters

**Why it's 👓**: You can see what to do, but the system won't stop you from doing it wrong.

### 🔍 Conceptually Coherent (Strong Guiding Framework)
Strong conceptual model guides toward valid patterns. The system shapes your thinking.

**Examples**:
- Builder pattern enforcing construction order
- State machines with explicit transitions
- Well-designed DSLs for specific domains

**Why it's 🔍**: Multiple valid approaches exist, all guided by coherent concepts.

### 🔬 Crystalline (The Newspeak Principle Inverted)
General purpose yet invalid states impossible. The rare achievement where flexibility meets safety.

**Examples**:
- Parser combinators producing only valid ASTs
- Algebraic data types with exhaustive pattern matching
- Type-level state machines

**Test**: Multiple expression paths compile to identical behavior/AST.

**Why it's 🔬**: The system is both flexible and precise - Orwell's Newspeak inverted for good.

## Assessment Questions

1. **Can you express what you need at all?** (No → 🙈)
2. **Is there a clear conceptual model?** (No → 🙈, Weak → 👓)
3. **Does the model guide you toward valid expressions?** (Yes → 🔍)
4. **Are invalid expressions impossible to construct?** (Yes → 🔬)

## The Goal

Move from "anything goes" to "the valid path is obvious and invalid paths impossible."

See also: [Error Surface](./error.md) for when errors are caught vs whether they're possible.
