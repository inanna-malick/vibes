# Context Flow 🌀→🧶→🪢→🎀

Measures dependency structure and whether traversal order changes meaning.

## The Scale

### 🌀 Entangled (Cyclic Dependencies)
Circular dependencies with feedback loops. Order of access changes results.

**Examples**:
- Spreadsheet with circular references
- Mutually recursive modules without clear interfaces
- Event systems where handlers trigger their own events

**Test**: Can component A's behavior change based on component B, which depends on A?

**Why it's 🌀**: You're caught in loops with no clear escape.

### 🧶 Coupled (Complex Acyclic Dependencies)
Multiple dependency paths without cycles. Hidden state mutations possible.

**Examples**:
- React components with shared context and effects
- Global configuration affecting multiple modules
- Deeply nested object updates with side effects

**Test**: Can state changes affect other components without explicit data flow?

**Why it's 🧶**: Dependencies exist but at least they don't loop back.

### 🪢 Pipeline (Linear Dependencies)
Sequential processing with clear data flow. Each step depends on previous but doesn't modify it.

**Examples**:
- Unix pipes: `cat file | grep pattern | sort | uniq`
- Promise chains: `fetch().then(parse).then(validate)`
- Functional pipelines: `data |> validate |> transform |> save`

**Test**: Can you trace a single path through the system?

**Why it's 🪢**: One thread to follow, like a linked list.

### 🎀 Independent (No Dependencies)
Components can be accessed in any order without side effects.

**Examples**:
- Tuple of values: `(name, age, email)`
- Pure functions with no shared state
- Immutable data structures

**Test**: Can you access/modify any part without affecting others?

**Why it's 🎀**: Each piece stands alone, tied up neatly.

## Assessment Questions

1. **Are there circular dependencies?** (Yes → 🌀)
2. **Can components affect each other indirectly?** (Yes → 🧶)
3. **Is there a single flow path?** (Yes → 🪢)
4. **Are all components independent?** (Yes → 🎀)

## Context Window Implications

What appears `🎀` in one window may be `🧶` across multiple:

```python
# Window 1: Looks independent
def calculate_price(items):
    return sum(item.price for item in items)

# Window 2: Hidden coupling revealed
GLOBAL_DISCOUNT = 0.1

def calculate_price(items):
    total = sum(item.price for item in items)
    return total * (1 - GLOBAL_DISCOUNT)  # Coupling!
```

Design with chunking in mind - make dependencies explicit.

See also: [Examples of hidden coupling](../patterns/context-flow/)
