# Error Surface 🌊→💧→🧊→💠

Measures when errors can occur in the system lifecycle.

## The Scale

### 🌊 Ocean (Errors Cascade Unpredictably)
One failure triggers system-wide effects. Errors spread like waves.

**Examples**:
- Shared mutable global state
- Null pointer exceptions cascading through object graphs
- Race conditions in concurrent systems

**Historical**: NASA Mars Climate Orbiter - unit confusion cascaded through systems.

**Why it's 🌊**: One error creates tsunamis throughout your system.

### 💧 Liquid (Runtime Errors, Handled)
Errors occur during execution but are contained.

**Examples**:
- `Result<T, Error>` types in Rust
- Try-catch blocks with proper error handling
- Validation at function boundaries

**Why it's 💧**: Errors flow but through defined channels.

### 🧊 Ice (Startup/Initialization Errors)
Errors caught when system boots or components initialize.

**Examples**:
- Dependency injection validation
- Configuration schema checking at startup
- Database connection verification

**Why it's 🧊**: Errors freeze the system before it can do damage.

### 💠 Crystal (Compile/Parse-Time Prevention)
Invalid states cannot be constructed. Types make errors impossible.

**Examples**:
- `divide :: Int -> NonZeroInt -> Int` (division by zero impossible)
- State machines encoded in types
- Parse-don't-validate patterns

**Historical**: Therac-25 radiation overdoses - type safety could have prevented patient deaths.

**Why it's 💠**: Errors literally cannot exist in the crystal structure.

## Assessment Questions

1. **Can one error crash unrelated parts?** (Yes → 🌊)
2. **Do errors occur during normal operation?** (Yes → 💧)
3. **Are errors caught at startup only?** (Yes → 🧊)
4. **Are errors impossible by construction?** (Yes → 💠)

## The Progression

```
🌊 try { doEverything() } catch { ??? }
   ↓
💧 Result.map(validate).flatMap(process).getOrElse(default)
   ↓
🧊 @Component class Service(required: Dependencies) // Fails at boot
   ↓
💠 newtype Email = Email String -- Can only construct valid emails
```

## Domain Priorities

- **Human lives at stake**: 💠 is non-negotiable
- **Financial systems**: At least 🧊, prefer 💠
- **Experimentation/prototypes**: 💧 might suffice
- **Never acceptable**: 🌊 in production

See also: [Transformation patterns](../transformations/error-progression.md) for moving up the scale.
