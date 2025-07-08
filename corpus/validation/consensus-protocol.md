# VIBES Consensus Protocol

## Overview

This protocol establishes how the VIBES community achieves reliable cross-model consensus on pattern ratings while preserving valuable divergence insights.

## Core Principles

1. **Polycentric Validation**: No single model or evaluator has authority
2. **Transparent Divergence**: Disagreements are as valuable as agreements
3. **Contextual Sensitivity**: Same pattern may rate differently in different contexts
4. **Temporal Awareness**: Ratings may evolve as tools and models evolve

## Consensus Calculation

### Basic Formula
```
Consensus Score = (Number of Agreeing Models / Total Models) × 100%

Agreement Levels:
- Strong Consensus: ≥90% agreement
- Moderate Consensus: 70-89% agreement  
- Weak Consensus: 50-69% agreement
- No Consensus: <50% agreement
```

### Axis-Specific Agreement
Calculate consensus per axis, not just overall rating:
```
Pattern X:
- Expressive: 4/4 models agree on 🔍 (100% consensus)
- Context: 3/4 models say 🧶, 1 says 🪢 (75% consensus)
- Error: 4/4 models agree on 💧 (100% consensus)
Overall: Moderate consensus with documented context flow divergence
```

## Assessment Protocol

### 1. Initial Assessment Round
```yaml
pattern: promise-pipeline
models_tested:
  - name: GPT-4
    version: "latest as of test date"
    date: 2025-07-20
    rating: <🔍🪢🧊>
    
  - name: Claude
    version: "latest as of test date"  
    date: 2025-07-20
    rating: <🔍🪢🧊>
    
  - name: Gemini
    version: "latest as of test date"
    date: 2025-07-20
    rating: <🔍🪢💧>  # Divergence on error surface
    
  - name: DeepSeek
    version: "latest as of test date"
    date: 2025-07-20
    rating: <🔍🪢🧊>
```

### 2. Divergence Investigation
When models disagree, document:
- **Rationale**: Why did Model X rate differently?
- **Context**: What assumptions influenced the rating?
- **Validity**: Is this a reasonable alternative interpretation?

Example:
```yaml
divergence:
  axis: Error Surface
  majority: 🧊 (Ice - startup validation)
  minority: 💧 (Liquid - runtime handling)
  
  gemini_rationale: "Promise rejection is still runtime, even if construction validates"
  
  validity: Reasonable - depends on whether we consider promise construction or rejection timing
  
  resolution: Document both interpretations as context-dependent
```

### 3. Context-Dependent Ratings
Some patterns legitimately have multiple ratings:

```yaml
pattern: react-hooks
contexts:
  - context: "Small component with 1-2 hooks"
    rating: <🔍🪢💧>
    consensus: 95%
    
  - context: "Complex component with 10+ hooks and dependencies"  
    rating: <👓🧶💧>
    consensus: 85%
    
  - context: "Custom hook composition"
    rating: <🔬🪢💧>
    consensus: 90%
```

## Validation Workflow

### Step 1: Prepare Test Case
```markdown
Pattern: [Name]
Language: [Language/Framework]
Context: [Specific use case or general]
Code Sample: [Minimal reproducible example]
```

### Step 2: Gather Assessments
Present to 3+ models with standardized prompt:
```
Please assess this code pattern using VIBES ratings:
- Expressive Power (🙈→👓→🔍→🔬): How well does it guide toward valid expressions?
- Context Flow (🌀→🧶→🪢→🎀): Does traversal order change meaning?
- Error Surface (🌊→💧→🧊→💠): When are invalid states detected?

Provide rationale for each axis rating.

[Code sample]
```

### Step 3: Calculate Consensus
1. Tally ratings per axis
2. Identify majority ratings
3. Calculate percentage agreement
4. Document divergences

### Step 4: Investigation Phase
For divergences:
1. Re-query divergent models for clarification
2. Test boundary cases
3. Consult domain experts if needed
4. Document findings

### Step 5: Publication
```yaml
# patterns/example-pattern.md
## Consensus Data
- Model Agreement: 87.5%
- Models Tested: GPT-4, Claude, Gemini, DeepSeek
- Divergence: Gemini rates Context as 🌀 due to event system interpretation
- Context Dependencies: Rating assumes single-file scope
- Test Date: 2025-07-20
```

## Handling Edge Cases

### New Models
When new models become available:
1. Re-test high-value patterns (toxic and crystalline examples)
2. Compare against existing consensus
3. Document any systematic biases
4. Update consensus scores if significant changes

### Framework Evolution
When languages/frameworks update:
1. Flag patterns for re-evaluation
2. Test with new version
3. Document temporal changes
4. Maintain version-specific ratings if needed

### Cultural/Regional Differences
Some patterns may rate differently based on:
- Programming culture (enterprise vs startup)
- Regional practices (US vs European approaches)
- Industry domain (fintech vs gaming)

Document these as context variations, not failures of consensus.

## Quality Criteria

### Good Consensus Data
- Tests recent model versions
- Includes 4+ diverse models
- Documents clear rationales
- Investigates divergences
- Notes context dependencies
- Updates periodically

### Poor Consensus Data
- Only 2 models tested
- No rationale provided
- Ignores divergences
- Assumes universal rating
- Never updated
- Uses outdated models

## Consensus Interpretation Guide

### Strong Consensus (≥90%)
- Pattern rating is reliable
- Safe to use as canonical example
- Divergence likely due to minor interpretation differences

### Moderate Consensus (70-89%)
- Pattern has mainstream interpretation
- Note minority viewpoints
- Consider context dependencies
- Useful with caveats

### Weak Consensus (50-69%)
- Pattern is ambiguous or context-dependent
- Document multiple valid interpretations
- Not suitable as canonical example
- Valuable for teaching boundaries

### No Consensus (<50%)
- Pattern may be:
  - Too new/experimental
  - Culturally specific
  - Domain-dependent
  - Poorly specified
- Requires further investigation
- May indicate framework limitations

## Meta-Consensus

This protocol itself should be validated:
1. Test with multiple models
2. Gather feedback from practitioners
3. Iterate based on usage
4. Document what works and what doesn't

The protocol succeeds when:
- Practitioners can reliably assess patterns
- Divergences are informative, not confusing
- Context dependencies are clear
- Evolution is tracked effectively

## Navigation

- [Back to Validation](./README.md)
- [Assessment Template](./assessment-template.md)
- [Model Comparison](./model-comparison.md)
