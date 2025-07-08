# VIBES Corpus

The VIBES Corpus contains validated patterns, transformations, and consensus data from cross-model testing.

## Structure

### [patterns/](./patterns/)
Individual code patterns demonstrating specific VIBES coordinates. Each pattern includes:
- VIBES rating with rationale
- Code examples
- Common occurrences
- Impact on LLM processing
- Consensus validation data

### [transformations/](./transformations/)
Before/after examples showing how to improve VIBES ratings:
- Step-by-step progression
- Intermediate states
- Rationale for each change
- Alternative paths

### [validation/](./validation/)
Cross-model consensus data:
- Test results from multiple LLMs
- Divergence analysis
- Context-dependent variations
- Temporal degradation studies

## How Patterns Are Selected

Patterns in this corpus must:
1. Represent real-world code structures
2. Have clear VIBES ratings on all three axes
3. Be validated by 3+ different models
4. Demonstrate teaching value

## Pattern Categories

### Toxic Patterns (Anti-patterns)
Combinations that create maximum friction:
- `<🙈🌀🌊>` - Complete chaos
- `<🙈🧶🌊>` - Hidden complexity
- `<👓🌀💧>` - Circular confusion

### Intermediate Patterns
Common patterns with room for improvement:
- `<🔍🧶💧>` - Good model, complex flow
- `<👓🪢🧊>` - Clear flow, weak model
- `<🔍🎀💧>` - Isolated but runtime-heavy

### Excellence Patterns
Patterns approaching ideal ergonomics:
- `<🔍🪢💠>` - Well-structured systems
- `<🔬🪢💠>` - Functional pipelines
- `<🔬🎀💠>` - Perfect composition

## Contributing Patterns

To add a pattern:
1. Use [patterns/PATTERN_TEMPLATE.md](./patterns/PATTERN_TEMPLATE.md)
2. Include working code example
3. Test with 3+ models
4. Document consensus percentage
5. Link related patterns

## Using This Corpus

### For Learning
Start with toxic patterns to understand what to avoid, then study transformations to see improvement paths.

### For Assessment
Use patterns as calibration examples when rating new code.

### For Teaching
Reference patterns when explaining VIBES concepts to teams.

## Navigation

- [View Patterns](./patterns/)
- [View Transformations](./transformations/)
- [View Validation Data](./validation/)
- [Back to Root](../README.md)
