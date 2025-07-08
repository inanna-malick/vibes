# VIBES Axes

The three axes measure different aspects of LLM ergonomics. Each axis uses emoji progression to show increasing quality.

## The Three Axes

### [Expressive Power](./expressive.md) 🙈→👓→🔍→🔬
How well does the system guide users toward valid expressions while making invalid states unrepresentable?

### [Context Flow](./context.md) 🌀→🧶→🪢→🎀
Does traversal order change meaning? How tangled are the dependencies?

### [Error Surface](./error.md) 🌊→💧→🧊→💠
When in the lifecycle are invalid states detected?

## Axis Interactions

Certain combinations create emergent patterns:

| Pattern | Impact |
|---------|--------|
| 🙈 + 🌀 | Maximum hallucination risk |
| 🔬 + 🎀 | Ideal for refactoring agents |
| 👓 + 💠 | Safety-critical appropriate |
| 🙈 + 🧶 + 🌊 | Toxic pattern (see [Kubernetes YAML](../patterns/anti-patterns/k8s-yaml.md)) |

## Quick Assessment

For each axis, ask:

**Expressive**: "How well does this guide me toward valid code?"
**Context**: "Can I understand this piece without seeing the whole?"  
**Error**: "When will mistakes be caught?"

See [notation](../notation/) for formal syntax and [validation](../validation/) for testing methodology.
