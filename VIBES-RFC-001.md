# VIBES-RFC-001: LLM Ergonomics

COMPATIBILITY = "Cross-Model Pattern Framework"  
VALIDATION_SCOPE = "Tested with: GPT-4.5, Claude Opus 4, Gemini 2.5, DeepSeek, Windsurf SWE-1"  


## The Ergonomics of AI Tool Design

This document provides a structured framework for evaluating and improving the ergonomics and affordances of tools and expression languages designed for LLM use.

Much like how humans can instinctively gauge the ergonomics of a tool by using it, LLMs can be asked to describe the experience of using a tool surface (MCP servers, expression languages designed to be written by LLMs, etc). In this manner we can optimize the ergonomics of our tools for use by LLMs, and we can model and create _new_ expression languages and architectures explicitly designed to maximize LLM ergonomics.

**Tool Fit Over Introspection**: VIBES does not attempt to model internal transformer states. It treats models as black boxes and focuses only on observable friction between external patterns and LLM behavior. We measure processing friction, not neural architecture—asking "how does this pattern feel to work with?" rather than "how does the model internally process this?"

Better VIBES properties lead to clearer comprehension, predictable outputs, and improved human-AI collaboration.

## Why VIBES Matters

Bad ergonomics waste 40-60% of LLM capacity on failed attempts and workarounds. Good VIBES can mean the difference between 70% and 95% task completion rates. Better VIBES → Higher completion rates → Fewer retries → Faster development.

---


## Vibes Coordinate System


This framework is qualitative and not quantitiatve. LLMs excel at answering qualitative questions, but if asked to compute a qualitative value (eg Kolmogorov Complexity) they will just make something up and optimizing for what sounds right. With that in mind, we have designed the 3-axis vibes coordinate system in this spec to embrace the strengths of LLMs - pattern recognition and natural language understanding—while avoiding reliance on computational analysis. The framework provides a shared vocabulary for discussing code quality in terms that align with how transformers process and understand code, not as a replacement for traditional static analysis tools.


The emoji-based ratings are intentionally qualitative, reflecting the probabilistic nature of LLM assessments rather than precise metrics.

### VIBES Quick Reference
| Axis | States | What It Measures |
|------|--------|------------------|
| **Expressive** | 🙈 👓 🔍 🔬 | How many valid ways to express ideas |
| **Context Flow** | 🌀 🧶 🪢 🎀 | How tangled dependencies are |
| **Error Surface** | 🌊 💧 🧊 💠 | When errors can occur in lifecycle |

Notation: `<Expressive/Context/Error>` e.g., `<🔍🪢💠>`

---

## The Three Axes Explained

The three axes capture fundamental properties:
- **Expressive Power**: Language flexibility and constraint precision
- **Context Flow**: Dependency structure and traversal constraints  
- **Error Surface**: When failures occur in the system lifecycle

## EXPRESSIVE POWER: 🙈→👓→🔍→🔬

Measures how well a system allows expression of valid computations while precisely constraining invalid ones. High expressive power means multiple natural ways to say things correctly with minimal risk of saying things incorrectly.

**🔬 Crystalline (Precision-Adaptive)**
Rich expressiveness with context-aware constraints. Semantic density—where tokens carry high meaning-per-bit—emerges most clearly here.

- **Example**: Well-designed DSLs, Haskell with good libraries
- **Why it matters**: LLMs can express naturally while staying safe


**🔍 Structured (Guided Flexibility)**
Multiple natural ways to express ideas. Constraints act as guardrails that shape valid forms without blocking creativity.
- **Example**: Composable traits and typeclasses
- **Processing experience**: Multiple valid expressions exist; constraints prevent invalid forms


**👓 Readable (Constrained Clarity)**
Rigid single paths. Constraints are blunt instruments. The system permits basic idioms with minimal flexibility. 
- **Example**: `add_floats(2.0, 2.0)` - functional but inflexible
- **Processing experience**: Single valid syntax is well-understood and predictable

**🙈 Noise (Blocked Expression)**
Cannot express needed computations. Constraints block everything. Valid expressions are rejected; working code requires extensive trial-and-error and brittle workarounds.
- **Example**: Stringly-typed APIs rejecting valid input formats
- **Why it matters**: Unusable - models can't say what they need


### CONTEXT FLOW: 🌀→🧶→🪢→🎀

**🎀 Independent (Tuple)**: No dependencies between components. Components can be accessed in any order without side effects.
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

**💠 Crystal**: Errors impossible at parse/compile/type-check time. Type system prevents invalid states from being constructed. Mistakes cannot exist within the system.
- **Example**: `divide :: Int -> NonZeroInt -> Int`
- **Why it matters**: Therac-25 radiation overdoses - type safety could have prevented patient deaths

**🧊 Ice**: Errors contained at startup/initialization. Dependency injection failures or configuration validation surface immediately at system boundaries.
- **Example**: `@Component class Service { Service(Required deps) {...} }`
- **Why it matters**: Fail fast at boot, not in production

**💧 Liquid**: Errors handled at runtime/execution. Try-catch blocks or Result types handle failures during operation explicitly.
- **Example**: `Result<User, Error> = fetchUser(id)`
- **Why it matters**: Graceful degradation vs silent corruption

**🌊 Ocean**: Errors cascade unpredictably across system. Shared mutable state and race conditions mean one failure triggers system-wide effects.
- **Example**: `window.APP.state.user = null // Crashes everywhere`
- **Why it matters**: NASA Mars Climate Orbiter - unit confusion cascaded through systems

---

## VIBES in Action: Configuration System Evolution

**Starting Point** `<🙈🌀🌊>`:
```xml
<config>
  <property name="timeout">${ENV_TIMEOUT:-${FALLBACK_TIMEOUT:-30}}</property>
  <import file="${CONFIG_DIR}/db.xml" if="${USE_DB}"/>
</config>
```
Circular imports, string interpolation, runtime failures everywhere.

**After VIBES Transformation** `<🔍🪢💠>`:
```typescript
const config = z.object({
  timeout: z.number().min(1).default(30),
  database: z.optional(databaseSchema)
}).parse(loadConfig());
```

**What Each Axis Contributed:**
- **Expressive** (🙈→🔍): From string soup to multiple valid config formats (JSON/YAML/env)
- **Context** (🌀→🪢): From circular imports to linear validation pipeline
- **Errors** (🌊→💠): From runtime explosions to parse-time guarantees

---

## Pattern Ergonomics: How Patterns Create Processing Friction

Different VIBES coordinates create distinct processing experiences:

`<🔬🎀💠>` **Crystalline Harmony**: Multiple valid paths, all safe. Multiple valid approaches achieve the same result safely.

`<🔍🪢🧊>` **Structured Flow**: Clear pipeline with flexibility. Each processing step has clear input/output with flexible internals.

`<👓🧶💧>` **Brittle Complexity**: One path through tangled dependencies. Global state changes affect seemingly unrelated components.

`<🙈🌀🌊>` **Chaotic Noise**: No valid expressions, circular dependencies, cascading failures. String parsing fails unpredictably; components reference each other circularly.

---

## Pattern Forces and Transformations

### How Axes Interact

Changing one axis creates forces on others. Here are concrete transformation paths:

**Path: Chaotic to Structured**  
`<🙈🌀🌊>` → `<👓🧶💧>` → `<🔍🪢🧊>`

1. **Stabilize Errors First** (🌊→💧)
   - Add error boundaries around unstable components
   - Example: Wrap all API calls in try-catch

2. **Then Untangle Dependencies** (🌀→🧶)
   - Break circular refs into one-way data flow
   - Example: Replace callbacks with event emitter

3. **Finally Increase Expressiveness** (🙈→🔍)
   - Now safe to add alternative syntaxes
   - Example: Support both REST and GraphQL

**Quick Transformations:**
- `🔍🧶💧` → Add types → `🔬🪢🧊`
- `👓🌀🌊` → Extract modules → `👓🧶💧`
- `🔬🪢💧` → Add parse layer → `🔬🪢💠`

## Common Anti-Pattern Gallery

**Everything Object** (Current: `<🙈🌀🌊>`)  
Goal: `<🔍🎀🧊>`
1. Extract coherent modules (🌀→🎀)
2. Define clear interfaces (🙈→🔍)
3. Add type guards at boundaries (🌊→🧊)

**Magic String Soup** (Current: `<🙈🧶🌊>`)  
Goal: `<🔍🪢💠>`
1. Introduce constants/enums (🙈→👓)
2. Convert to typed structures (🧶→🪢)
3. Parse once, use everywhere (🌊→💠)

**Callback Hell** (Current: `<👓🌀💧>`)  
Goal: `<🔍🪢🧊>`
1. Flatten with promises/async (🌀→🪢)
2. Add error boundaries (💧→🧊)
3. Enable multiple syntaxes (👓→🔍)

## Practical Assessment Guide

To assess a pattern's VIBES:

**For Expressive Power, ask:**
- Can you express what you need at all? (No → 🙈)
- Is there only one rigid way? → 👓
- Are there multiple natural expressions? → 🔍
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
