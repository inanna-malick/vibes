# VIBES Boundary Examples

Understanding the boundaries between adjacent states is crucial for accurate assessment. These examples show the key distinctions.

## Context Flow: 🧶 (Coupled) vs 🪢 (Pipeline)

The key test: **Can state changes in one component affect others without explicit data flow?**

### 🧶 Coupled Example
```javascript
// React with shared context and effects
const ThemeContext = React.createContext();
const UserContext = React.createContext();

function Header() {
  const theme = useContext(ThemeContext);
  const user = useContext(UserContext);
  
  // This effect changes global state
  useEffect(() => {
    if (user.preferences.darkMode) {
      theme.setDark(); // Side effect affects other components
    }
  }, [user]);
  
  return <h1 style={{color: theme.color}}>Hello {user.name}</h1>;
}

function Settings() {
  const theme = useContext(ThemeContext);
  const user = useContext(UserContext);
  
  // Changing this affects Header without explicit flow
  const toggleDarkMode = () => {
    user.preferences.darkMode = !user.preferences.darkMode;
    user.save(); // This mutation affects Header
  };
}
```

**Why 🧶**: Components share mutable state through context. Changes in Settings affect Header through hidden state mutations, not explicit data flow.

### 🪢 Pipeline Example
```javascript
// Functional pipeline with explicit flow
const pipeline = (initialData) => {
  return validateUser(initialData)
    .then(user => enrichUserData(user))
    .then(enrichedUser => applyTheme(enrichedUser))
    .then(themedUser => renderUser(themedUser));
};

// Each function only knows about its input
const validateUser = (data) => {
  if (!data.email) throw new Error('Email required');
  return { ...data, validated: true };
};

const enrichUserData = (user) => {
  return {
    ...user,
    fullName: `${user.firstName} ${user.lastName}`,
    memberSince: calculateMembershipLength(user.joinDate)
  };
};

// No hidden state or side effects
const applyTheme = (user) => {
  return {
    ...user,
    theme: user.preferences.darkMode ? darkTheme : lightTheme
  };
};
```

**Why 🪢**: Each step depends only on its input. Data flows linearly through the pipeline. No hidden state mutations or side effects.

## Error Surface: 💧 (Runtime Handled) vs 🧊 (Initialization)

The key test: **When is the FIRST moment this error is detected?**

### 💧 Runtime Handled Example
```python
class APIClient:
    def __init__(self, base_url):
        self.base_url = base_url  # No validation here
    
    def get_user(self, user_id):
        try:
            # Validation happens during operation
            if not isinstance(user_id, int):
                raise ValueError("User ID must be integer")
            
            response = requests.get(f"{self.base_url}/users/{user_id}")
            response.raise_for_status()
            return response.json()
            
        except requests.RequestException as e:
            # Errors handled at runtime
            logger.error(f"API request failed: {e}")
            return None
        except ValueError as e:
            logger.error(f"Invalid input: {e}")
            return None

# Client creation succeeds even with bad config
client = APIClient("not-a-url")  # No error yet

# Error only surfaces when used
user = client.get_user("invalid")  # Runtime error
```

**Why 💧**: Errors are caught and handled during operation. Invalid configuration doesn't fail until runtime.

### 🧊 Initialization Example
```python
from pydantic import BaseModel, AnyHttpUrl, validator

class APIConfig(BaseModel):
    base_url: AnyHttpUrl
    timeout: int
    max_retries: int
    
    @validator('timeout')
    def timeout_positive(cls, v):
        if v <= 0:
            raise ValueError('Timeout must be positive')
        return v
    
    @validator('max_retries')
    def retries_reasonable(cls, v):
        if v < 0 or v > 10:
            raise ValueError('Retries must be between 0 and 10')
        return v

class APIClient:
    def __init__(self, config_dict):
        # Validation happens at initialization
        self.config = APIConfig(**config_dict)  # Fails here if invalid
        
        # Test connection at startup
        try:
            response = requests.get(f"{self.config.base_url}/health", 
                                  timeout=self.config.timeout)
            response.raise_for_status()
        except Exception as e:
            raise ConnectionError(f"Failed to connect to API: {e}")
    
    def get_user(self, user_id: int):
        # By this point, we know config is valid
        response = requests.get(
            f"{self.config.base_url}/users/{user_id}",
            timeout=self.config.timeout
        )
        return response.json()

# Fails at creation with clear error
try:
    client = APIClient({
        "base_url": "not-a-url",  # Fails validation
        "timeout": -5,            # Fails validation
        "max_retries": 100        # Fails validation
    })
except ValidationError as e:
    print("Configuration invalid:", e)
    # Application can't start with bad config
```

**Why 🧊**: All validation happens at initialization. Invalid configuration prevents the client from being created. Application fails fast at startup.

## Error Surface: 🌊 (Cascading) vs 💧 (Contained)

### 🌊 Cascading Example
```javascript
// Global error state that affects entire app
window.APP_STATE = {
  user: null,
  cart: [],
  config: {}
};

function updateUserProfile(updates) {
  // No error handling
  window.APP_STATE.user = {
    ...window.APP_STATE.user,
    ...updates
  };
  
  // This might fail if user is null
  document.getElementById('username').innerText = window.APP_STATE.user.name;
  
  // Cascade: cart logic depends on user
  updateCartPricing(); // Fails if user.discount is undefined
  
  // More cascades
  updateRecommendations(); // Fails if user.preferences missing
  updateTheme(); // Fails if user.theme missing
}

function updateCartPricing() {
  // Assumes user exists and has discount
  const discount = window.APP_STATE.user.memberLevel.discount;
  window.APP_STATE.cart.forEach(item => {
    item.finalPrice = item.price * (1 - discount); // TypeError cascades
  });
}
```

**Why 🌊**: One error (null user) cascades through multiple systems. No boundaries contain the failure.

### 💧 Contained Example
```javascript
class UserProfile {
  constructor(user) {
    this.user = user || null;
  }
  
  update(updates) {
    try {
      if (!this.user) {
        return { success: false, error: 'No user to update' };
      }
      
      this.user = { ...this.user, ...updates };
      return { success: true, user: this.user };
      
    } catch (error) {
      return { success: false, error: error.message };
    }
  }
}

class ShoppingCart {
  constructor(userProfile) {
    this.userProfile = userProfile;
    this.items = [];
  }
  
  updatePricing() {
    try {
      const discount = this.getDiscount();
      this.items = this.items.map(item => ({
        ...item,
        finalPrice: item.price * (1 - discount)
      }));
      return { success: true };
      
    } catch (error) {
      // Error contained here
      console.error('Pricing update failed:', error);
      return { success: false, error: 'Could not update pricing' };
    }
  }
  
  getDiscount() {
    // Safe navigation with defaults
    return this.userProfile?.user?.memberLevel?.discount || 0;
  }
}
```

**Why 💧**: Errors are caught and contained within each component. Failures don't cascade to other systems.

## Expressive Power: 🔍 (Coherent) vs 🔬 (Crystalline)

### 🔍 Conceptually Coherent Example
```typescript
// Multiple ways to build queries, all valid but slightly different
class QueryBuilder {
  private query: QueryObject = {};
  
  // Method chaining style
  where(field: string, value: any): this {
    this.query.where = { ...this.query.where, [field]: value };
    return this;
  }
  
  // Object style
  filter(conditions: Record<string, any>): this {
    this.query.where = { ...this.query.where, ...conditions };
    return this;
  }
  
  // SQL-like style
  whereEquals(field: string, value: any): this {
    return this.where(field, { $eq: value });
  }
}

// All these create similar but slightly different queries
const q1 = new QueryBuilder().where('age', 25);
const q2 = new QueryBuilder().filter({ age: 25 });
const q3 = new QueryBuilder().whereEquals('age', 25);
// Results in different internal structures
```

**Why 🔍**: Multiple valid approaches exist, each with subtle differences in the resulting query structure.

### 🔬 Crystalline Example
```haskell
-- Parser combinators: infinite valid combinations, all provably correct
digit :: Parser Char
digit = satisfy isDigit

number :: Parser Int
number = read <$> some digit

-- These different expressions all produce identical behavior
expr1 = number <* spaces
expr2 = number <* many (char ' ')
expr3 = do
  n <- number
  spaces
  return n
expr4 = number >>= \n -> spaces >> return n

-- Type system guarantees all are Parser Int with identical semantics
```

**Why 🔬**: Multiple valid expressions exist, but they all compile to provably identical behavior. The type system ensures correctness regardless of which style you choose.

## Using These Boundaries

When assessing, ask:
1. **For Context**: Can I trace all effects through explicit data flow?
2. **For Errors**: At what point would invalid input first cause failure?
3. **For Expressive**: Do different valid approaches have identical behavior?

## Navigation

- [Back to Glossary](../../GLOSSARY.md)
- [Assessment Guide](../../guides/assessment.md)
- [Axis Definitions](../../VIBES-AXES.md)
