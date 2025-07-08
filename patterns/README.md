# VIBES Patterns

A catalog of common code patterns organized by their VIBES ratings. Each pattern includes rationale, examples, and transformation paths.

## Navigation by Rating

### Anti-Patterns (Avoid These)
- **[Noise Patterns](./anti-patterns/)** `<🙈*️⃣*️⃣>` - No conceptual framework
- **[Chaos Patterns](./anti-patterns/)** `<*️⃣🌀*️⃣>` - Circular dependencies  
- **[Ocean Patterns](./anti-patterns/)** `<*️⃣*️⃣🌊>` - Cascading failures

### Common Patterns
- **[Rigid Patterns](./rigid/)** `<👓*️⃣*️⃣>` - Clear purpose, limited flexibility
- **[Pipeline Patterns](./pipeline/)** `<*️⃣🪢*️⃣>` - Linear data flow
- **[Contained Patterns](./contained/)** `<*️⃣*️⃣💧>` - Runtime error handling

### Excellence Patterns  
- **[Coherent Patterns](./coherent/)** `<🔍*️⃣*️⃣>` - Strong conceptual models
- **[Independent Patterns](./independent/)** `<*️⃣🎀*️⃣>` - No dependencies
- **[Crystal Patterns](./crystal/)** `<*️⃣*️⃣💠>` - Compile-time safety

### Ideal Patterns
- **[Crystalline Excellence](./crystalline/)** `<🔬🎀💠>` - The pinnacle

## Quick Index

### By Problem Domain

**API Design**
- [RESTful Resources](./coherent/restful-resources.md) `<🔍🪢🧊>`
- [GraphQL Schema](./coherent/graphql-schema.md) `<🔍🧶💧>`
- [gRPC Services](./crystal/grpc-services.md) `<🔍🪢💠>`

**State Management**
- [Global State](./anti-patterns/global-state.md) `<🙈🌀🌊>` ❌
- [Redux Store](./pipeline/redux-store.md) `<🔍🪢💧>`
- [Event Sourcing](./crystalline/event-sourcing.md) `<🔬🎀💠>`

**Configuration**
- [Environment Variables](./anti-patterns/env-vars.md) `<🙈🧶🌊>` ❌
- [Config Files](./rigid/config-files.md) `<👓🪢💧>`
- [Type-Safe Config](./crystal/typed-config.md) `<🔍🪢💠>`

**Error Handling**
- [Try-Catch Everything](./anti-patterns/catch-all.md) `<🙈🌀🌊>` ❌
- [Result Types](./contained/result-types.md) `<🔍🪢💧>`
- [Parse Don't Validate](./crystal/parse-dont-validate.md) `<🔬🎀💠>`

## How to Use This Catalog

1. **Find Your Current Pattern**: Look for code that resembles yours
2. **Check the Rating**: Understand where you are on each axis
3. **Find Target Pattern**: Look for better-rated alternatives
4. **Follow Transformation**: See [transformations](../transformations/) for paths

## Pattern Template

Each pattern follows this structure:

```markdown
# Pattern Name

**Rating**: `<🔍🪢💠>`
**Domain**: API Design, State Management, etc.
**Also Known As**: Alternative names

## The Pattern

Brief description of what this pattern does.

## Code Example

​```language
// Minimal example showing the pattern
​```

## VIBES Analysis

**Expressive (🔍)**: Why this rating?
**Context (🪢)**: Why this rating?
**Error (💠)**: Why this rating?

## When to Use

- Specific use case 1
- Specific use case 2

## Trade-offs

- Pro: Benefit 1
- Pro: Benefit 2  
- Con: Drawback 1

## Transformations

- From: [Worse Pattern](../path/to/worse.md)
- To: [Better Pattern](../path/to/better.md)
```

## Contributing Patterns

See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines. New patterns require:
- Cross-model validation
- Clear code example
- Transformation paths
- Trade-off analysis
