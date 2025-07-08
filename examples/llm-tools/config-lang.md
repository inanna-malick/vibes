# Configuration Language for LLMs

A configuration format that makes invalid states difficult to express while providing multiple valid ways to specify the same configuration.

## Core Principle

Human configs optimize for:
- Minimal repetition (DRY)
- Hierarchical nesting
- Terse syntax
- Implicit defaults

LLM configs optimize for:
- Explicit relationships
- Multiple expression styles
- Self-validating structure
- Semantic groupings

## Design Sketch

### Traditional Configuration (YAML/TOML)
```yaml
# Terse, nested, implicit
server:
  port: 8080
  host: 0.0.0.0
  
database:
  url: postgres://localhost/myapp
  pool: 10
  
logging:
  level: info
  file: /var/log/app.log
  
features:
  - auth
  - api
  - admin
```

### LLM-Optimized Configuration Language
```
# Multiple equivalent ways to express configuration

# Method 1: Semantic grouping with domains
🌐 server configuration {
  listen on port 8080
  accept connections from all interfaces
  use protocol http/2
}

# Method 2: Declarative statements
server should listen on port 8080
server accepts connections from 0.0.0.0
server protocol is http/2 with fallback to http/1.1

# Method 3: Constraint-based
constraints for server {
  port must be between 1024 and 65535
  port is 8080
  
  host must be valid IP or hostname  
  host is "0.0.0.0" meaning all interfaces
  
  protocol must be one of [http/1.1, http/2, https]
  protocol prefers http/2
}

# Database configuration with relationships
🗄️ database setup {
  connect to postgres at localhost
  use database named "myapp"
  
  connection pool {
    minimum connections: 5
    maximum connections: 10
    idle timeout: 30 seconds
  }
  
  when connection fails {
    retry 3 times with exponential backoff
    then alert operations team
  }
}

# Feature flags with explicit dependencies
✨ features enabled {
  authentication {
    requires: database
    provides: user-context
    config: {
      session timeout: 1 hour
      multi-factor: optional
    }
  }
  
  api {
    requires: authentication
    provides: rest-endpoints
    rate-limit: 100 per minute per user
  }
  
  admin-panel {
    requires: [authentication, api]
    restricted-to: users with role admin
  }
}

# Logging with semantic intent
📊 logging configuration {
  capture events at level info and above
  
  write logs to {
    file at "/var/log/app.log" rotating daily keeping 7 days
    console when environment is development
    centralized service when environment is production
  }
  
  for errors {
    include stack traces
    notify operations via slack
  }
}
```

## Language Features

### 1. Domain Markers
```
🌐 networking { ... }
🗄️ database { ... }
📊 monitoring { ... }
🔒 security { ... }
✨ features { ... }
⚡ performance { ... }
```

### 2. Multiple Expression Forms

All equivalent:
```
# Property style
server.port = 8080

# Natural language
server listens on port 8080

# Constraint style
port of server must be 8080

# Declaration style
define server port as 8080
```

### 3. Explicit Relationships
```
service payment-processor {
  depends on: [database, message-queue, auth-service]
  provides: payment-api
  
  if database is unavailable {
    reject new payments
    process pending from cache
  }
}
```

### 4. Self-Validating Constraints
```
# Constraints make invalid configs impossible
url-endpoint signin {
  path must match "/auth/signin"
  method must be POST
  
  requires fields {
    email: valid email address
    password: string length between 8 and 128
  }
  
  rate-limit: 5 attempts per email per hour
  
  on success return {
    token: JWT valid for 24 hours
    user: public profile data
  }
}
```

### 5. Environment-Aware Configuration
```
configuration for environment {
  when development {
    🌐 server listens on localhost:3000
    🗄️ database uses sqlite file "./dev.db"
    📊 logging outputs to console at debug level
  }
  
  when staging {
    🌐 server listens on 0.0.0.0:8080
    🗄️ database connects to postgres://staging.internal/app
    📊 logging outputs to file at info level
  }
  
  when production {
    🌐 server listens on 0.0.0.0:443 with ssl
    🗄️ database connects to postgres://prod.internal/app with connection pool 20
    📊 logging outputs to centralized service at warning level
    ⚡ enable caching with redis
    🔒 require authentication for all endpoints
  }
}
```

## Composition Patterns

### Inheritance with Overrides
```
base-service template {
  timeout: 30 seconds
  retry: 3 times
  circuit-breaker: enabled
}

user-service extends base-service {
  endpoint: "/api/users"
  timeout: 10 seconds  # Override
  cache: 5 minutes     # Addition
}
```

### Conditional Configuration
```
feature email-notifications {
  enabled when {
    environment is production
    and email-service is configured
    and user has opted in
  }
  
  smtp configuration {
    host: env.SMTP_HOST or "smtp.gmail.com"
    port: env.SMTP_PORT or 587
    secure: true
  }
}
```

### Type-Safe References
```
services {
  define auth-service at "https://auth.internal"
  define user-service at "https://users.internal"
}

api-gateway routes {
  "/login" forwards to ${auth-service}/authenticate
  "/profile" forwards to ${user-service}/profile
  
  # Type error - payment-service not defined
  # "/pay" forwards to ${payment-service}/charge
}
```

## Parser Design

```python
from typing import Dict, Any, List
import re

class ConfigParser:
    def __init__(self):
        self.domains = {
            '🌐': 'networking',
            '🗄️': 'database',
            '📊': 'monitoring',
            '🔒': 'security',
            '✨': 'features',
            '⚡': 'performance'
        }
        
    def parse(self, config_text: str) -> Dict[str, Any]:
        # Multiple parsing strategies
        blocks = self.extract_blocks(config_text)
        statements = self.extract_statements(config_text)
        constraints = self.extract_constraints(config_text)
        
        # Merge all interpretations
        return self.merge_interpretations(blocks, statements, constraints)
        
    def validate_constraints(self, config: Dict[str, Any]):
        """Ensure all constraints are satisfied"""
        for constraint in self.constraints:
            if not constraint.satisfied_by(config):
                raise ConfigError(f"Constraint violated: {constraint}")
```

## Why This Works for LLMs

1. **Multiple Valid Expressions**: Natural language variations all parse correctly
2. **Explicit Dependencies**: Relationships clearly stated, not implicit
3. **Semantic Grouping**: Domain markers provide immediate context
4. **Self-Documenting**: Configuration explains its own constraints
5. **Error Prevention**: Invalid states hard to express

## Example: Complete Application Config

```
application my-awesome-app {
  version: 2.1.0
  environment: detect from ENV or default to development
  
  🌐 networking {
    primary server {
      port: env.PORT or 8080
      host: 0.0.0.0
      protocol: http/2 with http/1.1 fallback
    }
    
    health-check at /health returns 200 OK
    metrics endpoint at /metrics requires internal network
  }
  
  🗄️ data storage {
    primary database {
      type: postgresql
      connection: env.DATABASE_URL
      pool: 5 to 20 connections
      
      on connection error {
        retry 3 times with backoff
        then switch to read-only mode
      }
    }
    
    cache with redis {
      url: env.REDIS_URL or "redis://localhost:6379"
      ttl: 5 minutes default
      eviction: least-recently-used
    }
  }
  
  🔒 security policies {
    authentication required for all endpoints except [/health, /login]
    
    jwt tokens {
      secret: env.JWT_SECRET required
      expiration: 24 hours
      refresh-window: 1 hour before expiry
    }
    
    rate-limiting {
      anonymous: 10 requests per minute
      authenticated: 100 requests per minute
      by-endpoint overrides {
        /api/search: 30 per minute
        /api/bulk/*: 10 per hour
      }
    }
  }
  
  ✨ features {
    user-profiles: enabled
    notifications: enabled if email-service configured
    analytics: enabled in production only
    beta-features: enabled for users with flag beta-tester
  }
}
```

## VIBES Analysis

**Rating**: `<🔬🪢💠>`

- **Expressive** (🔬): Many ways to express same configuration
- **Context** (🪢): Linear parsing with clear dependencies
- **Error** (💠): Constraints validated at parse time

## Navigation

- [Back to LLM Tools](./README.md)
- [Error Domains](./error-domains.md)
- [VIBES-Lang](../../vibes-lang/)
