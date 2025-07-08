# Pattern: Global State Mutation

## VIBES Rating
`<🙈🌀🌊>`

## Description
The canonical toxic pattern - mutable global state that any code can modify, creating unpredictable cascading failures and circular dependencies.

## Example
```javascript
// The worst of all worlds
window.APP = {
  user: null,
  config: {},
  cache: {},
  temp: {}
};

function loadUser(id) {
  fetch(`/api/users/${id}`)
    .then(data => {
      window.APP.user = data;
      window.APP.cache[id] = data;
      updateEverything(); // Triggers cascade
    });
}

function updateEverything() {
  // Circular dependency
  if (window.APP.user && window.APP.config.theme) {
    updateTheme();
    updatePermissions();
    updateCache(); // Which updates user... which calls this...
  }
}

function randomBusinessLogic() {
  // Any function can break everything
  window.APP.user = null; // Boom! Cascading failures everywhere
}
```

## Why This Rating

### Expressive Power: 🙈
- No conceptual model whatsoever
- Any code can do anything to any state
- No guidance toward correct usage
- Mutation from anywhere is encouraged

### Context Flow: 🌀
- Circular dependencies everywhere
- Functions call each other in loops
- State changes trigger more state changes
- Impossible to trace causality

### Error Surface: 🌊
- One null value cascades through entire system
- Errors surface in distant, unrelated code
- No boundaries contain failures
- Debugging requires system-wide archaeology

## Common Occurrence
- Legacy JavaScript applications
- Quick prototypes that became production
- Systems that grew without architecture
- "We'll refactor it later" codebases

## Impact
- LLMs generate random mutations
- Each retry makes things worse
- No consistent mental model possible
- Testing is effectively impossible

## Related Patterns
- [kubernetes-yaml.md](./kubernetes-yaml.md) - Similar lack of structure
- [callback-hell.md](./callback-hell.md) - Often found together
- [module-pattern.md](./module-pattern.md) - The cure

## Consensus Data
- Model Agreement: 98%
- Disputed Aspects: None - universally recognized as toxic
- Test Date: July 2025
