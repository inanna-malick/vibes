# Pattern: Module Pattern

## VIBES Rating
`<🔍🎀🧊>`

## Description
Isolated modules with clear interfaces, dependency injection, and startup validation. Each module is self-contained with explicit contracts.

## Example
```javascript
// Well-structured module with clear boundaries
const createUserModule = ({ database, logger, config }) => {
  // Validate dependencies at creation time
  if (!database) throw new Error('UserModule requires database');
  if (!config.userTable) throw new Error('UserModule requires userTable config');
  
  // Private state, not accessible from outside
  const cache = new Map();
  
  // Clear public interface
  return {
    async findById(id) {
      if (cache.has(id)) {
        logger.debug(`Cache hit for user ${id}`);
        return cache.get(id);
      }
      
      const user = await database.query(
        `SELECT * FROM ${config.userTable} WHERE id = ?`,
        [id]
      );
      
      if (user) cache.set(id, user);
      return user;
    },
    
    async create(userData) {
      const validated = validateUserData(userData);
      const result = await database.insert(config.userTable, validated);
      logger.info(`Created user ${result.id}`);
      return result;
    },
    
    clearCache() {
      cache.clear();
    }
  };
};

// Usage - dependencies explicit at creation
const userModule = createUserModule({
  database: dbConnection,
  logger: appLogger,
  config: { userTable: 'users' }
});
```

## Why This Rating

### Expressive Power: 🔍
- Clear conceptual model: modules encapsulate related functionality
- Interface defines exactly what's possible
- Private implementation details hidden
- Dependency injection pattern guides structure

### Context Flow: 🎀
- Modules are completely independent
- Dependencies injected, not imported
- No shared mutable state
- Can be tested in complete isolation

### Error Surface: 🧊
- Dependency validation at module creation
- Missing requirements fail at startup
- Configuration errors caught before use
- Clear contract enforcement

## Common Occurrence
- Modern JavaScript applications
- Microservice architectures
- Well-designed APIs
- Functional core, imperative shell pattern

## Impact
- LLMs can reason about modules independently
- Clear interfaces guide correct usage
- Testing is straightforward
- Refactoring is localized

## Related Patterns
- [global-state-mutation.md](./global-state-mutation.md) - What this pattern prevents
- [dependency-injection.md](./dependency-injection.md) - Enabling pattern
- [parser-combinators.md](./parser-combinators.md) - Similar isolation principles

## Consensus Data
- Model Agreement: 91%
- Disputed Aspects: Some models rate 👓 when interfaces are too rigid
- Test Date: July 2025
