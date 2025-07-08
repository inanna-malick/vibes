# VIBES Formal Syntax

## Coordinate Notation

VIBES coordinates are expressed as a 3-tuple of emoji states:

```
<Expressive/Context/Error>
```

### Valid States

| Axis | States | Order |
|------|--------|-------|
| Expressive | 🙈 👓 🔍 🔬 | Noise → Purpose-Oriented → Coherent → Crystalline |
| Context | 🌀 🧶 🪢 🎀 | Entangled → Coupled → Pipeline → Independent |
| Error | 🌊 💧 🧊 💠 | Ocean → Liquid → Ice → Crystal |

### Examples
- `<🙈🌀🌊>` - Complete chaos (typical legacy system)
- `<🔍🪢💠>` - Well-structured typed system
- `<🔬🎀💠>` - Ideal functional composition

## Transformation Notation

Transformations show movement along axes:

```
<current> → <target>
```

Individual axis changes:
```
🙈→🔍 (improve expressiveness)
🌀→🪢 (linearize dependencies)  
🌊→💠 (shift errors to compile time)
```

## Annotation Format

For tooling integration:

```
# @vibes: <🔍🪢💠>
```

In code comments:
```javascript
// @vibes: <🔍🪢💠>
function processData(input: ValidatedInput): Result {
  // ...
}
```

In documentation:
```yaml
vibes:
  rating: <🔍🪢💠>
  consensus: 0.92
  models: ["GPT-4", "Claude", "Gemini", "DeepSeek"]
```

## Composition Rules

1. **Order Matters**: Always Expressive/Context/Error
2. **No Skipping**: Can't jump from 🙈 to 🔬 without intermediate states
3. **Independent Axes**: Each axis can be assessed separately
4. **Snapshot Rating**: Represents current state, not potential

## Meta-Notation

For disputed or context-dependent ratings:

```
<🔍|🔬, 🪢, 💠>  # Expressive disputed between 🔍 and 🔬
<🔍🪢💠>*         # Requires context (see notes)
<🔍🪢💠>~         # Temporal degradation likely
```

## Navigation

- [Back to Notation](./README.md)
- [Emoji Semantics](./emoji-semantics.md)
- [Assessment Protocol](./assessment-protocol.md)
