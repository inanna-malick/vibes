# Error Domains: Visual Type Systems for Failure

Error handling designed for pattern matching rather than hierarchical inheritance.

## Core Principle

Human error systems optimize for:
- Hierarchical exceptions (IOException → FileNotFoundException)
- Stack traces and debugging
- Try-catch locality
- Detailed error messages

LLM error systems optimize for:
- Visual domain recognition
- Pattern matching across contexts
- Semantic grouping
- Compositional error handling

## Design Sketch

### Traditional Error Handling
```python
# Hierarchical, text-based, inheritance-focused
try:
    user = db.get_user(user_id)
    if not user:
        raise UserNotFoundException(f"User {user_id} not found")
    
    order = process_order(user)
    payment = charge_payment(order)
    
except UserNotFoundException as e:
    logger.error(f"User error: {e}")
    return 404
except PaymentException as e:
    logger.error(f"Payment failed: {e}")
    refund_order(order)
    return 402
except DatabaseException as e:
    logger.error(f"Database error: {e}")
    return 500
```

### LLM-Optimized Error Domains
```typescript
// Visual domains with semantic grouping

// Domain definitions
const ErrorDomains = {
  🚫: "General failure",
  🔒: "Security/Permission",
  💳: "Payment/Financial", 
  📡: "Network/Connection",
  💾: "Storage/Database",
  ⏱️: "Timeout/Performance",
  🔍: "NotFound/Missing",
  ✅: "Success/Completion"
} as const;

// Errors as domain-tagged values
type DomainError<T> = {
  domain: keyof typeof ErrorDomains;
  code: string;
  context: T;
  recovery?: () => void;
};

// Usage - visual pattern matching
async function processUserOrder(userId: string) {
  const user = await findUser(userId);
  
  // Pattern match on domains
  switch (user.domain) {
    case '🔍':
      return { domain: '🔍', code: 'USER_MISSING', context: { userId } };
    
    case '💾':
      return { domain: '💾', code: 'DB_UNAVAILABLE', context: { retry: true } };
    
    case '✅':
      // Continue processing
      const order = await createOrder(user.data);
      return handleOrderResult(order);
  }
}

// Compositional error handling
function handleOrderResult(result: DomainResult<Order>) {
  // Match on multiple domains
  if (result.domain === '💳') {
    // Payment-specific handling
    return retryWithAlternativePayment(result);
  }
  
  if (result.domain === '📡') {
    // Network-specific handling
    return queueForRetry(result);
  }
  
  if (result.domain === '✅') {
    // Success path
    return completeOrder(result.data);
  }
  
  // Generic failure
  return { domain: '🚫', code: 'UNKNOWN_ERROR', context: result };
}
```

## Domain-Based Error Flows

### 1. Security Domain (🔒)
```yaml
🔒 Unauthorized:
  - Missing credentials
  - Invalid token
  - Expired session
  - Insufficient permissions
  
🔒 Forbidden:
  - Rate limit exceeded  
  - IP blocked
  - Account suspended
  - Feature not enabled

# Recovery patterns
on 🔒 error:
  if Missing credentials:
    redirect to login
  if Expired session:
    attempt token refresh
  if Rate limited:
    implement backoff
```

### 2. Data Domain (💾)
```yaml
💾 Connection:
  - Database unreachable
  - Connection pool exhausted
  - Query timeout
  
💾 Integrity:
  - Constraint violation
  - Duplicate key
  - Foreign key missing
  
💾 Transaction:
  - Deadlock detected
  - Rollback required
  - Isolation conflict

# Recovery patterns  
on 💾 error:
  if Connection:
    try replica
  if Integrity:
    validate inputs
  if Transaction:
    retry with backoff
```

### 3. Network Domain (📡)
```yaml
📡 Connectivity:
  - DNS resolution failed
  - Connection refused
  - Network unreachable
  
📡 Protocol:
  - TLS handshake failed
  - Certificate invalid
  - Protocol mismatch
  
📡 Transfer:
  - Timeout during transfer
  - Connection reset
  - Partial content

# Recovery patterns
on 📡 error:
  if DNS failed:
    try alternate endpoints
  if Certificate:
    check system time
  if Timeout:
    increase limits
```

## Compositional Error Types

```typescript
// Errors can combine domains
type CompositeError = {
  primary: DomainError<any>;
  secondary?: DomainError<any>;
  cascade?: DomainError<any>[];
};

// Example: Payment failure due to network issues
const paymentNetworkError: CompositeError = {
  primary: { 
    domain: '💳', 
    code: 'PAYMENT_FAILED',
    context: { amount: 99.99 }
  },
  secondary: {
    domain: '📡',
    code: 'GATEWAY_TIMEOUT', 
    context: { endpoint: 'stripe.com' }
  }
};

// Pattern matching on composite errors
function handleComposite(error: CompositeError) {
  // Check primary domain first
  if (error.primary.domain === '💳') {
    // Then check secondary for root cause
    if (error.secondary?.domain === '📡') {
      return queuePaymentRetry(error);
    }
    if (error.secondary?.domain === '💾') {
      return savePaymentForManualReview(error);
    }
  }
}
```

## Railway-Oriented Error Handling

```typescript
// Every operation returns Success or Error domain
type Result<T> = 
  | { domain: '✅', data: T }
  | { domain: '🚫' | '🔒' | '💳' | '📡' | '💾' | '⏱️' | '🔍', error: any };

// Chain operations on success rail
const processUser = (id: string): Result<ProcessedUser> =>
  findUser(id)
    |> validateUser
    |> enrichUserData  
    |> checkPermissions
    |> finalizeUser;

// Visual flow representation
/*
  findUser → 
    ✅ → validateUser →
      ✅ → enrichUserData →
        ✅ → checkPermissions →
          ✅ → finalizeUser → ✅
          🔒 → "Forbidden"
        💾 → "Database Error"
      🔍 → "Invalid User"
    🔍 → "User Not Found"
*/
```

## Implementation Patterns

### Error Factory with Domains
```typescript
class DomainErrors {
  static notFound<T>(context: T) {
    return { domain: '🔍' as const, code: 'NOT_FOUND', context };
  }
  
  static unauthorized<T>(context: T) {
    return { domain: '🔒' as const, code: 'UNAUTHORIZED', context };
  }
  
  static timeout<T>(context: T, duration: number) {
    return { 
      domain: '⏱️' as const, 
      code: 'TIMEOUT', 
      context: { ...context, duration }
    };
  }
  
  static database<T>(code: string, context: T) {
    return { domain: '💾' as const, code, context };
  }
}

// Usage
const error = DomainErrors.notFound({ userId: '123' });
if (error.domain === '🔍') {
  // TypeScript knows this is a not-found error
}
```

### Cross-Service Error Propagation
```yaml
# Service A
on request failed:
  return 📡 NetworkError:
    service: "ServiceB"
    attempt: 3
    lastError: "connection refused"

# API Gateway
on receiving 📡 from ServiceA:
  if attempt < maxRetries:
    route to ServiceB-replica
  else:
    return 📡 to client:
      message: "Service temporarily unavailable"
      retryAfter: 30
```

## Visual Error Aggregation

```typescript
// Dashboard shows errors by domain
const errorStats = {
  '🔒': 45,   // Security errors
  '💾': 12,   // Database errors  
  '📡': 23,   // Network errors
  '💳': 3,    // Payment errors
  '🔍': 156,  // Not found errors
  '⏱️': 8,    // Timeout errors
};

// Visual health indicator
function systemHealth(stats: ErrorStats): string {
  const total = Object.values(stats).sum();
  const critical = stats['💾'] + stats['🔒'];
  
  if (critical > 50) return '🔴 Critical';
  if (total > 100) return '🟡 Degraded';
  return '🟢 Healthy';
}
```

## Why This Works for LLMs

1. **Visual Pattern Matching**: Domains create instant recognition
2. **Cross-Context Consistency**: 🔒 means security everywhere
3. **Compositional Reasoning**: Errors combine predictably
4. **No Memorization**: Don't need to remember exception hierarchies
5. **Railway Thinking**: Success/failure paths are visual

## Integration with VIBES-Lang

```vibes
service UserAPI {
  🔍 operation findUser(id: UserId) → User | 🚫 {
    errors: [🔍 NotFound, 💾 DatabaseError]
    
    implementation {
      result = database.query(id)
      match result {
        ✅ User → return User
        🔍 → return 🔍 NotFound(id)
        💾 → return 💾 DatabaseError("Query failed")
        _ → return 🚫 UnknownError
      }
    }
  }
}
```

## VIBES Analysis

**Rating**: `<🔬🎀💠>`

- **Expressive** (🔬): Multiple ways to handle same error patterns
- **Context** (🎀): Domains are independent and composable
- **Error** (💠): Domain mismatches caught at compile time

## Navigation

- [Back to LLM Tools](./README.md)
- [VIBES-Lang](../../vibes-lang/)
- [Error Handling Guide](../../guides/)
