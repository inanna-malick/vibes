# Pattern: Typed Schema Validation

## VIBES Rating
`<🔍🪢💠>`

## Description
Schema validation at parse boundaries ensures data integrity throughout the system. Invalid data cannot enter, making runtime errors about data shape impossible.

## Example
```typescript
// Zod schema defines the contract at system boundary
import { z } from 'zod';

const UserSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  age: z.number().int().min(0).max(150),
  preferences: z.object({
    theme: z.enum(['light', 'dark']),
    notifications: z.boolean()
  })
});

type User = z.infer<typeof UserSchema>;

// API endpoint with parse-time validation
async function handleUserUpdate(request: Request) {
  // Parse fails at boundary if data invalid
  const body = await request.json();
  const validatedUser = UserSchema.parse(body); // Throws if invalid
  
  // After this point, TypeScript knows the exact shape
  await updateUser(validatedUser); // Can't pass invalid data
  
  return { success: true, user: validatedUser };
}

// Internal functions work with guaranteed-valid types
async function updateUser(user: User) {
  // No validation needed - type system guarantees validity
  await db.update('users', user.id, {
    email: user.email,
    age: user.age,
    theme: user.preferences.theme
  });
  
  // Can't access non-existent fields
  // user.invalidField - TypeScript error
}

// Configuration also validated at startup
const ConfigSchema = z.object({
  port: z.number().int().min(1).max(65535),
  database: z.object({
    host: z.string(),
    port: z.number(),
    name: z.string()
  })
});

// App can't start with invalid config
const config = ConfigSchema.parse(process.env);
```

## Why This Rating

### Expressive Power: 🔍
- Clear model: validate at boundaries, trust internally
- Schema serves as documentation and validation
- Multiple ways to define schemas (inline, composed, extended)
- Type inference provides IDE support

### Context Flow: 🪢
- Linear validation pipeline: raw data → schema → typed data
- Validation happens once at entry points
- Internal functions receive pre-validated data
- Clear data flow from untrusted to trusted

### Error Surface: 💠
- Invalid states cannot be constructed past parse boundary
- Schema mismatches caught at compile time
- Type system prevents accessing undefined fields
- Runtime behavior guaranteed by types

## Common Occurrence
- Modern TypeScript APIs
- GraphQL resolvers
- Configuration management
- Data pipeline entry points
- Form validation systems

## Impact
- LLMs can assume data validity after boundaries
- Type inference guides correct usage
- Refactoring is safe with compiler assistance
- Testing focuses on business logic, not data validation

## Related Patterns
- [runtime-validation.md](./runtime-validation.md) - What this improves upon
- [parse-dont-validate.md](./parse-dont-validate.md) - The principle behind this
- [parser-combinators.md](./parser-combinators.md) - Similar boundary philosophy

## Consensus Data
- Model Agreement: 94%
- Disputed Aspects: Some models want 🔬 when schemas are highly compositional
- Test Date: July 2025
