# VIBES-RFC-001: LLM Ergonomics

COMPATIBILITY = "Cross-Model Pattern Framework"  
VALIDATION_SCOPE = "Tested with: GPT-4.5, Claude Opus 4, Gemini 2.5, DeepSeek, Windsurf SWE-1"  


## The Ergonomics of AI Tool Design

This document provides a structured framework for evaluating and improving the ergonomics and affordances of tools and expression languages designed for LLM use.

Much like how humans can instinctively gauge the ergonomics of a tool by using it, LLMs can be asked to describe the experience of using a tool surface (MCP servers, expression languages designed to be written by LLMs, etc). In this manner we can optimize the ergonomics of our tools for use by LLMs, and we can model and create _new_ expression languages and architectures explicitly designed to maximize LLM ergonomics.

For the purposes of this document we will treat LLMs as black boxes. We will not attempt to engage with their inner workings. Instead, we will focus on what makes a tool surface (a repository, an expression language, etc) _ergonomic_ for an LLM. We will focus on what makes a tool have good ergonomics.

Better VIBES properties lead to clearer comprehension, predictable outputs, and improved human-AI collaboration.


---


## Vibes Coordinate System


This framework is qualitative and not quantitiatve. LLMs excel at answering qualitative questions, but if asked to compute a qualitative value (eg Kolmogorov Complexity) they will just make something up and optimizing for what sounds right. With that in mind, we have designed the 3-axis vibes coordinate system in this spec to embrace the strengths of LLMs - pattern recognition and natural language understanding—while avoiding reliance on computational analysis. The framework provides a shared vocabulary for discussing code quality in terms that align with how transformers process and understand code, not as a replacement for traditional static analysis tools.


The emoji-based ratings are intentionally qualitative, reflecting the probabilistic nature of LLM assessments rather than precise metrics. (TODO: 1-2 sentences introducing the axes, maybe mini-QRC?


---

## The Three Axes Explained

The three axes capture fundamental properties:
- **Expressive Power**: Language flexibility and constraint precision
- **Context Flow**: Dependency structure and traversal constraints  
- **Error Surface**: When failures occur in the system lifecycle

## EXPRESSIVE POWER: 🙈→👓→🔍→🔬

Measures how well a system allows expression of valid computations while precisely constraining invalid ones. High expressive power means multiple natural ways to say things correctly with minimal risk of saying things incorrectly.

**🔬 Crystalline (Precision-Adaptive)**
Rich expressiveness with context-aware constraints. 

- **Example**: Well-designed DSLs, Haskell with good libraries
- **Why it matters**: LLMs can express naturally while staying safe


**🔍 Structured (Guided Flexibility)**
Multiple natural ways to express ideas. Constraints act as guardrails that shape valid forms without blocking creativity.
- **Example**: Composable traits and typeclasses
- **Processing experience**: "I can express this naturally. The system guides me toward correctness."


**👓 Readable (Constrained Clarity)**
Rigid single paths. Constraints are blunt instruments. The system permits basic idioms with minimal flexibility. 
- **Example**: `add_floats(2.0, 2.0)` - functional but inflexible
- **Processing experience**: "I know the one way to do this. Boring but reliable."

**🙈 Noise (Blocked Expression)**
Cannot express needed computations. Constraints block everything. Valid expressions feel unreachable or require brittle workarounds. Processing friction is extreme - like searching through fog while hitting walls.
- **Example**: Stringly-typed APIs rejecting valid input formats
- **Why it matters**: Unusable - models can't say what they need


### CONTEXT FLOW: 🌀→🧶→🪢→🎀

**🎀 Independent (Tuple)**: No dependencies between components. Like tuple elements - access any without affecting others.
- **Example**: `(name, age, email)` - change any without affecting others
- **Pattern**: Pure functions, immutable data

**🪢 Pipeline (Linked List)**: Linear dependencies, immutable during traversal. Each step depends on previous but doesn't modify it.
- **Example**: `data |> validate |> transform |> save`
- **Pattern**: Functional pipelines, promise chains

**🧶 Coupled (Acyclic Graph)**: Complex dependencies without cycles. Hidden state mutations during traversal.
- **Example**: React components with shared context and effects
- **Why it matters**: Heartbleed - complex OpenSSL dependencies hid critical vulnerability

**🌀 Entangled (Cyclic Graph)**: Circular dependencies with feedback loops. Traversal order changes results.
- **Example**: Spreadsheet with circular references
- **Pattern**: Event systems with feedback, mutually recursive modules


### ERROR SURFACE: Failure Timing: 🌊→💧→🧊→💠

Measures when and how errors can occur in the system.

**💠 Crystal**: Errors impossible at parse/compile/type-check time. Like using types that make divide-by-zero unrepresentable. Mistakes cannot exist within the system.
- **Example**: `divide :: Int -> NonZeroInt -> Int`
- **Why it matters**: Therac-25 radiation overdoses - type safety could have prevented patient deaths

**🧊 Ice**: Errors contained at startup/initialization. Like dependency injection failures or configuration validation. Problems surface immediately at system boundaries.
- **Example**: `@Component class Service { Service(Required deps) {...} }`
- **Why it matters**: Fail fast at boot, not in production

**💧 Liquid**: Errors handled at runtime/execution. Like try-catch blocks or Result types. Failures occur during operation but have explicit handling.
- **Example**: `Result<User, Error> = fetchUser(id)`
- **Why it matters**: Graceful degradation vs silent corruption

**🌊 Ocean**: Errors cascade unpredictably across system. Like shared mutable state with race conditions. One failure triggers system-wide effects.
- **Example**: `window.APP.state.user = null // Crashes everywhere`
- **Why it matters**: NASA Mars Climate Orbiter - unit confusion cascaded through systems

---

## Pattern Ergonomics: How Patterns Create Processing Friction

Patterns create different levels of processing friction for transformers. Like testing how well a tool fits your hand, VIBES identifies which patterns flow naturally versus which create resistance.

**How Different Patterns Feel to Process:**

---

## Pattern Forces and Transformations

### How Axes Interact

Changing one axis creates forces on others:

**Improving Expressive Power** (🙈→🔬):
- Add alternative valid syntaxes and idioms
- Refine constraints to be precise scalpels, not hammers
- Enable terse and verbose expressions as appropriate
- Example: Allow both JSON and YAML, both `{"add": [2,2]}` and `add(2,2)`

**Improving Context Flow** (🌀→🎀):
- Break cycles into acyclic graphs
- Linearize complex graphs into pipelines
- Decouple pipelines into independent components
- Example: Event loops → Command pattern → Pure functions

**Improving Error Surface** (🌊→💠):
- Often enables more Expressive Power (precise constraints enable safe flexibility)
- May initially constrain Context Flow (types limit some patterns)
- Example: Adding type safety → clearer interfaces → more safe expression options

## Practical Assessment Guide

To assess a pattern's VIBES:

**For Expressive Power, ask:**
- Can you express what you need at all? (No → 🙈)
- Is there only one rigid way? → X
- Are there multiple natural expressions? → X
- Can you express anything valid with precise constraints? → 🔬

**For Context Flow, ask:**
- Can any component be accessed independently? → 🎀
- Do dependencies form a simple chain? → 🪢
- Are there branches but no cycles? → 🧶
- Do circular dependencies exist? → 🌀

**For Error Surface, ask:**
- Do types or syntax make errors impossible? → 💠
- Are errors caught at initialization/startup? → 🧊
- Are errors handled during runtime execution? → 💧
- Can one error trigger unpredictable failures? → 🌊

---

## Validation Methodology

This framework was developed through iterative cross-model testing:

1. **Pattern Corpus**: 28 code patterns + 9 delta transformations spanning multiple paradigms
2. **Model Testing**: Each pattern presented to GPT-4.5, Claude Opus 4, Gemini 2.5, and DeepSeek
3. **Assessment Protocol**: Models provided VIBES ratings with confidence levels and rationales
4. **Divergence Analysis**: When models disagreed:
   - Identified sources (training data, architecture, terminology)
   - Refined definitions to reduce ambiguity
   - Tested clarifications with models
5. **Iteration**: Specification updated based on convergence patterns

**Key Findings**:
- High agreement on anti-patterns (`<🙈🌀🌊>`)
- Divergence on effect systems and framework-specific patterns
- Terminology precision critical for consistent assessment

This methodology enables community validation and extension.
