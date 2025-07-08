# LLM-Optimized Tools

These sketches demonstrate how tools designed specifically for LLM use differ from human-optimized interfaces. Each example shows how different design principles lead to better LLM ergonomics.

## Design Principles for LLM Tools

1. **Semantic Clarity Over Brevity** - LLMs parse meaning, not keystrokes
2. **Stateless Over Stateful** - Each interaction should be self-contained
3. **Explicit Over Implicit** - All context in the command/call
4. **Multiple Aliases** - Many ways to express the same intent
5. **Domain Markers** - Visual/semantic grouping of related operations

## Tool Sketches

### [llm-cli.md](./llm-cli.md) - Command Line for LLMs
Shows how CLI design changes when optimizing for pattern matching vs typing efficiency.

### [semantic-api.md](./semantic-api.md) - REST API with Semantic Aliases
Demonstrates multiple equivalent endpoints that compile to same operation.

### [config-lang.md](./config-lang.md) - Configuration Language
Configuration format that guides toward valid states through structure.

### [error-domains.md](./error-domains.md) - Domain-Based Error Handling
How emoji domains create visual type systems for error handling.

## Why These Designs Work

Traditional tools optimize for:
- Minimum keystrokes (CLI)
- DRY principles (APIs)
- Terse syntax (configs)
- Hierarchical errors (exceptions)

LLM tools optimize for:
- Pattern recognition (semantic clarity)
- Context completeness (stateless design)
- Multiple expressions (aliases and variants)
- Visual categorization (domain markers)

## Navigation

- [Back to Examples](../README.md)
- [VIBES-Lang](../vibes-lang/) - Full language design
- [Core Principles](../../VIBES-RFC-001.md)
