# Dependency Injection Pattern

## Pattern Name
Dependency Injection Container

## VIBES Rating
<🔍🎀🧊>

## Description
Dependency injection pattern that validates all dependencies at startup, making runtime dependency errors impossible. Shows the clear boundary between 💧 (runtime) and 🧊 (initialization) error handling.

## Code Example
```typescript
// Dependency injection with startup validation
class Container {
  private services = new Map<string, any>();
  private factories = new Map<string, () => any>();
  private initialized = false;
  
  register<T>(name: string, factory: () => T): void {
    if (this.initialized) {
      throw new Error("Cannot register after initialization");
    }
    this.factories.set(name, factory);
  }
  
  // All dependency errors surface here at startup
  async initialize(): Promise<void> {
    const initOrder = this.topologicalSort();
    
    for (const name of initOrder) {
      try {
        const factory = this.factories.get(name)!;
        const instance = await factory();
        this.services.set(name, instance);
      } catch (error) {
        throw new Error(`Failed to initialize ${name}: ${error.message}`);
      }
    }
    
    this.initialized = true;
  }
  
  get<T>(name: string): T {
    if (!this.initialized) {
      throw new Error("Container not initialized");
    }
    
    const service = this.services.get(name);
    if (!service) {
      throw new Error(`Service ${name} not found`);
    }
    
    return service;
  }
  
  private topologicalSort(): string[] {
    // Detect circular dependencies at startup
    // Implementation details...
    return [];
  }
}

// Usage - all errors happen at startup
const container = new Container();

container.register('db', () => new Database(config.dbUrl));
container.register('cache', () => new RedisCache(config.redisUrl));
container.register('userService', () => 
  new UserService(container.get('db'), container.get('cache'))
);

// This is where ALL dependency errors surface
await container.initialize().catch(error => {
  console.error("Startup failed:", error);
  process.exit(1);
});

// Runtime usage is guaranteed safe
const userService = container.get<UserService>('userService');
// No null checks needed - initialization proved it exists
```

## Why This Rating?

### Expressive Power: 🔍 (Structured)
- Multiple ways to register dependencies
- Supports factories, singletons, scopes
- Flexible registration patterns

### Context Flow: 🎀 (Independent)
- Services can be registered in any order
- Topological sort handles ordering
- No hidden dependencies between registrations

### Error Surface: 🧊 (Ice)
- ALL dependency errors surface at startup
- Missing dependencies caught during initialize()
- Circular dependencies detected early
- Runtime gets are guaranteed safe

## Boundary Demonstration: 💧 vs 🧊

### Liquid Version (💧 - Runtime Errors)
```typescript
// Errors discovered during execution
class RuntimeContainer {
  private services = new Map<string, any>();
  
  get<T>(name: string): T {
    if (!this.services.has(name)) {
      // Try to create it now - might fail!
      try {
        const instance = this.createService(name);
        this.services.set(name, instance);
      } catch (error) {
        // Runtime explosion
        throw new Error(`Failed to create ${name}: ${error}`);
      }
    }
    return this.services.get(name);
  }
  
  private createService(name: string): any {
    // Dependencies might not exist yet
    // Circular deps cause stack overflow
    // Errors happen during user requests
  }
}
```

### Ice Version (🧊 - This Pattern)
- Dependencies validated at startup
- Circular dependencies detected before running
- Missing services caught during initialization
- Runtime is guaranteed safe

### Crystal Version (💠 - Compile-Time)
```typescript
// Using TypeScript's type system for compile-time DI
type ServiceMap = {
  db: Database;
  cache: Cache;
  userService: UserService;
};

class TypedContainer<T extends ServiceMap> {
  constructor(private services: T) {}
  
  get<K extends keyof T>(name: K): T[K] {
    return this.services[name];
  }
}

// Compile error if dependencies don't match
const container = new TypedContainer({
  db: new Database(config.dbUrl),
  cache: new RedisCache(config.redisUrl),
  userService: new UserService(db, cache)
  // TypeScript ensures all required services exist
});
```

## Context Considerations
- For small apps: Manual wiring might be simpler
- For large apps: DI containers reduce boilerplate
- With TypeScript: Can approach 💠 with compile-time checks
- In dynamic languages: 🧊 is the best achievable

## Common Mistakes

### Late Binding (Drops to 💧)
```typescript
// DON'T: Allow runtime registration
container.register('lateService', () => new Service());
// Now errors can happen during execution
```

### Hidden Dependencies (Drops to 🧶)
```typescript
// DON'T: Access container inside factories
container.register('bad', () => {
  const dep = container.get('other'); // Hidden coupling!
  return new BadService(dep);
});
```

### Over-Engineering
```typescript
// DON'T: Add unnecessary complexity
class QuantumEntangledMetaContainer<T> extends AbstractContainerFactory {}
```

## Related Patterns
- [[service-locator]] - Anti-pattern version
- [[module-pattern]] - Alternative organization
- [[factory-pattern]] - Simpler creation pattern

## References
- Martin Fowler's "Inversion of Control Containers"
- Spring Framework documentation
- Angular's dependency injection system
