# VIBES-RFC-001: LLM Affordances Framework

**Status**: Draft  
**Version**: 1.0  
**Date**: July 2025  
**Validation**: Cross-model consensus with latest models

## Abstract

VIBES (Visual Intuitive Benchmarks for Expression Systems) provides a framework for evaluating and improving tool affordances specifically for LLM use. Using a 3-axis qualitative system with emoji-based notation, VIBES measures what actions patterns afford rather than computational metrics.

## Motivation

As LLM-driven development becomes mainstream, the economic impact of poor tool affordances compounds exponentially. Each failed attempt multiplies token costs and development time. Tools optimized for human use often fail to afford appropriate actions for LLMs.

## Core Principle

**LLMs and humans need fundamentally different tools.** Just as we don't expect humans to write assembly code or CPUs to parse English, we shouldn't force LLMs to use human-optimized interfaces.

## The Three Axes

VIBES measures affordances across three dimensions:

1. **Expressive Power** (🙈→👓→🔍→🔬): How well systems afford valid expressions
2. **Context Flow** (🌀→🧶→🪢→🎀): Whether patterns afford order-independent traversal
3. **Error Surface** (🌊→💧→🧊→💠): When patterns afford error detection

See [VIBES-AXES.md](./VIBES-AXES.md) for detailed definitions.

## Notation

VIBES coordinates use the format: `<Expressive/Context/Error>`

Examples:
- `<🙈🌀🌊>` - Affords only chaos
- `<🔍🪢💠>` - Affords structured reasoning
- `<🔬🎀💠>` - Affords ideal composition

See [notation/formal-syntax.md](./notation/formal-syntax.md) for complete specification.

## Assessment Protocol

1. Test with multiple LLMs (minimum 3)
2. Document consensus percentage
3. Note divergent assessments
4. Update patterns based on feedback

See [notation/assessment-protocol.md](./notation/assessment-protocol.md) for methodology.

## Transformation Principle

Always improve in order:
1. Stabilize Errors (🌊→💧→🧊→💠)
2. Untangle Dependencies (🌀→🧶→🪢→🎀)
3. Build Conceptual Model (🙈→👓→🔍→🔬)

See [guides/transformation-order.md](./guides/transformation-order.md) for rationale.

## Repository Structure

This RFC is structured as a navigable repository:
- [VIBES-AXES.md](./VIBES-AXES.md) - Detailed axis definitions
- [corpus/](./corpus/) - Pattern library with examples
- [notation/](./notation/) - Formal syntax and semantics
- [guides/](./guides/) - Improvement patterns
- [examples/](./examples/) - LLM-optimized languages

## Known Limitations

- Discrete levels hide continuous spectrums
- Awaits quantitative validation
- Missing dimensions: performance, discoverability
- Cultural bias toward Western programming paradigms

## Future Work

- Tooling for automated assessment
- Quantitative correlation studies
- Additional axes exploration
- Cross-cultural validation

## Contributing

See individual directory READMEs for contribution guidelines. All patterns require:
- Multi-model validation
- Consensus documentation
- Related pattern links

## References

- Pattern examples: [corpus/patterns/](./corpus/patterns/)
- Transformation guides: [guides/](./guides/)
- Example implementations: [examples/](./examples/)

## Conclusion

VIBES provides vocabulary for discussing what LLM tools afford, not comprehensive evaluation. The core insight: patterns should afford valid expressions while preventing invalid ones, designed specifically for pattern-matching rather than logic engines.

For detailed specifications, navigate the repository structure starting from [README.md](./README.md).
