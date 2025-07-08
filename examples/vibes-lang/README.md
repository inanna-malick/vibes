# VIBES-Lang: An LLM-Optimized Expression Language

VIBES-Lang demonstrates how emojis can serve as semantic domain markers, creating visual type systems that LLMs can pattern-match across contexts.

## Core Concept: Semantic Domains

Instead of parsing strings like "NotFoundException" or "UnauthorizedError", LLMs recognize that anything prefixed with 🚫 belongs to the error domain. This creates immediate, cross-linguistic pattern recognition.

## Language Specification

### Domain Definitions

```vibes
domains {
  🚫 = Error/Failure states
  ✅ = Success/Completion states  
  🔒 = Security/Permission states
  📊 = Observability/Metrics
  ⚡ = Performance/Optimization
  🔄 = Async/Concurrent operations
  🔍 = Query/Search operations
  🗑️ = Deletion/Cleanup operations
  📝 = Update/Modification operations
}
```

### Service Definition Example

```vibes
service UserAPI {
  # Query operation returning User or any Error-domain value
  🔍 operation findUser(id: UserId) → User | 🚫 {
    errors: [🚫 NotFound, 🚫 DatabaseTimeout]
    requires: [🔒 Authenticated]
    effects: [📊 metrics.query_count++]
  }
  
  # Update operation with security requirements
  📝 operation updateProfile(id: UserId, data: ProfileData) → ✅ | 🚫 {
    requires: [🔒 Owner(id) | 🔒 Admin]
    effects: [
      📊 audit.log("Profile updated for ${id}"),
      🔄 async.notify(user.followers)
    ]
  }
  
  # Deletion with cascading async effects
  🗑️ operation deleteUser(id: UserId) → ✅ | 🚫 {
    requires: [🔒 Admin, 🔒 AuditLog]
    effects: [
      📊 audit.write("User ${id} deleted"),
      🔄 async.cascade_delete(user.content),
      🔄 async.notify(user.contacts)
    ]
  }
}
```

### Pattern Matching on Domains

```vibes
# Match on entire domains, not specific values
match operation.result {
  ✅ → continue()           # Any success state
  🚫 NotFound → create()    # Specific error
  🚫 → retry()              # Any other error
  🔒 → escalate()           # Any security issue
}

# Combine domains for nuanced handling
match api_response {
  🔒🚫 → alert_security()   # Security-related error
  ⚡✅ → cache_result()     # Performance-optimized success
  🔄🚫 → retry_async()      # Async operation failed
  _ → log_unknown()
}
```

### Performance Hints and Caching

```vibes
function processUsers(users: List<User>) → ✅ | 🚫 {
  ⚡ parallel: true         # Performance domain hint
  ⚡ cache: 5.minutes       # Cache directive
  ⚡ batch_size: 100        # Optimization parameter
  
  return users
    |> 🔄 map(validate)     # Async map operation
    |> 🔄 filter(active)    # Async filter
    |> ✅                   # Success domain result
}
```

### Preconditions and Error Propagation

```vibes
# Domain-based preconditions
operation transferFunds(from: Account, to: Account, amount: Money) → ✅ | 🚫 {
  preconditions {
    from.balance >= amount else 🚫 InsufficientFunds
    to.status == Active else 🚫 AccountInactive
    amount > 0 else 🚫 InvalidAmount
  }
  
  requires: [
    🔒 Authenticated,
    🔒 VerifiedIdentity,
    🔒 TransactionLimit(amount)
  ]
  
  effects: [
    📊 audit.transaction(from, to, amount),
    🔄 async.notify_parties([from.owner, to.owner])
  ]
}
```

### Observability Integration

```vibes
# Metrics and tracing as first-class domains
pipeline DataProcessor {
  📊 trace: "data-processing"
  📊 metrics: ["latency", "throughput", "error_rate"]
  
  stage extract() → RawData | 🚫 {
    📊 timer: "extract_duration"
    📊 counter: "records_extracted"
  }
  
  stage transform(data: RawData) → ProcessedData | 🚫 {
    📊 histogram: "transform_complexity"
    ⚡ parallel: true
  }
  
  stage load(data: ProcessedData) → ✅ | 🚫 {
    📊 gauge: "records_loaded"
    🔄 retry: 3.times
  }
}
```

## Why This Works for LLMs

### Visual Namespacing
- `🚫 Unauthorized` immediately signals "error domain" without string parsing
- Pattern recognition works across different natural languages
- Reduces ambiguity in multi-lingual contexts

### Cross-Context Patterns
- LLMs recognize 🔒 means "security-related" everywhere in the codebase
- Consistent semantic marking across different services and modules
- Domain knowledge transfers between similar systems

### Compositional Semantics
- Can combine domains: `🔒🚫 SecurityError` is both security AND error
- Hierarchical matching: match on 🚫 catches all errors including 🔒🚫
- Natural precedence: more specific patterns match first

### Reduced Token Complexity
- Single emoji token vs multi-token class names
- Visual distinctiveness aids pattern matching
- Consistent token boundaries improve context efficiency

## VIBES Analysis of VIBES-Lang

**Rating**: `<🔬🪢💠>`

**Expressive Power (🔬)**: Multiple valid ways to express operations, all guided by domain semantics. Invalid domain combinations are syntactically impossible.

**Context Flow (🪢)**: Linear pipeline operations with clear data flow. Effects are explicit side-channels that don't affect main flow.

**Error Surface (💠)**: Domain mismatches caught at parse time. Security requirements validated before execution. Invalid states cannot be constructed.

## Navigation

- [Back to Examples](../README.md)
- [Formal Syntax](../../notation/formal-syntax.md)
- [View Implementation](./implementation.md)
