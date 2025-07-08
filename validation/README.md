# VIBES Validation

How we test and validate VIBES ratings across different language models.

## Methodology

### Cross-Model Consensus Testing

We validate VIBES ratings through consensus across multiple models:

- **Test Set**: 28 canonical patterns + 9 transformations
- **Models**: Latest from Anthropic, OpenAI, Google, and others
- **Threshold**: 3/4 agreement = stable rating
- **Documentation**: All disagreements documented as boundary cases

### Rating Stability Categories

| Category | Agreement | Interpretation |
|----------|-----------|----------------|
| **Stable** | ≥75% | High confidence rating |
| **Disputed** | 50-74% | Context-dependent rating |
| **Model-Dependent** | <50% | Requires careful consideration |

## Test Protocol

### 1. Pattern Presentation

Each pattern is presented without rating to multiple models:

```markdown
## Pattern: [Name]
```[language]
[code sample]
```

Task: Rate this pattern using VIBES notation <Expressive/Context/Error>
Provide rationale for each axis rating.
```

### 2. Consensus Building

When models disagree:
1. Identify source of disagreement (terminology, training data, paradigm)
2. Refine pattern description or axis definition
3. Re-test with clarifications
4. Document persistent disagreements

### 3. Validation Criteria

Ratings are considered valid when:
- Rationales align with axis definitions
- Similar patterns receive similar ratings
- Transformations show expected progressions
- Edge cases are explicitly acknowledged

## Key Findings

### High Agreement Patterns

Anti-patterns consistently rated `<🙈🌀🌊>`:
- Global mutable state
- Stringly-typed everything
- Circular dependencies

Well-designed patterns consistently rated `<🔍🪢💠>` or better:
- Type-safe builders
- Pure functional pipelines
- Parse-don't-validate

### Disagreement Sources

1. **Framework-Specific Knowledge**: React hooks vs Redux patterns
2. **Paradigm Bias**: OOP vs FP perspectives
3. **Temporal Knowledge**: Newer patterns not in all training data
4. **Cultural Context**: Different programming traditions

## Validation Data

- [Consensus Results](./consensus/) - Raw agreement data
- [Boundary Cases](./boundary-cases.md) - Patterns with split ratings
- [Model Comparisons](./model-comparisons.md) - How different models rate

## Contributing Validation

To add validation data:

1. Test pattern with 4+ models
2. Document all ratings and rationales
3. Calculate consensus percentage
4. Submit PR with results

See [CONTRIBUTING.md](../CONTRIBUTING.md) for details.

## Evolution

VIBES ratings may evolve as:
- New programming paradigms emerge
- Model training data updates
- Community understanding deepens

We version our validation data to track these changes over time.
