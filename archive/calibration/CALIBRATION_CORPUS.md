# VIBES Calibration Corpus

## Purpose
Cross-model calibration for VIBES (Visual Interface for Better Ergonomic Systems) assessment. This corpus tests whether different LLMs converge on consistent ratings for code ergonomics.

## VIBES Quick Reference
- **Expressive Power**: 🙈(blocked) → 👓(rigid) → 🔍(flexible) → 🔬(crystalline)
- **Context Flow**: 🌀(circular) → 🧶(tangled) → 🪢(pipeline) → 🎀(independent)
- **Error Surface**: 🌊(cascading) → 💧(runtime) → 🧊(startup) → 💠(parse-time)

Notation: `<Expressive/Context/Error>` e.g., `<🔍🪢💠>`

---

## PART 1: Core Concept Exemplars

### Example 1: Global State Chaos
**Domain**: JavaScript global state with type confusion
```javascript
// Global chaos - everything wrong at once
window.APP = {
  timeout: process.env.TIMEOUT || "30",  // String or number?
  data: null
};

function init() {
  window.APP.data = fetchData();  // Async but no await
  if (window.APP.data) {  // Always false, async not complete
    process();
  }
}

function process() {
  const timeout = window.APP.timeout * 1000;  // NaN if string
  window.APP.data.forEach(item => {  // Null pointer
    setTimeout(() => handle(item), timeout);
  });
}
```

---

### Example 2: Type-Safe Functional Design
**Domain**: Haskell with advanced types
```haskell
-- Multiple valid expressions, all safe
newtype Email = Email Text deriving (Eq, Show)
newtype UserId = UserId Int deriving (Eq, Show)

mkEmail :: Text -> Maybe Email
mkEmail t 
  | "@" `T.isInfixOf` t = Just (Email t)
  | otherwise = Nothing

-- Can be used with any monad transformer stack
findUser :: MonadIO m => Email -> m (Maybe UserId)
findUser email = liftIO $ queryDB email

-- Or in pure context
validateEmail :: Text -> Either Text Email
validateEmail t = maybe (Left "Invalid email") Right (mkEmail t)
```

---

### Example 3: State Machine Types
**Domain**: Rust state machine
```rust
// States as types - only one way to transition
struct Locked;
struct Unlocked;

impl Door<Locked> {
    fn unlock(self, key: Key) -> Result<Door<Unlocked>, Self> {
        if key.is_valid() {
            Ok(Door::<Unlocked> { id: self.id })
        } else {
            Err(self)
        }
    }
}

impl Door<Unlocked> {
    fn lock(self) -> Door<Locked> {
        Door::<Locked> { id: self.id }
    }
}
// Cannot call unlock on unlocked door - won't compile
```

---

### Example 4: Framework Magic
**Domain**: Ruby on Rails with callbacks
```ruby
class Order < ActiveRecord::Base
  has_many :items, dependent: :destroy
  belongs_to :user
  
  before_save :calculate_total
  after_save :notify_warehouse
  after_commit :charge_customer
  
  def add_item(product)
    items.create!(product: product, price: product.current_price)
    save!  # Triggers callback cascade
  end
  
  private
  def calculate_total
    self.total = items.sum(:price)
  end
  
  def notify_warehouse
    WarehouseAPI.update_inventory(self)  # External call in callback
  end
end
```

---

### Example 5: Pipeline Architecture
**Domain**: Node.js data transformation
```javascript
// Clear pipeline with runtime handling
const pipeline = (data) =>
  Promise.resolve(data)
    .then(validate)
    .then(transform)
    .then(enrich)
    .then(save)
    .catch(err => {
      logger.error('Pipeline failed:', err);
      throw err;
    });

// Multiple composition options
const customPipeline = compose(
  save,
  enrich,
  transform,
  validate
);
```

---

### Example 6: Service Dependencies
**Domain**: Spring Boot microservice
```java
@Service
@RequiredArgsConstructor
public class OrderService {
    private final UserService userService;
    private final PaymentService paymentService;
    private final InventoryService inventoryService;
    
    @Transactional
    public Order createOrder(OrderRequest request) {
        User user = userService.validateUser(request.userId);
        inventoryService.reserve(request.items);
        Payment payment = paymentService.charge(user, request.total);
        return new Order(user, payment, request.items);
    }
}
```

---

### Example 7: Microservice Entanglement
**Domain**: Distributed system with hidden coupling
```javascript
// Order Service
async function createOrder(orderData) {
  const user = await userService.getUser(orderData.userId);
  const inventory = await inventoryService.check(orderData.items);
  const payment = await paymentService.authorize(user, orderData.total);
  
  // If payment fails, need to rollback inventory
  if (!payment.success) {
    await inventoryService.release(orderData.items);
    throw new Error('Payment failed');
  }
  
  // Notify all dependent services
  await Promise.all([
    shippingService.schedule(orderData),
    emailService.notify(user, orderData),
    analyticsService.track('order', orderData)
  ]);
}
```

---

### Example 8: Type Gymnastics
**Domain**: Over-abstracted TypeScript
```typescript
type DeepPartial<T> = T extends object ? {
  [P in keyof T]?: DeepPartial<T[P]>;
} : T;

type AsyncReturnType<T extends (...args: any) => Promise<any>> = 
  T extends (...args: any) => Promise<infer R> ? R : never;

type ConfigBuilder<T> = {
  [K in keyof T]-?: T[K] extends infer U | undefined
    ? (value: U) => ConfigBuilder<T>
    : (value: T[K]) => ConfigBuilder<T>;
} & { build(): T };

// Usage becomes incomprehensible
const config = createConfig<AppConfig>()
  .port(3000)
  .database(dbConfig)
  .build();
```

---

## PART 2: Transformation Studies

### Transformation 1: String Config to Typed Config

**BEFORE**:
```python
# config.py with string interpolation hell
import os

TIMEOUT = os.getenv('TIMEOUT', '30')  # String
DATABASE_URL = f"postgres://localhost:{os.getenv('DB_PORT', 5432)}"

def connect():
    conn = psycopg2.connect(DATABASE_URL, timeout=int(TIMEOUT))
    # Crashes if TIMEOUT not numeric
```

**AFTER**:
```python
# config.py with pydantic
from pydantic import BaseSettings, PostgresDsn

class Settings(BaseSettings):
    timeout: int = 30
    database_url: PostgresDsn
    
    class Config:
        env_file = '.env'

settings = Settings()  # Validates at import time

def connect():
    conn = psycopg2.connect(settings.database_url, timeout=settings.timeout)
```

---

### Transformation 2: Callback Hell to Async/Await

**BEFORE**:
```javascript
getUserData(userId, (err, user) => {
  if (err) return handleError(err);
  
  getOrders(user.id, (err, orders) => {
    if (err) return handleError(err);
    
    getPayments(user.id, (err, payments) => {
      if (err) return handleError(err);
      
      render({ user, orders, payments });
    });
  });
});
```

**AFTER**:
```javascript
async function loadUserDashboard(userId) {
  try {
    const user = await getUserData(userId);
    const [orders, payments] = await Promise.all([
      getOrders(user.id),
      getPayments(user.id)
    ]);
    return { user, orders, payments };
  } catch (err) {
    handleError(err);
  }
}
```

---

### Transformation 3: Rigid to Flexible Service

**BEFORE**:
```typescript
// Rigid but predictable
class EmailSender {
  constructor(private readonly config: SMTPConfig) {
    // Validates config at construction
  }
  
  send(to: string, subject: string, body: string): void {
    // Single way to send
  }
}
```

**AFTER**:
```typescript
// Flexible but complex
class NotificationService {
  constructor(
    private providers: Map<string, NotificationProvider>
  ) {}
  
  async notify(
    recipient: string,
    message: any,
    options?: NotifyOptions
  ): Promise<void> {
    const channel = options?.channel || 'email';
    const provider = this.providers.get(channel);
    
    if (!provider) {
      throw new Error(`No provider for ${channel}`);
    }
    
    await provider.send(recipient, message, options);
  }
}
```

---

### Transformation 4: Runtime to Startup Validation

**BEFORE**:
```javascript
// Node.js service with runtime config checks
class PaymentService {
  async processPayment(amount, currency) {
    const apiKey = process.env.STRIPE_API_KEY;
    if (!apiKey) {
      throw new Error('Missing Stripe API key');
    }
    
    const stripe = new Stripe(apiKey);
    return stripe.charges.create({ amount, currency });
  }
}
```

**AFTER**:
```javascript
// Same pipeline, but config validated at startup
const apiKey = process.env.STRIPE_API_KEY;
if (!apiKey) {
  throw new Error('Missing Stripe API key');
}

const stripe = new Stripe(apiKey);

class PaymentService {
  async processPayment(amount, currency) {
    return stripe.charges.create({ amount, currency });
  }
}
```

---

## PART 3: Edge Cases & Disagreement Zones

### Edge 1: Framework Magic - Feature or Bug?
```python
# Django ORM with implicit queries
class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)

# Later in view
books = Book.objects.all()
for book in books:
    print(book.author.name)  # N+1 query hidden behind attribute access
```

---

### Edge 2: Type Escape Hatches
```typescript
// TypeScript with deliberate anys
function processData(input: any): any {
  // Escape hatch for dynamic data
  return input.map((x: any) => ({
    ...x,
    processed: true
  }));
}

// vs strict version
function processData<T extends {id: string}>(
  input: T[]
): (T & {processed: true})[] {
  return input.map(x => ({...x, processed: true as const}));
}
```

---

### Edge 3: Monadic Patterns in Non-FP Languages
```javascript
// Result monad in JavaScript
class Result {
  static ok(value) { 
    return new Result(value, null);
  }
  
  static err(error) {
    return new Result(null, error);
  }
  
  map(fn) {
    return this.error ? this : Result.ok(fn(this.value));
  }
  
  flatMap(fn) {
    return this.error ? this : fn(this.value);
  }
}

// Usage fights JavaScript idioms
const result = Result.ok(5)
  .map(x => x * 2)
  .flatMap(x => x > 0 ? Result.ok(x) : Result.err("negative"));
```

---

### Edge 4: Intent Changes Everything
**Same code, different documentation**

**Version A - Temporary Bridge**:
```typescript
/**
 * @deprecated Legacy data bridge for v1 API migration.
 * Will be removed once all clients update to v2.
 * 
 * DO NOT use for new features.
 */
function processLegacyData(input: any): any {
  // Type safety deliberately sacrificed for migration
  return { ...input, version: 'legacy' };
}
```

**Version B - Core Architecture**:
```typescript
/**
 * Core plugin data processor.
 * Designed for maximum flexibility to support
 * our dynamic plugin ecosystem.
 */
function processPluginData(input: any): any {
  // Intentionally untyped for plugin authors
  return { ...input, version: 'plugin' };
}
```

---

### Edge 5: Expressiveness vs. Readability
**Ultra-dense functional vs. verbose imperative**

**Version A - Point-free Crystalline**:
```haskell
-- Highly expressive, but cognitively dense
processData :: [Int] -> Int
processData = sum . map (*2) . filter even . take 10
```

**Version B - Step-by-step Clear**:
```python
# Less expressive, but more readable
def process_data(numbers):
    first_ten = numbers[:10]
    even_numbers = [n for n in first_ten if n % 2 == 0]
    doubled = [n * 2 for n in even_numbers]
    return sum(doubled)
```

---

## Testing Protocol

### Protocol Steps
1. **First Pass**: Rate each axis independently
2. **Second Pass**: Consider interactions between axes
3. **Context Check**: Does domain knowledge affect rating?
4. **Intent Check**: For documented code, does intent change rating?
5. **Human Factors**: Consider readability alongside expressiveness
6. **Confidence Level**: High/Medium/Low for each rating

### Example Protocol Walkthrough

**Evaluating Example 4: Framework Magic**

**First Pass (Independent Axis Rating)**:
- Expressive: Consider the variety of ways to interact with models
- Context: Examine callback execution order dependencies
- Error: Identify when and how failures occur

**Second Pass (Interaction Check)**:
- How does high expressiveness interact with dependency structure?
- Do dependencies make errors harder to debug?
- Consider whether axes reinforce each other

### Domain Knowledge Requirements

**Minimal Domain Knowledge Required**:
- Examples 1-3: Basic programming concepts
- Example 5: Understanding of async/promises
- Transformation 1-2: General config/async patterns

**Moderate Domain Knowledge Required**:
- Example 4: Rails callback lifecycle
- Example 6: Dependency injection patterns
- Transformation 3: Service architecture

**High Domain Knowledge Required**:
- Edge 1: Django ORM behavior (N+1 queries)
- Edge 3: Monadic patterns and language idioms
- Edge 5: Point-free style implications

---

## Key Questions for Assessment

1. When does expressiveness become harmful?
2. How do we rate "good patterns in bad contexts"?
3. Should language idioms affect VIBES ratings?
4. Is rigid but safe always better than flexible with runtime errors?
5. How do we handle framework magic consistently?
6. Does documented intent change VIBES assessment?
7. How do we balance expressiveness with readability?
8. Can we identify single-axis improvements?

---

## Assessment Guidelines

When providing your VIBES ratings:
- Provide axis-by-axis rationales
- State your confidence level
- Note any domain knowledge that influenced your rating
- For edge cases, discuss alternative interpretations
- Consider whether the pattern would help or hinder LLM code generation