# Contributing to VIBES

Thank you for helping improve the VIBES framework! This document explains how to contribute effectively.

## Quick Start

1. **Found a pattern?** Add it to [patterns/](./patterns/)
2. **Discovered a transformation?** Document it in [transformations/](./transformations/)
3. **Built something cool?** Share it in [examples/](./examples/)
4. **Spotted an error?** Open an issue or submit a fix

## Contribution Types

### Adding Patterns

Patterns require cross-model validation:

1. Create pattern file in appropriate directory:
   - `patterns/anti-patterns/` for `<🙈🌀🌊>` disasters
   - `patterns/crystalline/` for `<🔬🎀💠>` excellence
   - Others based on primary characteristic

2. Follow the pattern template:
```markdown
# Pattern Name

**Rating**: `<🔍🪢💠>`
**Domain**: API Design, State Management, etc.
**Also Known As**: Alternative names

## The Pattern
[Brief description]

## Code Example
[Minimal working example]

## VIBES Analysis
**Expressive (🔍)**: [Rationale]
**Context (🪢)**: [Rationale]  
**Error (💠)**: [Rationale]

## Validation
Tested with: GPT-4, Claude, Gemini, DeepSeek
Agreement: 4/4 (unanimous)
```

3. Include validation data showing model consensus

### Adding Transformations

Transformations show paths between patterns:

1. Place in `transformations/` organized by:
   - Starting point: `from-noise/`, `from-ocean/`, etc.
   - Goal: `to-clarity/`, `to-crystal/`, etc.

2. Include:
   - Step-by-step progression with intermediate states
   - Rationale for each step
   - Working code at each stage
   - Tests that pass throughout

### Validation Requirements

All contributions need validation:

1. **Pattern Validation**:
   - Test with 4+ current LLM models
   - Document ratings and rationales
   - Note any disagreements
   - Calculate consensus percentage

2. **Transformation Validation**:
   - Each step must preserve functionality
   - Include tests or verification method
   - Show VIBES progression at each stage

3. **Example Validation**:
   - Code must be runnable/compilable
   - Include setup instructions
   - Show before/after VIBES ratings

## Submission Process

### For Small Contributions

1. Fork the repository
2. Create feature branch: `git checkout -b add-pattern-name`
3. Make changes following templates
4. Validate with multiple models
5. Submit pull request with:
   - Clear description
   - Validation results
   - Links to related patterns

### For Large Contributions

1. Open an issue first to discuss
2. Get feedback on approach
3. Follow small contribution process
4. Be prepared for iteration

## Quality Guidelines

### Code Examples

- **Minimal**: Show only what's needed
- **Realistic**: Use actual patterns not toy examples
- **Runnable**: Include imports and setup
- **Universal**: Avoid niche frameworks when possible

### Writing Style

- **Clear**: Assume LLM readers with varying context
- **Concise**: Every word should add value
- **Consistent**: Follow existing patterns
- **Linked**: Connect related concepts

### Validation Data

Include in your PR:

```yaml
validation:
  models:
    - name: GPT-4
      version: "2024-08-01"
      rating: "<🔍🪢💠>"
      confidence: high
    - name: Claude 3.5
      version: "2024-06-20"
      rating: "<🔍🪢💠>"
      confidence: high
  consensus: "4/4 unanimous"
  notes: "All models noted the type safety benefits"
```

## What We're Looking For

### High-Value Patterns

- **Cross-paradigm**: Patterns that transcend languages
- **Transformative**: Patterns that drastically improve VIBES
- **Common pitfalls**: Anti-patterns many developers hit
- **Excellence examples**: Patterns approaching `<🔬🎀💠>`

### Needed Documentation

- Framework-specific patterns (React, Django, Rails)
- Domain-specific patterns (ML, Games, Embedded)
- Transformation case studies with metrics
- LLM-specific optimizations

## Community Standards

### Be Inclusive

- Welcome newcomers
- Explain jargon
- Provide context
- Link to resources

### Be Rigorous

- Test thoroughly
- Document edge cases
- Acknowledge limitations
- Cite sources

### Be Collaborative

- Build on others' work
- Credit contributors
- Share validation data
- Iterate based on feedback

## Recognition

Contributors are recognized through:

- [CONTRIBUTORS.md](./CONTRIBUTORS.md) listing
- Pattern attribution in files
- Monthly highlight of top contributions
- Invitation to VIBES working group

## Questions?

- Open an issue for clarification
- Join [VIBES Discord](#) for discussion
- Email vibes@example.com for sensitive matters

## License

By contributing, you agree that your contributions will be licensed under the same [CC BY 4.0](./LICENSE.md) license as the project.

---

Remember: VIBES is about making tools better for LLMs. Every contribution helps the entire ecosystem level up.
