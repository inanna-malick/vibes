# VIBES-RFC-001: LLM Ergonomics

COMPATIBILITY = "Cross-Model Pattern Framework"  
VALIDATION_SCOPE = "Tested with: GPT-4.5, Claude Opus 4, Gemini 2.5, DeepSeek, Windsurf SWE-1"  

## Core Principle: Tool Fit Over Introspection

VIBES measures processing friction, not neural architecture. Like testing if a hammer fits your hand, not trying to deduce your neural wiring from how it feels. Transformers don't introspect their inner states—they register affordance friction.

---

## What This Document Is

This specification provides a formal framework for evaluating and improving the ergonomics and affordances of tools and expression languages designed for LLM use by measuring computational fit between patterns and transformer processing.

**Important**: VIBES assessments are qualitative pattern recognition, not quantitative measurement. The framework helps identify and communicate pattern characteristics, not calculate precise metrics.

**Note:** VIBES describes pattern properties using tool ergonomics metaphors. Preference language means structural fit, not psychological states.

## LLM-Native Assessment Framework

VIBES is a pattern language for LLMs - explicitly designed for qualitative, pattern-based evaluation by language models. It embraces the strengths of LLMs—pattern recognition and natural language understanding—while avoiding reliance on computational analysis. The framework provides a shared vocabulary for discussing code quality in terms that align with how transformers process and understand code, not as a replacement for traditional static analysis tools. The emoji-based ratings are intentionally qualitative, reflecting the probabilistic nature of LLM assessments rather than precise metrics.

---

## The Three Axes Explained

### ERROR SURFACE: Failure Timing

Measures when and how errors can occur in the system.

**Most Common States**: 🧊 (startup validation) and 💧 (runtime handling)

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

### CONTEXT FLOW: Dependency Topology

Measures how tangled dependencies are and whether they mutate during traversal. Think of untangling Christmas lights - you're converting cycles into chains into separate strands.

**Most Common States**: 🪢 (pipelines) and 🧶 (complex but acyclic)

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

### EXPRESSIVE POWER: Can Models Say What They Need Safely?

Evaluates how much useful expressiveness a pattern enables without introducing risk. High expressive power means multiple natural ways to express the same computation, with constraints that prevent only truly invalid expressions.

**Most Common States**: 🧗 (minimal/rigid) and 🏔️ (good balance)

**🕳️ None**: Cannot express needed computations. Constraints block everything.
- **Example**: Config format that rejects all useful input
- **Why it matters**: Unusable - models can't say what they need

**🧗 Minimal**: Rigid single paths. Constraints are blunt instruments.
- **Example**: 
  ```xml
  <add><val>2</val><val>2</val></add>  <!-- Only way -->
  ```
- **Pattern**: Verbose XML schemas, overly restrictive formats

**🏔️ Good**: Multiple natural ways to express ideas. Constraints precisely target invalid states.
- **Example**: 
  ```sql
  -- Many valid ways to express the same query
  SELECT * FROM users WHERE age > 18;
  SELECT * FROM users WHERE 18 < age;
  SELECT name, email FROM users WHERE age > 18;
  -- But invalid syntax caught immediately
  SELECT * FROM users WHERE age > 'eighteen';  -- Error!
  ```
- **Why it matters**: LLMs can express naturally while staying safe

**🎯 Maximum**: Optimal expressive power with minimal necessary constraints.
- **Example**: Well-designed DSLs, Haskell with good libraries
- **Pattern**: Every valid computation expressible, constraints prevent only true errors

---

## Pattern Ergonomics: How Patterns Create Processing Friction

Patterns create different levels of processing friction for transformers. Like testing how well a tool fits your hand, VIBES identifies which patterns flow naturally versus which create resistance.

**How Different Patterns Feel to Process:**

| Pattern | VIBES | Processing Experience |
|---------|-------|----------------------|
| **🎯🎀💠** | Perfect language | Like playing jazz - infinite expression within perfect structure |
| **🏔️🪢🧊** | Well-designed API | Like using a good tool - natural grip, clear purpose |
| **🧗🌀🌊** | Rigid mess | Like bureaucratic form - one way to fail many ways |

> **Note**: This describes pattern fit, not transformer introspection. "Processing friction" is phenomenology, not architectural analysis.

---

## Pattern Forces and Transformations

### How Axes Interact

Changing one axis creates forces on others:

**Improving Expressive Power** (🕳️→🎯):
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

### Transformation Paths

**From CHAOTIC `<🕳️🌀🌊>` to HARMONIC `<🏔️🪢💠>`:**

1. **Stabilize Error Surface First** (🌊→💧)
   - Add error boundaries
   - Log failure points
   - Isolate unstable components

2. **Untangle Dependencies** (🌀→🧶)
   - Break circular references
   - Convert to directed acyclic graph
   - Make mutation points explicit

3. **Increase Expressive Power** (🧗→🏔️)
   - Add alternative idioms and syntaxes
   - Refine constraints to be precise, not blunt
   - Enable both terse and verbose expressions

4. **Iterate until reaching target state**

### Quick Diagnostics

**Signs of 🕳️ (No Power)**: Can't express needed computations, overly restrictive
**Signs of 🌀 (Cyclic Dependencies)**: Feedback loops, traversal order affects results, circular references
**Signs of 🌊 (Cascading Errors)**: Null crashes everywhere, defensive programming, try/catch layers

**Tokenization Warning**: Mathematical notation and special characters may degrade ratings due to subword splitting (e.g., `θ=μ/Σ` from 🔬→🔍)

---

## Preference Language Guidelines

✅ **VALID**: "Transformers prefer dense patterns"
   Means: Dense patterns align with training/architecture
   
❌ **INVALID**: "Transformers want simpler code"  
   Implies: Conscious desire or values

**Rule**: Preference = Structural Fit, not Psychological State

**Note**: "Preference" describes emergent system behavior, not consciousness - like "markets prefer liquidity" or "corporations want growth."

Examples:
- "LLMs prefer explicit dependencies" → Dependencies match processing model
- "Models favor type-safe patterns" → Types reduce prediction ambiguity
- "Transformers like functional patterns" → Functional style common in training

---

## Decision Matrices

### When to Sacrifice Expressive Power (🏔️) for Error Safety (💠)

|                          | High Criticality System | Low Criticality System |
|--------------------------|------------------------|------------------------|
| **Long-lived artifacts** | ✅ Always favor 💠<br>Precise constraints > flexibility | ⚖️ Balance (🏔️ + 💠)<br>Multiple safe paths |
| **Ephemeral artifacts**  | ⚖️ Constrained 🧗 with 💠<br>Limited but safe | ✅ Maximize 🏔️<br>Natural expression |

**Decision Triggers**:  
- Choose 💠 when: Invalid expressions could be catastrophic  
- Choose 🏔️ when: Developer/LLM ergonomics crucial, safety nets exist

---

## Pattern Interactions

### Common Correlations

While axes are independent, certain combinations appear frequently:

- 🏔️ (Good expressive power) often correlates with 💠 (Crystal errors) - flexible languages usually have precise validation
- 🧶 or 🌀 (Acyclic/Cyclic graphs) frequently leads to 🌊 (Ocean errors) - complex dependencies create cascading failure modes
- 🕳️ (No expressive power) makes other axes irrelevant - if nothing is expressible, nothing else matters

### Anti-Correlations

- 🌀 (Cyclic) cannot coexist with 💠 (Crystal) - circular dependencies prevent static guarantees
- 🕳️ (No power) cannot coexist with anything useful - no expressiveness means no program

---

## Handling Assessment Ambiguity

When patterns exhibit properties spanning categories based on specific conditions:
- Report primary rating with confidence level
- Note conditions causing variance
- Example: "🪢 (Pipeline) but approaches 🎀 with parallel scheduler"

---

## Practical Assessment Guide

To assess a pattern's VIBES:

**For Expressive Power, ask:**
- Can you express what you need at all? (No → 🕳️)
- Is there only one rigid way? → 🧗
- Are there multiple natural expressions? → 🏔️
- Can you express anything valid with precise constraints? → 🎯

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

---

## Summary

The three axes capture fundamental properties:
- **Expressive Power**: Language flexibility and constraint precision
- **Context Flow**: Dependency structure and traversal constraints  
- **Error Surface**: When failures occur in the system lifecycle

Better VIBES properties lead to clearer comprehension, predictable outputs, and improved human-AI collaboration.

---

## Appendix A: Pattern Reference Catalog

### Canonical Patterns

**Pure Function (Target State)**
```haskell
add :: Int -> Int -> Int
add x y = x + y
```
VIBES: `<🎯🎀💠>` - Maximum expressive power, any evaluation order, type-guaranteed safety

**Configuration as Code**
```yaml
deploy:
  when: $BRANCH == "main"
  steps: [build, test, deploy]
```
VIBES: `<🏔️🎀🧊>` - Good expressiveness, independent sections, validated at load

**Dependency Injection**
```java
@Component
class Service {
  Service(Repository repo, Logger log) {
    this.repo = repo;
    this.log = log;
  }
}
```
VIBES: `<🧗🪢🧊>` - Minimal expression options, sequential construction, startup validation

**Callback Hell (Anti-pattern)**
```javascript
getData(function(a) {
    getMoreData(a, function(b) {
        getMoreData(b, function(c) {
            // deeply nested
        });
    });
});
```
VIBES: `<🧗🧶💧>` - Rigid nesting pattern, coupled execution, runtime error handling

**Global Singleton (Anti-pattern)**
```python
class Database:
    _instance = None
    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance
```
VIBES: `<🕳️🌀🌊>` - No expressive power, global coupling, cascading failures

---

## Appendix C: Anti-Pattern Gallery

### The Everything Object
```javascript
window.APP = {
  state: {},
  config: {},
  utils: {},
  // 500 more properties
};
```
VIBES: `<🕳️🌀🌊>` - Every axis degraded

**Recovery Path**:
1. Extract modules (🌀→🧶)
2. Define interfaces (🧶→🪢)
3. Add types (🌊→💧)

### The Magic String
```python
if user_input == "PROD_MODE_OVERRIDE_ADMIN_BYPASS_2024":
    grant_full_access()
```
VIBES: `<🧗🧶🌊>` - Rigid auth check, coupled logic, cascading security risk

**Recovery Path**:
1. Add multiple auth methods (🧗→🏔️)
2. Add validation (🌊→💧)
3. Use proper auth (🧶→🪢)
