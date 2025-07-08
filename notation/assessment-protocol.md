# VIBES Assessment Protocol

## Overview

This protocol ensures consistent VIBES ratings across different assessors and models.

## Assessment Process

### 1. Initial Assessment

For each axis, ask the operational question:

**Expressive Power**: How well does the system guide toward valid expressions?
- Can invalid states be constructed? → Not 💠
- Is there a guiding conceptual model? → At least 👓
- Multiple valid approaches that all work? → At least 🔍
- General yet constrains perfectly? → 🔬

**Context Flow**: Does traversal order change meaning?
- Circular dependencies exist? → 🌀
- Complex paths affecting each other? → 🧶
- Single linear path only? → 🪢
- Components fully independent? → 🎀

**Error Surface**: When are invalid states detected?
- Runtime with cascading effects? → 🌊
- Runtime but contained? → 💧
- Startup/initialization? → 🧊
- Cannot be constructed? → 💠

### 2. Boundary Tests

For ambiguous cases, apply these tests:

**🧶 vs 🪢 Test**: 
Can state changes in one component affect others without explicit data flow?
- Yes → 🧶 (Coupled)
- No → 🪢 (Pipeline)

**💧 vs 🧊 Test**:
When does the system first reject invalid input?
- During operation → 💧
- At startup/config → 🧊

**🔍 vs 🔬 Test**:
Do multiple valid expressions compile to identical behavior?
- No, subtle differences → 🔍
- Yes, provably identical → 🔬

### 3. Multi-Model Validation

1. Test pattern with minimum 3 different LLMs
2. Document each model's rating
3. Calculate consensus percentage
4. Note specific divergences

Example validation record:
```yaml
pattern: kubernetes-yaml
models_tested: 4
consensus: 
  expressive: 100% (🙈)
  context: 75% (🧶 vs 🌀 split)
  error: 100% (🌊)
divergence_notes: "Some models see service mesh as circular (🌀)"
test_date: 2025-07-15
```

### 4. Context Considerations

Ratings may vary based on:

**Usage Context**:
- Library API vs Application code
- Framework vs User code
- Generated vs Hand-written

**Temporal Context**:
- Initial implementation
- After refactoring
- Under maintenance pressure

**Team Context**:
- Greenfield project
- Legacy system
- Mixed skill levels

Document context when it affects rating.

### 5. Consensus Categories

**Stable** (≥90% agreement):
- Clear, unambiguous rating
- Consistent across contexts
- Example: Global mutable state is 🌀

**Mostly Stable** (75-89% agreement):
- Minor divergence on boundaries
- Context affects edge cases
- Example: React hooks (🧶 vs 🪢)

**Disputed** (50-74% agreement):
- Significant model disagreement
- Context heavily influences
- Example: GraphQL (🔍 vs 🔬)

**Unstable** (<50% agreement):
- No clear consensus
- Requires more analysis
- May indicate framework limitation

### 6. Documentation Requirements

Each assessed pattern must include:

```markdown
## Consensus Data
- Model Agreement: [percentage]%
- Models Tested: [list]
- Disputed Aspects: [specific divergences]
- Context Dependencies: [when ratings change]
- Test Date: [YYYY-MM-DD]
```

### 7. Reassessment Triggers

Reassess ratings when:
- New language/framework version
- Significant pattern evolution
- New models available
- Community reports divergence

## Common Assessment Errors

### Over-Rating Expressive Power
- Confusing "many ways" with "many valid ways"
- Missing subtle constraints that guide usage
- Not testing edge cases for guidance

### Under-Rating Context Flow
- Missing hidden dependencies
- Not considering state propagation
- Focusing only on explicit data flow

### Mis-Rating Error Surface
- Confusing error detection with error handling
- Not distinguishing cascade from containment
- Missing initialization-time validation

## Assessment Checklist

- [ ] Applied operational definitions
- [ ] Tested boundary cases
- [ ] Validated with 3+ models
- [ ] Documented consensus percentage
- [ ] Noted context dependencies
- [ ] Included test date
- [ ] Linked related patterns

## Navigation

- [Back to Notation](./README.md)
- [Formal Syntax](./formal-syntax.md)
- [View Examples](../corpus/patterns/)
