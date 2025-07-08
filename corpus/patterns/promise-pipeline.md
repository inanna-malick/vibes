# Pattern: Promise Pipeline

## VIBES Rating
`<🔍🪢🧊>`

## Description
Promise pipelines transform nested callback chaos into linear, readable flows. Each step clearly depends on the previous, errors bubble predictably, and the conceptual model guides correct usage.

## Example
```javascript
// Promise pipeline with clear linear flow
fetchUser(userId)
  .then(user => enrichUserData(user))
  .then(enrichedUser => fetchUserPosts(enrichedUser.id))
  .then(posts => renderProfile(enrichedUser, posts))
  .catch(handleError);

// Modern async/await variant
async function loadUserProfile(userId) {
  try {
    const user = await fetchUser(userId);
    const enrichedUser = await enrichUserData(user);
    const posts = await fetchUserPosts(enrichedUser.id);
    return renderProfile(enrichedUser, posts);
  } catch (error) {
    return handleError(error);
  }
}
```

## Why This Rating

### Expressive Power: 🔍
- Clear conceptual model: each step transforms the previous
- Multiple valid expressions (then chains, async/await)
- Difficult to write incorrectly once pattern understood
- Type inference flows through the pipeline

### Context Flow: 🪢
- Strictly linear dependency chain
- Each step has access only to its input
- No hidden state or circular dependencies
- Data flows in one direction

### Error Surface: 🧊
- Errors caught at promise construction
- Single error handler for entire pipeline
- Failed promises prevent downstream execution
- Stack traces maintain async context

## Common Occurrence
- Modern JavaScript/TypeScript
- API call orchestration
- Data transformation pipelines
- Functional reactive programming

## Impact
- LLMs excel at extending promise chains
- Clear data flow enables accurate predictions
- Error handling patterns are consistent
- Refactoring is straightforward

## Related Patterns
- [callback-hell.md](./callback-hell.md) - What this pattern replaces
- [parser-combinators.md](./parser-combinators.md) - Similar compositional approach

## Consensus Data
- Model Agreement: 92%
- Disputed Aspects: Some models rate 💧 for errors due to runtime promise rejection
- Test Date: July 2025
