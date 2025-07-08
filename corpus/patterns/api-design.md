# API Design Pattern

## Pattern Name
RESTful API Design

## VIBES Rating
<🔍🪢💧>

## Description
Well-structured REST API with clear resource modeling, consistent naming conventions, and explicit error handling. Represents good intermediate practice - better than string chaos but not yet crystalline.

## Code Example
```typescript
// API route definitions
const routes = {
  users: {
    list: 'GET /api/users',
    get: 'GET /api/users/:id',
    create: 'POST /api/users',
    update: 'PUT /api/users/:id',
    delete: 'DELETE /api/users/:id'
  },
  posts: {
    list: 'GET /api/posts',
    byUser: 'GET /api/users/:userId/posts'
  }
}

// Error handling middleware
app.use((err, req, res, next) => {
  const status = err.status || 500;
  const message = err.message || 'Internal Server Error';
  
  res.status(status).json({
    error: {
      message,
      status,
      timestamp: new Date().toISOString()
    }
  });
});

// Resource handler with validation
async function createUser(req, res, next) {
  try {
    const { email, name } = req.body;
    
    if (!email || !name) {
      return res.status(400).json({
        error: { message: 'Email and name required' }
      });
    }
    
    const user = await userService.create({ email, name });
    res.status(201).json({ data: user });
  } catch (error) {
    next(error);
  }
}
```

## Why This Rating?

### Expressive Power: 🔍 (Structured)
- Multiple ways to structure endpoints (nested vs flat)
- Clear conventions guide design
- Flexibility in response formats

### Context Flow: 🪢 (Pipeline)
- Request → Validation → Service → Response
- Clear linear flow with middleware chain
- Each step depends on previous

### Error Surface: 💧 (Liquid)
- Errors handled at runtime
- Try-catch blocks throughout
- Validation happens during execution

## Anti-Pattern Version
<🙈🧶🌊>
```javascript
// String-based routing chaos
app.get('/*', (req, res) => {
  const path = req.path;
  if (path.includes('user')) {
    if (req.method === 'GET') {
      // Parse ID from somewhere in path...
    }
  }
  // String parsing nightmares...
});
```

## Crystalline Version
<🔬🪢💠>
```typescript
// Type-safe API with compile-time validation
const api = createAPI<{
  users: {
    list: { query: { limit?: number }, response: User[] }
    get: { params: { id: UUID }, response: User }
    create: { body: CreateUserDTO, response: User }
  }
}>()

// Compile-time type checking
api.users.create({
  body: { email: Email, name: NonEmptyString }
}) // Type error if invalid
```

## Context Considerations
- As internal API: Can achieve 💠 with shared types
- As public API: Limited to 💧 due to runtime validation
- With GraphQL: Different patterns, potentially 🔬🎀🧊

## Related Patterns
- [[validation-chain]] - For request validation
- [[error-domains]] - For error handling
- [[typed-schema]] - For type-safe versions

## References
- REST architectural constraints
- OpenAPI/Swagger specifications
- JSON:API standard
