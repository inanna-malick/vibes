# Semantic API Design for LLMs

An API design that provides multiple semantically equivalent endpoints, all compiling to the same underlying operations.

## Core Principle

Human APIs optimize for:
- DRY (Don't Repeat Yourself)
- Single canonical path
- Terse endpoints (`/users/{id}`)
- Consistent naming conventions

LLM APIs optimize for:
- Multiple semantic paths
- Redundant clarity
- Self-describing endpoints
- Concept-first design

## Design Sketch

### Traditional REST API
```http
# Terse, canonical, positional
GET /users/123
POST /users
PUT /users/123
DELETE /users/123
GET /users/123/orders
GET /orders?user_id=123
```

### LLM-Optimized Semantic API
```http
# Multiple equivalent endpoints (all route to same handler)

# User retrieval - many ways to express the same intent
GET /users/with-id/123
GET /users/having-identifier/123
GET /find-user-by-id/123
GET /user-lookup/id/123
GET /👤/identify/123

# User creation - semantic variations
POST /users/create
POST /users/new
POST /create-new-user
POST /register-user
POST /👤/➕

# With body:
{
  "action": "create-user",
  "data": {
    "email": "user@example.com",
    "name": "Alice Smith"
  }
}

# Relationship queries - multiple mental models
GET /users/123/orders
GET /orders/for-user/123
GET /orders/belonging-to/user/123
GET /find-orders-where-user-is/123
GET /👤/123/📦
GET /📦/owned-by/👤/123
```

## Semantic Aliasing System

### 1. Action Aliases
```yaml
# All map to same underlying operation
actions:
  retrieve:
    - get
    - fetch
    - find
    - lookup
    - retrieve
    - 🔍
    
  create:
    - create
    - new
    - add
    - register
    - make
    - ➕
    
  update:
    - update
    - modify
    - change
    - edit
    - patch
    - ✏️
    
  delete:
    - delete
    - remove
    - destroy
    - purge
    - 🗑️
```

### 2. Relationship Expressions
```http
# All equivalent ways to express "user's orders"
GET /users/{id}/orders
GET /orders/of-user/{id}
GET /orders/for/{id}
GET /orders?user={id}
GET /orders?belonging-to-user={id}
GET /relationships/user/{id}/has/orders
```

### 3. Query Semantics
```http
# Multiple ways to filter
GET /users?status=active
GET /users/with-status/active
GET /users/having/status/active
GET /users/where/status/equals/active
GET /active-users
GET /users/filtered-by/status/active

# Complex queries
GET /orders/recent?days=7&status=shipped
GET /orders/shipped-in-last/7/days
GET /recent-shipped-orders?within-days=7
GET /find-orders/status/shipped/within/7/days
```

## Response Envelopes

Responses include semantic metadata:

```json
{
  "request": {
    "semantic_intent": "retrieve-user-by-id",
    "canonical_path": "/users/123",
    "alternatives": [
      "/users/with-id/123",
      "/find-user-by-id/123",
      "/user-lookup/id/123"
    ]
  },
  "data": {
    "id": 123,
    "name": "Alice Smith",
    "email": "alice@example.com"
  },
  "relationships": {
    "orders": "/users/123/orders",
    "profile": "/users/123/profile",
    "settings": "/users/123/settings"
  }
}
```

## Domain-Based Endpoints

Using emoji domains for visual categorization:

```http
# User domain (👤)
GET /👤/123
POST /👤/new
PUT /👤/123/update
DELETE /👤/123

# Order domain (📦)
GET /📦/456
GET /📦/for/👤/123
POST /📦/create

# Analytics domain (📊)
GET /📊/users/active/today
GET /📊/orders/trend/weekly
GET /📊/revenue/by-month/2025

# Search domain (🔍)
GET /🔍/users?name=alice
GET /🔍/orders?status=pending
GET /🔍/all?query=smith
```

## Compositional Endpoints

Build complex queries through composition:

```http
# Base + filters + modifiers
GET /users
    /with-role/admin
    /created-after/2025-01-01
    /sorted-by/last-login
    /limit/10

# Equivalent to:
GET /users?role=admin&created_after=2025-01-01&sort=last_login&limit=10

# Or:
GET /admin-users/recent/sorted/by-login/top/10
```

## GraphQL Alternative with Semantic Fields

```graphql
# Multiple field aliases resolve to same data
type User {
  id: ID!
  identifier: ID!  # Alias for id
  user_id: ID!     # Alias for id
  
  name: String!
  full_name: String!     # Alias for name
  display_name: String!  # Alias for name
  
  orders: [Order!]!
  purchases: [Order!]!     # Alias for orders
  transactions: [Order!]!  # Alias for orders
}

query GetUserWithOrders {
  findUser(id: 123) {
    identifier
    display_name
    purchases {
      id
      total
    }
  }
}

# Equivalent query with different semantics
query RetrieveUserPurchases {
  userById(identifier: 123) {
    user_id
    full_name
    transactions {
      id
      total
    }
  }
}
```

## Implementation Sketch

```python
from typing import List, Callable
import re

class SemanticRouter:
    def __init__(self):
        self.routes = {}
        self.aliases = {}
        
    def register_handler(self, handler: Callable, patterns: List[str]):
        """Register multiple patterns for same handler"""
        canonical = patterns[0]
        for pattern in patterns:
            regex = self._pattern_to_regex(pattern)
            self.routes[regex] = handler
            self.aliases[regex] = canonical
            
    def _pattern_to_regex(self, pattern: str) -> re.Pattern:
        # Convert /users/{id}/orders to regex
        # Handle {id} -> (?P<id>[^/]+)
        pass
        
    def route(self, path: str):
        for regex, handler in self.routes.items():
            if match := regex.match(path):
                return handler, match.groupdict(), self.aliases[regex]
        return None, None, None

# Usage
router = SemanticRouter()
router.register_handler(
    get_user,
    [
        "/users/{id}",
        "/users/with-id/{id}",
        "/find-user-by-id/{id}",
        "/user-lookup/id/{id}",
        "/👤/{id}"
    ]
)
```

## Why This Works for LLMs

1. **Pattern Recognition**: Multiple paths express same intent
2. **Semantic Clarity**: Endpoints describe their purpose
3. **No Memorization**: Don't need to remember exact paths
4. **Natural Language**: Reads like English
5. **Visual Domains**: Emojis provide instant context

## Trade-offs

- **Larger API Surface**: More endpoints to document/maintain
- **Routing Complexity**: Multiple patterns per handler
- **Potential Ambiguity**: Must carefully design to avoid conflicts
- **Performance**: Slightly more complex routing logic

## VIBES Analysis

**Rating**: `<🔬🪢🧊>`

- **Expressive** (🔬): Many valid paths to same resource
- **Context** (🪢): Clear request/response pipeline
- **Error** (🧊): Route validation at startup

## Navigation

- [Back to LLM Tools](./README.md)
- [LLM CLI](./llm-cli.md)
- [Config Language](./config-lang.md)
