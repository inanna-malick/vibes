# VIBES: Visual Intuitive Benchmarks for Expression Systems

A framework for evaluating and improving the affordances of tools and expression languages designed for LLM use.

## What is VIBES?

VIBES provides a 3-axis qualitative system for measuring what actions tools and languages afford to LLMs:
- **Expressive Power** (🙈→👓→🔍→🔬): How well systems afford valid expressions
- **Context Flow** (🌀→🧶→🪢→🎀): Whether patterns afford order-independent traversal
- **Error Surface** (🌊→💧→🧊→💠): When patterns afford error detection

## Quick Start

1. **Understand the Framework**: Read [VIBES-AXES.md](./VIBES-AXES.md) for core definitions
2. **Learn Assessment**: Check [GLOSSARY.md](./GLOSSARY.md) for visual reference
3. **See Examples**: Browse [corpus/patterns/](./corpus/patterns/) for rated patterns
4. **Transform Code**: Follow [guides/transformation-order.md](./guides/transformation-order.md)

## Repository Structure

```
vibespec/
├── README.md                           # This file
├── VIBES-RFC-001.md                   # Core specification
├── VIBES-AXES.md                      # Detailed axis definitions
├── GLOSSARY.md                        # Visual emoji reference
│
├── notation/                          # Formal syntax and semantics
│   ├── formal-syntax.md              # Coordinate notation rules
│   ├── assessment-protocol.md        # How to assess ratings
│   ├── axis-interactions.md          # Emergent patterns
│   └── boundary-examples.md          # Clarifying edge cases
│
├── corpus/                           # Pattern library
│   ├── patterns/                     # Individual patterns (7 examples)
│   │   ├── PATTERN_TEMPLATE.md      
│   │   ├── kubernetes-yaml.md       # <🙈🧶🌊>
│   │   ├── promise-pipeline.md      # <🔍🪢🧊>
│   │   ├── parser-combinators.md    # <🔬🪢💠>
│   │   └── ...                     
│   └── transformations/             # Before/after examples
│       └── global-to-modular.md    # <🙈🌀🌊> → <🔍🎀🧊>
│
├── guides/                          # Improvement guidance
│   ├── assessment.md               # How to rate consistently
│   ├── transformation-order.md     # Optimal improvement sequence
│   └── good-to-great.md           # Excellence patterns
│
└── examples/                       # Example languages/tools
    ├── vibes-lang/                # Emoji semantic domains
    └── llm-tools/                 # LLM-specific designs
        ├── llm-cli.md            # Semantic CLI
        ├── semantic-api.md       # Multi-path REST
        ├── config-lang.md        # Self-validating config
        └── error-domains.md      # Visual error systems
```

## Core Insights

**LLMs and humans need fundamentally different tools.** Just as we don't expect humans to write assembly or CPUs to parse English, we shouldn't force LLMs to use human-optimized interfaces.

**Transformation Order Matters**: Always improve in sequence:
1. Stabilize Errors (🌊→💧→🧊→💠)
2. Untangle Dependencies (🌀→🧶→🪢→🎀)  
3. Build Conceptual Model (🙈→👓→🔍→🔬)

**Better VIBES = Higher completion rates, fewer retries, faster development.**

## Assessment Example

```
Kubernetes YAML: <🙈🧶🌊>
- 🙈: Affords any string input, no conceptual model
- 🧶: Affords hidden cross-file dependencies  
- 🌊: Affords runtime failures that cascade unpredictably

Promise Pipeline: <🔍🪢🧊>
- 🔍: Affords clear conceptual model that guides usage
- 🪢: Affords linear dependency chain
- 🧊: Affords error detection at promise construction
```

## TODO: Future Expansions

### Pattern Corpus Expansion
- [ ] Add more domain-specific patterns:
  - [ ] Web framework patterns (React, Vue, Angular)
  - [ ] Database query patterns (SQL, GraphQL, ORMs)
  - [ ] Infrastructure patterns (Terraform, Docker, K8s)
  - [ ] ML pipeline patterns (data processing, training loops)
- [ ] Add patterns for emerging languages (Rust, Zig, etc.)
- [ ] Document anti-patterns from popular libraries

### Transformation Library
- [ ] Add more complete transformation journeys
- [ ] Create domain-specific transformation guides
- [ ] Document "quick win" single-axis improvements
- [ ] Add transformation cost/benefit analysis

### Tool Integration
- [ ] Create linter rules for common anti-patterns
- [ ] Build IDE plugins for VIBES annotations
- [ ] Develop CI/CD integration guides
- [ ] Create automated assessment tools

### Framework Extensions
- [ ] Explore additional axes (Performance? Discoverability?)
- [ ] Develop quantitative validation methodology
- [ ] Create cross-model consensus protocols
- [ ] Build VIBES assessment training materials

### Community Building
- [ ] Establish contribution guidelines
- [ ] Create VIBES certification/training
- [ ] Build pattern submission workflow
- [ ] Develop consensus validation tools

### Research Directions
- [ ] Correlate VIBES ratings with LLM success rates
- [ ] Study temporal degradation patterns
- [ ] Investigate cultural/linguistic variations
- [ ] Explore domain-specific adaptations

## Contributing

Test patterns with multiple LLMs, document consensus and divergence, follow templates in each directory, and link related patterns. See individual directory READMEs for specific guidelines.

## Navigation

- [Axis Definitions](./VIBES-AXES.md)
- [Visual Glossary](./GLOSSARY.md)
- [Pattern Library](./corpus/patterns/)
- [Transformation Guides](./guides/)
- [Example Tools](./examples/)

## Status

VIBES-RFC-001 represents the initial framework specification. Core concepts are fully developed with representative examples across the pattern spectrum. The framework is ready for use while remaining open for community expansion in the marked TODO areas.

**Validation**: Tested with latest models from Anthropic, OpenAI, Google, and others as of July 2025.

---

*The goal isn't to document every pattern, but to provide enough examples that the community can recognize and contribute new ones. VIBES is a living framework designed to evolve with our understanding of what LLM tools afford.*
