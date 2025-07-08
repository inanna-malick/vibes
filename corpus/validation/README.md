# VIBES Validation Methodology

This directory contains consensus data, validation protocols, and cross-model testing results for VIBES patterns.

## Structure

### Consensus Data
- [consensus-protocol.md](./consensus-protocol.md) - How we achieve cross-model agreement
- [divergence-catalog.md](./divergence-catalog.md) - Documented disagreements and their implications
- [model-comparison.md](./model-comparison.md) - How different models assess patterns

### Test Results
- [pattern-consensus/](./pattern-consensus/) - Individual pattern validation results
- [boundary-tests/](./boundary-tests/) - Edge case assessments
- [temporal-studies/](./temporal-studies/) - How ratings change over time

### Validation Tools
- [assessment-template.md](./assessment-template.md) - Template for new validations
- [consensus-calculator.md](./consensus-calculator.md) - How to calculate agreement
- [quality-criteria.md](./quality-criteria.md) - What makes a good assessment

## Key Findings

### High Consensus Patterns (>90% agreement)
- Toxic patterns (`<🙈🌀🌊>`) - Universal recognition
- Crystalline patterns (`<🔬🪢💠>`) - Clear excellence
- Simple transformations (single axis changes)

### Medium Consensus (70-90% agreement)
- Framework-specific patterns (React, Spring, etc.)
- Boundary cases (🧶 vs 🪢, 💧 vs 🧊)
- Domain-specific interpretations

### Low Consensus (<70% agreement)
- Cutting-edge patterns (new languages/frameworks)
- Cultural differences in assessment
- Patterns requiring deep domain knowledge

## Validation Principles

1. **Minimum 3 Models**: Every pattern requires assessment from at least 3 different LLMs
2. **Document Context**: Note model versions, prompts used, date of assessment
3. **Capture Rationale**: Record not just ratings but reasoning
4. **Track Evolution**: Patterns may change rating as languages/frameworks evolve

## Contributing Validations

To contribute validation data:
1. Use the [assessment-template.md](./assessment-template.md)
2. Test with 3+ different models
3. Document any divergence
4. Submit results in pattern-consensus/

## Navigation

- [Back to Corpus](../README.md)
- [Assessment Protocol](../../notation/assessment-protocol.md)
- [Pattern Library](../patterns/)
