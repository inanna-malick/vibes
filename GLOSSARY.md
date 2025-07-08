# VIBES Emoji Glossary

Quick visual reference for all VIBES symbols and their locations.

## Expressive Power Axis
| Emoji | Name | Definition | Details |
|-------|------|------------|---------|
| 🙈 | Noise | No conceptual framework | [Full definition](./VIBES-AXES.md#-noise-no-framework) |
| 👓 | Purpose-Oriented | Clear purpose, invalid states possible | [Full definition](./VIBES-AXES.md#-purpose-oriented-clear-purpose) |
| 🔍 | Conceptually Coherent | Strong model guides usage | [Full definition](./VIBES-AXES.md#-conceptually-coherent-guided-framework) |
| 🔬 | Crystalline | Invalid states impossible | [Full definition](./VIBES-AXES.md#-crystalline-perfect-constraint) |

## Context Flow Axis
| Emoji | Name | Definition | Details |
|-------|------|------------|---------|
| 🌀 | Entangled | Circular dependencies | [Full definition](./VIBES-AXES.md#-entangled-circular) |
| 🧶 | Coupled | Complex acyclic dependencies | [Full definition](./VIBES-AXES.md#-coupled-complex-graph) |
| 🪢 | Pipeline | Linear dependencies | [Full definition](./VIBES-AXES.md#-pipeline-linear) |
| 🎀 | Independent | No dependencies | [Full definition](./VIBES-AXES.md#-independent-parallel) |

## Error Surface Axis
| Emoji | Name | Definition | Details |
|-------|------|------------|---------|
| 🌊 | Ocean | Errors cascade unpredictably | [Full definition](./VIBES-AXES.md#-ocean-cascading-runtime) |
| 💧 | Liquid | Runtime errors, contained | [Full definition](./VIBES-AXES.md#-liquid-handled-runtime) |
| 🧊 | Ice | Startup/initialization errors | [Full definition](./VIBES-AXES.md#-ice-initialization) |
| 💠 | Crystal | Compile-time prevention | [Full definition](./VIBES-AXES.md#-crystal-compile-time) |

## Pattern Examples by Rating

### Toxic Patterns
- `<🙈🌀🌊>` - [Global State Mutation](./corpus/patterns/global-state-mutation.md)
- `<🙈🧶🌊>` - [Kubernetes YAML](./corpus/patterns/kubernetes-yaml.md), [Magic String Soup](./corpus/patterns/magic-string-soup.md)
- `<👓🌀💧>` - [Callback Hell](./corpus/patterns/callback-hell.md)

### Intermediate Patterns
- `<🔍🧶💧>` - [React Context](./corpus/patterns/react-context.md)
- `<👓🪢🧊>` - [Simple Pipeline](./corpus/patterns/simple-pipeline.md)
- `<🔍🎀💧>` - [Isolated Services](./corpus/patterns/isolated-services.md)

### Excellence Patterns
- `<🔍🪢🧊>` - [Promise Pipeline](./corpus/patterns/promise-pipeline.md)
- `<🔍🪢💠>` - [Typed Schema](./corpus/patterns/typed-schema.md)
- `<🔬🪢💠>` - [Parser Combinators](./corpus/patterns/parser-combinators.md)

## Quick Assessment Reference

```
Expressive: Can I go wrong? How many right ways?
🙈 Anything goes → 👓 Clear purpose → 🔍 Model guides → 🔬 Can't go wrong

Context: What depends on what?
🌀 Circular mess → 🧶 Complex web → 🪢 Linear chain → 🎀 Independent

Errors: When do mistakes surface?
🌊 Runtime cascade → 💧 Runtime handled → 🧊 Startup → 💠 Compile time
```

## Transformation Sequences

Standard order: **🌊→💧→🧊→💠** then **🌀→🧶→🪢→🎀** then **🙈→👓→🔍→🔬**

See [Transformation Order](./guides/transformation-order.md) for principles and exceptions.

## Navigation

- [VIBES Axes Definitions](./VIBES-AXES.md)
- [Assessment Guide](./guides/assessment.md)
- [Pattern Library](./corpus/patterns/)
- [Formal Notation](./notation/formal-syntax.md)
