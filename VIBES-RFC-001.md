# VIBES-RFC-001: LLM Ergonomics

VALIDATION_SCOPE = "Tested with: GPT-4.5, Claude 4 Opus, Gemini 2.5 Pro, DeepSeek V2"  

## 1. Introduction

VIBES provides a structured framework for evaluating and improving the ergonomics of tools and expression languages designed for LLM use. As LLM-driven development becomes mainstream, the economic impact of poor tool ergonomics compounds exponentially through failed attempts and workarounds.

**Core Insight**: LLMs and humans need fundamentally different tools. Just as we don't expect humans to write assembly code or CPUs to parse English, we shouldn't force LLMs to use human-optimized interfaces. The most effective approach is building purpose-specific tools for each type of user.

**The Framework**: VIBES uses a 3-axis qualitative system that embraces LLM strengths—pattern recognition and natural language understanding—rather than computational metrics. It treats models as black boxes, measuring processing friction rather than internal states.

**Why It Works**: VIBES describes patterns that already exist in well-engineered code. Every principle maps to established wisdom (type safety, functional programming, loose coupling). Future LLMs will naturally understand VIBES because they are trained on codebases embodying these principles.

## 2. The Three Axes

### VIBES Quick Reference
| Axis | States | What It Measures |
|------|--------|------------------|
| **Expressive** | 🙈 👓 🔍 🔬 | How many valid ways to express ideas |
| **Context Flow** | 🌀 🧶 🪢 🎀 | How tangled dependencies are |
| **Error Surface** | 🌊 💧 🧊 💠 | When errors can occur in lifecycle |

**Emoji Logic**: 
- Expressive: From blindness (🙈) to microscopic precision (🔬)
- Context: From chaotic swirl (🌀) to neat bow (🎀)  
- Error: From vast ocean (🌊) to crystallized/frozen (💠)

Notation: `<Expressive/Context/Error>` e.g., `<🔍🪢💠>`

### 2.1 Validation Methodology

Framework developed through iterative testing of multiple patterns across GPT-4.5o, Claude 4 Opus, Gemini 2.5 Pro, and DeepSeek V2. **VIBES ratings represent consensus patterns**—a pattern achieving 3/4 model agreement receives that rating.

**Critical Distinction**:
- **VIBES Assessment** (Qualitative): LLMs rate patterns based on interaction experience
- **Impact Validation** (Quantitative): Humans measure retry rates, completion times to verify correlation

**Example Divergence**: GPT-4o rated Redux components 🧶 (Coupled), Claude rated 🪢 (Pipeline); resolved by documenting both perspectives—external state management creates coupling even with unidirectional flow.

See `calibration/CALIBRATION_CORPUS.md` for the complete validation suite with consensus ratings.

## 3. Axis Definitions

### 3.1 Expressive Power: 🙈→👓→🔍→🔬

Measures how well a system allows expression of valid computations while constraining invalid ones.

**Real Impact**: GitHub Copilot and similar tools generate more successful completions with APIs supporting multiple natural expressions.

**🙈 Noise**: Cannot express needed computations. Constraints block valid expressions.
- Example: Stringly-typed API rejecting valid but differently-formatted inputs

**👓 Readable**: Single rigid path. One way to express each operation.
- Example: `add_floats(2.0, 2.0)` - functional but inflexible

**🔍 Structured**: Multiple natural ways to express ideas with meaningful constraints.
- Example: Supporting both `users.filter(active)` and `filter(users, active)`

**🔬 Crystalline**: Rich expressiveness with precise semantic guarantees. Multiple aliases for same operation.
- Example: SQL DSL accepting `WHERE x > 5`, `FILTER(x > 5)`, and `x.gt(5)` - all compile to same AST
- "Many ways" = 6+ different valid syntaxes with identical semantics

### 3.2 Context Flow: 🌀→🧶→🪢→🎀

Measures dependency structure and traversal constraints.

**Real Impact**: The Heartbleed vulnerability remained hidden in OpenSSL's complex dependency graph (🧶) for over 2 years, affecting millions of systems.

**🌀 Entangled**: Circular dependencies with feedback loops. Order changes results.
- Example: Spreadsheet with circular references

**🧶 Coupled**: Complex dependencies without cycles. Hidden state mutations.
- Example: React components with shared context and effects
- Key distinction: Multiple interacting paths with shared mutable state
- Decision guide: Can you trace a single path? → 🪢. Multiple paths affecting each other? → 🧶

**🪢 Pipeline**: Linear dependencies, immutable during traversal.
- Example: `data |> validate |> transform |> save`

**🎀 Independent**: No dependencies between components. Any access order works.
- Example: `(name, age, email)` - change any without affecting others

### 3.3 Error Surface: 🌊→💧→🧊→💠

Measures when errors can occur in the system lifecycle.

**Real Impact**: The Therac-25 radiation overdoses that killed 6 patients resulted from race conditions (🌊) that compile-time safety (💠) would have prevented.

**🌊 Ocean**: Errors cascade unpredictably. One failure triggers system-wide effects.
- Example: `window.APP.state.user = null // Crashes everywhere`

**💧 Liquid**: Errors handled at runtime. Explicit error handling required.
- Example: `Result<User, Error> = fetchUser(id)`

**🧊 Ice**: Errors caught at startup/initialization. Fail fast at boundaries.
- Example: Dependency injection validates all requirements at boot

**💠 Crystal**: Errors impossible at compile/parse time. Invalid states cannot be constructed.
- Example: `divide :: Int -> NonZeroInt -> Int` - division by zero impossible
- Rule of thumb: 💠 when invalid states cannot be expressed

**Error Progression**: 
- 💧: `if (denominator != 0) result = numerator / denominator`
- 🧊: `assert(denominator != 0); result = numerator / denominator`
- 💠: `divide(numerator: Int, denominator: NonZeroInt)`

## 4. Practical Application

### 4.1 Assessment Guide

**Expressive Power**: Count syntactically different but semantically identical ways to accomplish a task.
- 0 ways → 🙈 
- 1 way → 👓
- 2-5 ways → 🔍
- 6+ ways with precise constraints → 🔬

**Context Flow**: Trace dependencies between components.
- Circular dependencies → 🌀
- Complex branches with shared state → 🧶
- Single linear path → 🪢
- Independent components → 🎀

**Error Surface**: Identify when failures can occur.
- Cascading runtime failures → 🌊
- Handled runtime errors → 💧
- Startup/initialization failures → 🧊
- Compile-time prevention → 💠 (invalid states cannot be expressed)

### 4.2 Common Transformations

**Transformation Order**: Stabilize Errors First → Untangle Dependencies → Increase Expressiveness (prevents building flexibility on unstable foundations)

**Callback Hell → Promise Pipeline** (`<👓🌀💧>` → `<🔍🪢🧊>`)
```javascript
// Before: Nested callbacks with circular deps
getUserData(id, (err, user) => {
  if (err) handleError(err);
  else getUserPosts(user.id, (err, posts) => {
    // More nesting...
  });
});

// After: Linear promise chain
getUserData(id)
  .then(user => getUserPosts(user.id))
  .then(posts => render(posts))
  .catch(handleError);
```

**Global State → Module Pattern** (`<👓🌀🌊>` → `<🔍🎀🧊>`)
```javascript
// Before: Global mutations everywhere
window.APP_STATE = { user: null };
function login(user) { window.APP_STATE.user = user; }

// After: Isolated module with clear boundaries
const UserModule = (() => {
  let state = { user: null };
  return {
    login: (user) => { state.user = user; },
    getUser: () => ({ ...state.user })  // Defensive copy
  };
})();
```

### 4.2.1 Boundary Examples

**👓→🔍 (Rigid to Structured)**
```python
# Before (👓): Single rigid syntax
def process_data(data: List[int]) -> int:
    return sum(data)

# After (🔍): Multiple valid approaches  
def process_data(data: Sequence[int]) -> int:
    return sum(data)  # Now accepts list, tuple, or any sequence
```

**💧→🧊 (Runtime to Initialization)**
```typescript
// Before (💧): Runtime config errors
function getConfig(key: string): string {
  const value = process.env[key];
  if (!value) throw new Error(`Missing ${key}`);
  return value;
}

// After (🧊): Initialization-time validation
const config = {
  apiUrl: process.env.API_URL!,
  apiKey: process.env.API_KEY!,
} as const;
// Errors surface at startup, not during request handling
```

### 4.3 Context-Dependent Priorities

Not all axes deserve equal weight in every domain:

**Interactive Tools (REPLs, CLIs)**: Prioritize Expressive Power (🔍→🔬)
- Target: `<🔬🪢💧>` - Maximum flexibility for experimentation

**Infrastructure & Configuration**: Prioritize Error Surface (🧊→💠)
- Target: `<🔍🎀💠>` - Predictability over flexibility

**Data Pipelines**: Prioritize Context Flow (🪢→🎀)
- Target: `<🔍🪢🧊>` - Clear data flow for debugging

**Safety-Critical Systems**: Error Surface is non-negotiable
- Target: `<👓🎀💠>` or `<🔍🎀💠>` depending on domain constraints

**Priority Decision Rules**:
1. Human lives at stake → Error Surface (💠) first
2. Iteration speed critical → Expressive Power (🔬) first  
3. Debugging time dominates → Context Flow (🎀) first
4. When in doubt → Balance all three at 🔍🪢🧊

### 4.4 Anti-Pattern Quick Fixes

**Everything Object** (`<🙈🌀🌊>`): Extract modules → Define interfaces → Add type guards

**Magic String Soup** (`<🙈🧶🌊>`): Use enums → Add types → Parse once

**Global State Mutation** (`<👓🌀🌊>`): Isolate state → Use immutability → Add boundaries

### 4.5 Good to Great: Excellence Patterns

VIBES isn't just for fixing problems—it guides the journey from functional to exceptional:

**API Evolution** (`<🔍🪢💧>` → `<🔬🪢💠>`)
```typescript
// Good: Basic typed API (functional but limited)
function query(table: string, filter: object): Promise<any[]>

// Great: Type-safe DSL with compile-time validation
const users = await db
  .from(tables.users)
  .where(u => u.age.gt(18))
  .select(u => ({ name: u.name, email: u.email }));
// SQL injection impossible, return type inferred, discoverable API
```

**The Excellence Mindset**: Good code works. Great code makes errors impossible while supporting multiple natural expressions.

### 4.6 Complete Case Study: Configuration System Evolution

**Starting Point** `<🙈🌀🌊>`:
```xml
<config>
  <property name="timeout">${ENV_TIMEOUT:-${FALLBACK_TIMEOUT:-30}}</property>
  <import file="${CONFIG_DIR}/db.xml" if="${USE_DB}"/>
  <!-- Circular imports, runtime explosions, string interpolation hell -->
</config>
```

**Step 1: Stabilize Errors** (🌊→💧) → `<🙈🌀💧>`:
```javascript
try {
  const timeout = process.env.ENV_TIMEOUT || process.env.FALLBACK_TIMEOUT || "30";
  config.timeout = parseInt(timeout);
} catch (e) {
  console.error("Config error:", e);
}
```

**Step 2: Untangle Dependencies** (🌀→🪢) → `<🙈🪢💧>`:
```javascript
const loadConfig = () => {
  const base = loadBaseConfig();
  const db = shouldUseDb() ? loadDbConfig() : {};
  return { ...base, ...db };  // Linear merge, no cycles
};
```

**Step 3: Increase Expressiveness** (🙈→🔍) → `<🔍🪢💧>`:
```typescript
// Now supports JSON, YAML, or env vars
const config = await Config
  .from.env()
  .from.file('config.yaml')
  .from.json({ timeout: 30 })
  .load();
```

**Final State** `<🔍🪢💠>`:
```typescript
const ConfigSchema = z.object({
  timeout: z.number().min(1).max(300).default(30),
  database: z.optional(DatabaseSchema)
});

const config = ConfigSchema.parse(await loadConfig());
// @VIBES: <🔍🪢💠> - Parse-time validation, linear pipeline, flexible sources
```

## 5. LLM-Specific Considerations

### 5.1 Semantic Variation: Aliases, Ambiguity, and Emoji

**Semantic Aliases vs Ambiguity**

**Good - Semantic Aliases** (improves 🔬): Multiple syntaxes, identical semantics
- `filter/where`, `map/select`, `&&/AND`
- Test: Do all forms compile to identical behavior?

**Bad - Semantic Ambiguity** (degrades toward 🙈): Similar syntax, different behaviors
- JavaScript's `==` vs `===`
- `+` for both addition and concatenation

**Emoji Token Guidance**

Emojis can serve as semantic domain markers in LLM-optimized languages, creating visual type systems that transformers can pattern-match across contexts.

**The Core Insight**: Instead of parsing strings like "NotFoundException" or "UnauthorizedError", LLMs can recognize that anything prefixed with 🚫 belongs to the error domain. This creates immediate, cross-linguistic pattern recognition.

**Example: VIBES-Lang with Semantic Domains**

```vibes
# VIBES-Lang: Emojis define semantic domains, not just individual tokens

# Define semantic domains with emoji prefixes
domains {
  🚫 = Error/Failure states
  ✅ = Success/Completion states  
  🔒 = Security/Permission states
  📊 = Observability/Metrics
  ⚡ = Performance/Optimization
  🔄 = Async/Concurrent operations
}

service UserAPI {
  🔍 operation findUser(id: UserId) → User | 🚫 {
    # Any 🚫-prefixed value is an error
    errors: [🚫 NotFound, 🚫 DatabaseTimeout]
    requires: [🔒 Authenticated]
    effects: [📊 metrics.query_count++]
  }
  
  🗑️ operation deleteUser(id: UserId) → ✅ | 🚫 {
    requires: [🔒 Admin, 🔒 AuditLog]
    effects: [
      📊 audit.write("User ${id} deleted"),
      🔄 async.notify(user.contacts)
    ]
  }
}

# Pattern matching on domains
match operation.result {
  ✅ → continue()           # Any success state
  🚫 NotFound → create()    # Specific error
  🚫 → retry()              # Any other error
  🔒 → escalate()           # Any security issue
}

# Performance hints using domain markers
function processUsers(users: List<User>) → ✅ | 🚫 {
  ⚡ parallel: true        # Performance domain hint
  ⚡ cache: 5.minutes      # Another perf directive
  
  return users
    |> 🔄 map(validate)    # Async domain operation
    |> 🔄 filter(active)   
    |> ✅                  # Success domain result
}
```

**Why This Works for LLMs**:
- **Visual Namespacing**: `🚫 Unauthorized` immediately signals "error domain"
- **Cross-Context Patterns**: LLMs recognize 🔒 means "security-related" everywhere
- **Reduced Ambiguity**: No parsing whether "Invalid" is an error or a status
- **Compositional**: Can combine domains: `🔒🚫 SecurityError`

**Schema Documentation**: Always document domain meanings:
```yaml
@semantic_domains: {
  🚫: 'Error/Failure states',
  ✅: 'Success/Completion states',
  🔒: 'Security/Permission states',
  📊: 'Observability/Metrics',
  ⚡: 'Performance/Optimization',
  🔄: 'Async/Concurrent operations'
}
```

**Avoid When**:
- **Semantic meaning unclear or varies culturally**: 🙏 (prayer/thanks/please?)
- **No clear visual metaphor exists**
- **Human developers are the primary users**

**Schema Documentation**: Always provide emoji schemas in documentation:
```yaml
@operators: {➕: 'addition', ➖: 'subtraction', ✖️: 'multiplication', ➗: 'division'}
@actions: {🚀: 'deploy', 📝: 'update', 🗑️: 'delete', 🔍: 'search'}
```

### 5.2 Context Window Considerations

A system rated `🎀` (Independent) within one window may become `🧶` (Coupled) when split across multiple interactions:

```python
# Window 1: Module with "independent" functions
def calculate_total(items):
    subtotal = sum(item.price for item in items)
    return apply_discount(subtotal)

def apply_discount(amount):
    # Function appears independent...
    return amount * (1 - DISCOUNT_RATE)

# Window 2: Constants defined elsewhere
DISCOUNT_RATE = 0.1  # LLM can't see this is used above

# Window 3: Another "independent" function
def update_discount(new_rate):
    global DISCOUNT_RATE  # Hidden coupling!
    DISCOUNT_RATE = new_rate
```

What seems like three independent functions (`🎀`) is actually a coupled system (`🧶`) through hidden global state. Design with chunking in mind: make dependencies explicit in function signatures.

## 6. Summary

VIBES provides vocabulary for patterns LLMs already recognize from their training data. The framework is descriptive of existing engineering excellence, not prescriptive of new behaviors.

**Key Insights**:
- Better VIBES ratings correlate with higher task completion rates and fewer retries
- Different domains require different axis priorities (safety → 💠, speed → 🔬, debugging → 🎀)
- LLM ergonomics diverge fundamentally from human ergonomics, requiring separate tools
- Cross-model validation ensures broad applicability across transformer architectures

**The Three Axes in Practice**:
- **Expressive Power**: From rigid single-syntax APIs to rich semantic aliases
- **Context Flow**: From circular spaghetti to clean, traceable pipelines  
- **Error Surface**: From runtime explosions to compile-time impossibility

**Team Usage**: VIBES provides shared vocabulary for improvement, not ammunition for criticism. Focus on *why* patterns have certain ratings, not just the scores. The spirit matters more than the letter: are we genuinely improving LLM ergonomics or just checking boxes?

**Remember**: VIBES guides both remediation (fixing `<🙈🌀🌊>`) and excellence (achieving `<🔬🎀💠>`).

**Multi-Model Systems**: When rating systems that combine multiple LLMs, assess the weakest link—a pipeline is only as ergonomic as its least ergonomic component.

**Version Migration**: VIBES ratings may shift with framework/language updates. Document ratings with version context (e.g., "React 16: 🧶, React 18 with Suspense: 🪢").

For complete pattern corpus, detailed examples, and philosophical foundations, see:
- `calibration/CALIBRATION_CORPUS.md` - Validation patterns with consensus ratings
- `calibration/run_0_analysis.md` - Cross-model validation results and analysis
