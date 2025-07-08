# VIBES Transformation Order

## The Fundamental Principle

**Always transform in this order: Stabilize Errors → Untangle Dependencies → Build Conceptual Model**

This ordering reflects a deep truth about system improvement: you cannot reason about structure while things are exploding, and you cannot build elegant abstractions on top of tangled dependencies.

## Why This Order Works

### 1. Stabilize Errors First (🌊→💧→🧊→💠)

**You cannot improve what you cannot observe.**

When errors cascade unpredictably:
- Debugging is archaeology
- Changes have unknowable effects
- Progress requires prayer

Start by containing the blast radius:
- Add error boundaries (🌊→💧)
- Push validation earlier (💧→🧊)
- Make invalid states unrepresentable (🧊→💠)

### 2. Untangle Dependencies Second (🌀→🧶→🪢→🎀)

**You cannot abstract what you cannot trace.**

With errors contained, now you can see the dependency graph:
- Break circular references (🌀→🧶)
- Linearize complex flows (🧶→🪢)
- Isolate independent components (🪢→🎀)

### 3. Build Conceptual Model Last (🙈→👓→🔍→🔬)

**Only stable, comprehensible systems deserve beautiful abstractions.**

With errors contained and dependencies clear:
- Define the domain model (🙈→👓)
- Create guiding frameworks (👓→🔍)
- Achieve crystalline clarity (🔍→🔬)

## Common Anti-Pattern: Premature Abstraction

The temptation is to start with beautiful concepts:
"Let's create a nice DSL for this mess!"

This fails because:
- Hidden errors corrupt your abstractions
- Tangled dependencies break your model
- You're polishing chaos

## Practical Example

Starting with callback hell:
```javascript
// Initial state: <👓🌀💧>
getData(id, (err, data) => {
  if (err) handleError(err);
  else processData(data, (err2, result) => {
    if (err2) handleError(err2);
    else saveResult(result, (err3) => {
      if (err3) handleError(err3);
      else updateUI();
    });
  });
});
```

**Step 1: Stabilize Errors** (💧→🧊)
```javascript
// Promises handle errors consistently
getData(id)
  .then(processData)
  .then(saveResult)
  .then(updateUI)
  .catch(handleError); // Single error boundary
```

**Step 2: Untangle Dependencies** (🌀→🪢)
```javascript
// Linear pipeline, clear flow
const pipeline = async (id) => {
  const data = await getData(id);
  const result = await processData(data);
  await saveResult(result);
  return updateUI();
};
```

**Step 3: Build Conceptual Model** (👓→🔍)
```javascript
// Domain-driven clarity
class DataProcessor {
  async process(userId: UserId): Promise<ProcessedResult> {
    const rawData = await this.fetch(userId);
    const validated = this.validate(rawData);
    const transformed = this.transform(validated);
    await this.persist(transformed);
    return transformed;
  }
}
```

## Exceptions and Edge Cases

### When to Deviate

**Safety-Critical Systems**: May need to prioritize 💠 immediately
- Medical devices
- Aviation software  
- Financial transactions

**Greenfield Projects**: Can design all three axes together
- Start with the end state in mind
- Build in the correct order from scratch

### Partial Transformations

Sometimes you can only improve one axis:
- Legacy system with frozen architecture
- Time/budget constraints
- Team capability limits

Even partial improvement helps:
- 🌊→💧 makes debugging possible
- 🌀→🧶 makes reasoning feasible
- 🙈→👓 makes onboarding easier

## The Meta-Principle

This transformation order itself demonstrates good VIBES:
- **Clear conceptual model** (🔍): Three ordered phases
- **Linear dependencies** (🪢): Each phase enables the next
- **Early validation** (🧊): Can't skip steps without obvious failure

## Navigation

- [Back to Guides](./README.md)
- [Stabilize Errors Guide](./stabilize-errors.md)
- [Untangle Dependencies Guide](./untangle-dependencies.md)
- [Build Conceptual Model Guide](./build-conceptual-model.md)
