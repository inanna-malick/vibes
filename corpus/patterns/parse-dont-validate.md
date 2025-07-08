# Parse Don't Validate Pattern

## Pattern Name
Parse, Don't Validate

## VIBES Rating
<🔬🪢💠>

## Description
The crystalline pattern of making invalid states unrepresentable in the type system. Instead of validating data repeatedly, parse it once into a type that can only hold valid values. This pattern demonstrates the ultimate achievement: errors that cannot exist.

## Code Example
```haskell
-- The crystalline version: Parse don't validate
newtype Email = Email Text
  deriving (Eq, Show)

newtype Age = Age Int
  deriving (Eq, Show, Ord)

newtype UserId = UserId UUID
  deriving (Eq, Show)

-- Smart constructors return Maybe/Either
mkEmail :: Text -> Maybe Email
mkEmail txt 
  | "@" `isInfixOf` txt = Just (Email txt)
  | otherwise = Nothing

mkAge :: Int -> Maybe Age
mkAge n
  | n >= 0 && n <= 150 = Just (Age n)
  | otherwise = Nothing

-- Once parsed, functions can trust their inputs completely
data User = User
  { userId :: UserId
  , userEmail :: Email
  , userAge :: Age
  }

-- This function CANNOT receive invalid data
sendBirthdayEmail :: User -> IO ()
sendBirthdayEmail user = do
  -- No validation needed - Email is guaranteed valid
  send (userEmail user) "Happy Birthday!"
  -- No null checks - all fields guaranteed present
  log $ "Sent to user " <> show (userId user)

-- Parse at the boundary
createUser :: Text -> Int -> IO (Either UserError User)
createUser emailText ageInt = do
  userId <- UserId <$> nextUUID
  case (mkEmail emailText, mkAge ageInt) of
    (Just email, Just age) -> 
      pure $ Right $ User userId email age
    (Nothing, _) -> 
      pure $ Left InvalidEmail
    (_, Nothing) -> 
      pure $ Left InvalidAge
```

## Why This Rating?

### Expressive Power: 🔬 (Crystalline)
- Types precisely express domain constraints
- Multiple ways to construct (smart constructors, parsers)
- Rich type-level guarantees

### Context Flow: 🪢 (Pipeline)
- Parse at boundaries, use everywhere
- Data flows through type-safe pipelines
- Clear separation of parsing and logic

### Error Surface: 💠 (Crystal)
- Invalid states cannot be constructed
- Errors impossible at compile time
- No defensive programming needed

## The Boundary: 🧊 vs 💠

### Ice Version (🧊 - Startup Validation)
```typescript
// Validation at initialization
class EmailService {
  constructor(private config: Config) {
    // Validate at startup
    if (!this.isValidEmail(config.adminEmail)) {
      throw new Error("Invalid admin email");
    }
  }
  
  sendEmail(to: string, body: string) {
    // Still need runtime validation!
    if (!this.isValidEmail(to)) {
      throw new Error("Invalid recipient email");
    }
    // send...
  }
  
  private isValidEmail(email: string): boolean {
    return email.includes('@');
  }
}
```

### Crystal Version (💠 - This Pattern)
```typescript
// TypeScript approaching crystalline
type Brand<K, T> = K & { __brand: T };
type Email = Brand<string, 'Email'>;
type Age = Brand<number, 'Age'>;

// Parse functions - the only way to create these types
function parseEmail(input: string): Email | null {
  if (input.includes('@')) {
    return input as Email;
  }
  return null;
}

function parseAge(input: number): Age | null {
  if (input >= 0 && input <= 150) {
    return input as Age;
  }
  return null;
}

// Functions can trust their inputs completely
function sendEmail(to: Email, body: string) {
  // No validation needed - Email type guarantees validity
  send(to, body);
}

// Parse at the boundary
function handleRequest(req: Request) {
  const email = parseEmail(req.body.email);
  if (!email) {
    return { error: "Invalid email" };
  }
  
  // Inside this block, email is guaranteed valid
  sendEmail(email, "Welcome!");
}
```

## The Key Insight

### Validation (Inferior Pattern)
```javascript
// Checks repeatedly, errors lurk everywhere
function isValid(email) { return email.includes('@'); }

function process(email) {
  if (!isValid(email)) throw new Error();
  // ... 100 lines later ...
  if (!isValid(email)) throw new Error(); // Paranoid recheck
}
```

### Parsing (Superior Pattern)
```javascript
// Parse once, trust forever
function parseEmail(str) {
  return str.includes('@') ? { tag: 'Email', value: str } : null;
}

function process(email: Email) {
  // email is ALWAYS valid here
  // No checks needed, ever
}
```

## Real-World Examples

### Rust's NonZeroU32
```rust
// Cannot be zero at compile time
let x = NonZeroU32::new(5).unwrap();
let y = NonZeroU32::new(0); // Returns None

// Division by x CANNOT panic
let result = 100u32 / x; // Compiler knows x ≠ 0
```

### Haskell's Natural
```haskell
-- Cannot be negative
import Numeric.Natural

factorial :: Natural -> Natural
factorial 0 = 1
factorial n = n * factorial (n - 1)
-- No need to check for negative numbers!
```

### F# Units of Measure
```fsharp
[<Measure>] type meter
[<Measure>] type second

let speed = 10.0<meter/second>
let time = 5.0<second>
let distance = speed * time  // Type: float<meter>

// Compile error: can't add meters to seconds
// let nonsense = distance + time
```

## Implementation Strategies

### 1. Newtype Pattern
```scala
case class Email private (value: String)
object Email {
  def parse(s: String): Option[Email] = 
    if (s.contains("@")) Some(Email(s)) else None
}
```

### 2. Phantom Types
```typescript
type Validated<T> = T & { readonly __validated: unique symbol };
type Unvalidated<T> = T & { readonly __unvalidated: unique symbol };

function validate(data: Unvalidated<UserData>): Validated<UserData> | null {
  // Validation logic
}
```

### 3. Refinement Types (Dependent Types)
```idris
-- Email is a String that satisfies a predicate
Email : Type
Email = (s : String ** (isEmail s = True))
```

## When to Apply

### Perfect Fit
- Domain has clear validation rules
- Invalid data causes bugs downstream
- Same validation repeated multiple places
- Type system supports it well

### Poor Fit
- Validation rules change frequently
- Performance critical paths (parsing overhead)
- Dynamic/unknown schemas
- Team unfamiliar with advanced types

## Exercises

1. **Find Validation Repetition**: Search for functions that validate the same thing
2. **Design Types**: What would perfect types for your domain look like?
3. **Identify Boundaries**: Where does external data enter your system?
4. **Count Defensive Checks**: How many "just in case" validations exist?
5. **Implement One**: Pick one type and implement parse-don't-validate

## Common Mistakes

### Over-Parsing
```haskell
-- DON'T: Parse everything
newtype StringLongerThan5 = StringLongerThan5 Text
newtype EvenNumber = EvenNumber Int
-- Too granular, diminishing returns
```

### Unsafe Backdoors
```scala
// DON'T: Provide unsafe constructors
case class Email(value: String) // Anyone can construct!
```

### Forgetting Serialization
```haskell
-- DO: Remember the boundaries
instance ToJSON Email where
  toJSON (Email txt) = toJSON txt
  
instance FromJSON Email where
  parseJSON v = parseJSON v >>= \txt ->
    case mkEmail txt of
      Just email -> pure email
      Nothing -> fail "Invalid email"
```

## Related Patterns
- [[smart-constructors]] - Building blocks
- [[phantom-types]] - Type-level guarantees
- [[dependent-types]] - Ultimate validation
- [[type-driven-design]] - Design philosophy

## References
- "Parse, Don't Validate" - Alexis King
- "Making Illegal States Unrepresentable" - Yaron Minsky
- "Type-Driven Development" - Edwin Brady

---

*"Make illegal states unrepresentable" - This is the crystalline ideal of type-driven design. When you achieve it, entire categories of bugs simply cease to exist.*
