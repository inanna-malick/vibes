# Global State Mutation

**Rating**: `<🙈🌀🌊>`
**Domain**: State Management
**Also Known As**: The Singleton Trap, Ambient Authority, Action at a Distance

## The Pattern

Using global mutable variables that can be accessed and modified from anywhere in the codebase. Often disguised as "convenient" singletons or module-level variables.

## Code Example

```javascript
// The tempting trap
let currentUser = null;
let shoppingCart = [];
let discountRate = 0.1;

function login(username, password) {
  // Mutates global state
  currentUser = validateUser(username, password);
  if (currentUser.isPremium) {
    discountRate = 0.2; // Side effect!
  }
}

function addToCart(item) {
  if (!currentUser) throw new Error("Not logged in");
  shoppingCart.push(item);
  // Hidden dependency on global discountRate
  item.finalPrice = item.price * (1 - discountRate);
}

function checkout() {
  // Depends on multiple global states
  const order = {
    user: currentUser,
    items: shoppingCart,
    total: shoppingCart.reduce((sum, item) => sum + item.finalPrice, 0)
  };
  
  // More mutations!
  shoppingCart = [];
  return order;
}
```

## VIBES Analysis

**Expressive (🙈)**: No conceptual model guides usage. Any function can mutate any global state at any time. The system provides no orientation toward correct usage.

**Context (🌀)**: Circular dependencies abound. Functions depend on global state which they also modify, creating feedback loops. Order of function calls drastically changes behavior.

**Error (🌊)**: Errors cascade unpredictably. A mutation in one function breaks unrelated features. Race conditions in async code. State corruption spreads like wildfire.

## Why This Pattern Emerges

- **Quick prototyping**: "I'll refactor it later" (but later never comes)
- **Misunderstood convenience**: Seems easier than passing parameters
- **Framework defaults**: Some frameworks encourage ambient state
- **Legacy evolution**: Started small, grew organically

## The Hidden Costs

1. **Unpredictable behavior**: Function behavior depends on invisible global state
2. **Impossible testing**: Must reset entire global state between tests
3. **Concurrency nightmares**: Thread safety becomes impossible
4. **Debugging hell**: Can't trace where state changes originate
5. **LLM confusion**: Models can't track state mutations across context windows

## Real-World Horror Story

One e-commerce site had a global `DISCOUNT_RATE` that customer service could modify. During Black Friday, a support agent helping one customer accidentally gave everyone 90% off for 12 minutes. The mutation was 47 function calls away from where the discount was applied. Loss: $1.2M.

## Transformations

**Immediate Mitigation** `<🙈🌀🌊>` → `<🙈🌀💧>`:
```javascript
// At least make mutations explicit
const globalState = {
  currentUser: null,
  shoppingCart: [],
  discountRate: 0.1,
  
  setUser(user) { this.currentUser = user; },
  clearCart() { this.shoppingCart = []; }
};
```

**Better: Dependency Injection** `<🙈🌀💧>` → `<🔍🪢💧>`:
```javascript
class ShoppingService {
  constructor(userService, pricingService) {
    this.userService = userService;
    this.pricingService = pricingService;
  }
  
  addToCart(userId, item) {
    const user = this.userService.getUser(userId);
    const discount = this.pricingService.getDiscount(user);
    // ... explicit dependencies
  }
}
```

**Best: Immutable State** `<🔍🪢💧>` → `<🔬🎀💠>`:
See [Event Sourcing](../../crystalline/event-sourcing.md) or [Redux Pattern](../../pipeline/redux-store.md)

## Related Patterns

- [Hidden Dependencies](./hidden-dependencies.md) - The cousin pattern
- [God Object](./god-object.md) - When globals become objects
- [Spooky Action](./spooky-action.md) - Mutations across module boundaries

## Detection

Look for:
- Top-level `let` or `var` declarations
- Singleton patterns with mutable state
- Functions with no parameters but complex behavior
- Test suites with extensive setup/teardown
- The phrase "just use the global"
