# VIBES FAQ

Common questions about the Variance in Behavior Ergonomics Specification.

## General Questions

### What is VIBES?

VIBES is a framework for evaluating how well tools and languages work for Large Language Models (LLMs). It uses three axes - Expressive Power, Context Flow, and Error Surface - to measure "ergonomics" from an LLM's perspective.

### Why do LLMs need different tools than humans?

Just as we don't expect humans to write assembly code or CPUs to parse English, LLMs process information differently than humans. They excel at pattern matching across large contexts but struggle with hidden state, circular dependencies, and runtime errors.

### Is this meant to replace human-centered design?

No! VIBES advocates for purpose-built tools for each user type. Human tools and LLM tools can coexist, just like GUIs and CLIs serve different needs.

### Who created VIBES?

VIBES emerged from cross-model consensus testing and community observation of what makes LLMs succeed or fail with different code patterns.

## Technical Questions

### How are ratings determined?

Ratings emerge from consensus across multiple LLMs. We present code patterns to 4+ models and document their assessments. Patterns with 75%+ agreement are considered stable.

### What do the emojis mean?

Each axis uses emoji progression to show improvement:
- Expressive: 🙈 (chaos) → 👓 (rigid) → 🔍 (guided) → 🔬 (crystalline)
- Context: 🌀 (circular) → 🧶 (tangled) → 🪢 (linear) → 🎀 (independent)
- Error: 🌊 (cascading) → 💧 (runtime) → 🧊 (startup) → 💠 (compile-time)

### Can I use decimal ratings like 3.7?

No. VIBES uses discrete levels to avoid false precision. The qualitative distinctions between levels are more important than numeric gradations.

### How do I rate my code?

1. Assess each axis using the [guides](./axes/)
2. Combine into notation like `<🔍🪢💠>`
3. Validate with multiple LLMs if possible
4. Document your rationale

## Practical Questions

### Which axis should I prioritize?

It depends on your domain:
- **Safety-critical**: Error Surface (💠) first
- **Interactive tools**: Expressive Power (🔬) first
- **Data pipelines**: Context Flow (🎀) first
- **General**: Balance all three

### Can code be "too crystalline"?

Yes. `<🔬🎀💠>` might be over-engineered for prototypes or scripts. VIBES describes capabilities, not prescriptions. Choose ratings appropriate for your context.

### How do I improve my code's VIBES?

Follow the transformation order:
1. Stabilize errors (🌊→💧)
2. Untangle dependencies (🌀→🪢)
3. Build conceptual model (🙈→🔍)

See [transformations](./transformations/) for detailed guides.

### Do I need to achieve `<🔬🎀💠>`?

No! Many successful systems operate at `<🔍🪢💧>`. The goal is appropriate VIBES for your use case, not maximum ratings.

## Philosophical Questions

### Is VIBES prescriptive or descriptive?

Descriptive. VIBES provides vocabulary for discussing patterns that already exist. It doesn't mandate specific approaches.

### Why "ergonomics" instead of "quality"?

Ergonomics emphasizes fit between tool and user. What's ergonomic for LLMs might be awkward for humans, and vice versa. Quality implies universal goodness.

### Will VIBES become obsolete as LLMs improve?

VIBES evolves with LLM capabilities. As models improve, patterns may shift ratings, but the axes remain relevant for measuring tool-user fit.

### Does VIBES work for non-code domains?

The principles apply to any structured information LLMs process - configuration, documentation, prompts. The specific patterns would differ.

## Contributing Questions

### How can I contribute?

See [CONTRIBUTING.md](./CONTRIBUTING.md). We especially need:
- New patterns with validation
- Transformation case studies
- Framework-specific examples
- Cross-model testing data

### My models disagree on ratings. What do I do?

Document the disagreement! Split ratings often reveal interesting boundary cases. Include all perspectives in your contribution.

### Can I create VIBES tools?

Yes! VIBES is CC BY 4.0 licensed. We'd love to see:
- Linters that detect anti-patterns
- IDE plugins for VIBES annotations
- Automated rating tools
- Visualization systems

## Implementation Questions

### How do I annotate my code?

Use comments with VIBES notation:
```javascript
// @vibes: <🔍🪢💠>
function parseConfig(input: string): Config { ... }
```

### Should I refactor all `<🙈🌀🌊>` code immediately?

No. Prioritize code that:
- LLMs frequently work with
- Causes repeated failures
- Is core to your system
- You're already refactoring

### How do I explain VIBES to my team?

Start with concrete examples:
1. Show a `<🙈🌀🌊>` anti-pattern they recognize
2. Transform it step by step
3. Demonstrate improved LLM success rate
4. Introduce the framework

## Meta Questions

### Is VIBES itself well-designed for LLMs?

We try! This documentation aims for `<🔍🎀💧>`:
- Clear conceptual model (🔍)
- Browsable structure (🎀)
- Explicit error handling (💧)

### What's next for VIBES?

- Expanded pattern library
- Quantitative validation
- Tool ecosystem
- Academic research
- Integration with LLM training

### Where can I learn more?

- [Philosophy](./philosophy/) - Deeper thinking
- [Examples](./examples/) - See it in action
- [Research](./research/) - Academic work
- [Discord](#) - Community discussion

---

Still confused? Open an issue! VIBES improves through community insight.
