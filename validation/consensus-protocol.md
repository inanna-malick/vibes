# VIBES Validation Protocol

## Consensus Mechanics

The VIBES framework uses multi-model consensus to validate pattern assessments. This protocol ensures ratings reflect genuine ergonomic properties rather than model-specific biases.

## Core Principles

1. **Minimum 3 models** must assess each pattern
2. **Convergence threshold**: 3/4 models must agree on each axis
3. **Divergence documentation**: When models disagree, document why
4. **Context sensitivity**: Same code can rate differently as library vs application

## Assessment Procedure

### Phase 1: Independent Assessment
Each model receives:
```
Pattern: [code sample]
Context: [library|application|framework|script]
Task: Rate using <Expressive/Context/Error> notation
Required: Explain each rating with specific evidence
```

### Phase 2: Divergence Analysis
When ratings differ by >1 step on any axis:
1. Identify source of divergence (training data, terminology, perspective)
2. Clarify ambiguous aspects
3. Re-assess with clarifications
4. Document persistent divergences

### Phase 3: Consensus Recording
```yaml
pattern: validation-chain
consensus: <🔍🪢💧>
confidence: high (4/4 agreement)
divergences: 
  - none
context_notes:
  - "As library: could be 🔬 with type parameters"
  - "As script: might be 👓 due to fixed pipeline"
```

## Divergence Patterns

Common sources of assessment divergence:

**Training Data Bias**
- Framework-specific patterns (React, Django, Rails)
- Language idioms varying by ecosystem
- Historical context (pre/post certain language features)

**Terminology Ambiguity**
- "Global state" (module-level vs truly global)
- "Type safety" (static vs runtime validation)
- "Dependencies" (import vs data flow)

**Perspective Differences**
- User vs implementer view
- Library vs application context
- Beginner vs expert expectations

## Calibration Patterns

Use these for initial model calibration:

**Canonical Toxic**: `<🙈🌀🌊>`
```javascript
eval(localStorage.getItem('code'))
```

**Canonical Good**: `<🔍🪢🧊>`
```typescript
const pipeline = pipe(
  validate,
  transform,
  persist
);
```

**Canonical Crystalline**: `<🔬🎀💠>`
```haskell
newtype Email = Email Text
mkEmail :: Text -> Maybe Email
```

## Documentation Requirements

Each validated pattern must include:
- Consensus rating with confidence level
- Individual model assessments (anonymized)
- Divergence explanations if any
- Context variations noted
- Transformation potential identified

## Community Validation

Patterns achieve "community validated" status when:
1. Multi-model consensus achieved (this protocol)
2. Human expert review confirms assessment
3. Real-world usage validates ergonomics
4. Transformation paths proven in practice

This creates a living validation system that improves over time through collective intelligence.
