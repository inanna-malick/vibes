# String-to-Type Transformation

## Overview
One of the most powerful VIBES transformations: converting stringly-typed systems to type-safe ones. This journey from <🙈🧶🌊> to <🔬🪢💠> demonstrates all three axes improving together.

## The Journey

### Stage 1: Recognition <🙈🧶🌊>
```javascript
// The string soup we've all inherited
function processOrder(orderData) {
  const type = orderData.split('|')[0];
  const items = orderData.split('|')[1].split(',');
  const customer = orderData.split('|')[2];
  
  if (type === 'rush') {
    // Hope the format never changes...
    const priority = orderData.split('|')[3];
    return rushProcess(items, customer, priority);
  }
  // String parsing cascades through system
}

// Called with: processOrder("standard|SKU123,SKU456|CUST789")
// Or was it: processOrder("CUST789|standard|SKU123,SKU456")?
```

**Problems:**
- 🙈: Can't express structure, just hoping strings align
- 🧶: Format knowledge scattered everywhere 
- 🌊: One format change breaks everything

### Stage 2: Stabilize Errors <🙈🧶💧>
```javascript
// First, contain the chaos
function processOrder(orderData) {
  try {
    const parts = orderData.split('|');
    if (parts.length < 3) {
      throw new Error('Invalid order format');
    }
    
    const [type, itemsStr, customer, ...rest] = parts;
    const items = itemsStr.split(',');
    
    // At least errors are caught
    return handleOrder({ type, items, customer, extra: rest });
  } catch (error) {
    console.error('Order processing failed:', error);
    return null;
  }
}
```

**Progress:**
- 🌊→💧: Errors now handled, not cascading
- Still 🙈🧶: Structure unclear, parsing everywhere

### Stage 3: Untangle Dependencies <🙈🪢💧>
```javascript
// Centralize parsing knowledge
class OrderParser {
  static parse(orderData) {
    const parts = orderData.split('|');
    if (parts.length < 3) {
      throw new Error('Invalid order format');
    }
    
    return {
      type: parts[0],
      items: parts[1].split(','),
      customer: parts[2],
      priority: parts[3] || 'normal'
    };
  }
}

// Now only one place knows the format
function processOrder(orderData) {
  try {
    const order = OrderParser.parse(orderData);
    return handleOrder(order);
  } catch (error) {
    console.error('Order parsing failed:', error);
    return null;
  }
}
```

**Progress:**
- 🧶→🪢: Parsing logic centralized
- Still 🙈: Can't validate at compile time

### Stage 4: Build Type System <👓🪢🧊>
```typescript
// Add compile-time structure
interface Order {
  type: 'standard' | 'rush' | 'subscription';
  items: string[];
  customer: string;
  priority?: 'low' | 'normal' | 'high';
}

class OrderParser {
  static parse(orderData: string): Order {
    const parts = orderData.split('|');
    if (parts.length < 3) {
      throw new Error('Invalid order format');
    }
    
    const type = parts[0];
    if (!['standard', 'rush', 'subscription'].includes(type)) {
      throw new Error(`Invalid order type: ${type}`);
    }
    
    return {
      type: type as Order['type'],
      items: parts[1].split(','),
      customer: parts[2],
      priority: (parts[3] || 'normal') as Order['priority']
    };
  }
}

// Validate at startup
const validators = {
  order: new OrderParser()
};
```

**Progress:**
- 🙈→👓: Types provide structure
- 💧→🧊: Validation at startup
- 🪢 maintained: Still linear parsing

### Stage 5: Achieve Crystalline <🔬🪢💠>
```typescript
// Type-safe with compile-time guarantees
import { z } from 'zod';

// Define schema once
const OrderSchema = z.object({
  type: z.enum(['standard', 'rush', 'subscription']),
  items: z.array(z.string().regex(/^SKU\d+$/)),
  customer: z.string().regex(/^CUST\d+$/),
  priority: z.enum(['low', 'normal', 'high']).default('normal')
});

type Order = z.infer<typeof OrderSchema>;

// Multiple valid input formats
const OrderParsers = {
  fromString: (data: string): Order => {
    const parts = data.split('|');
    return OrderSchema.parse({
      type: parts[0],
      items: parts[1].split(','),
      customer: parts[2],
      priority: parts[3]
    });
  },
  
  fromJSON: (data: unknown): Order => OrderSchema.parse(data),
  
  fromForm: (form: FormData): Order => OrderSchema.parse({
    type: form.get('type'),
    items: form.getAll('items'),
    customer: form.get('customer'),
    priority: form.get('priority')
  })
};

// Usage - compile-time safe
function processOrder(order: Order) {
  // TypeScript knows order.type is exactly one of three values
  switch (order.type) {
    case 'rush':
      return rushProcess(order);
    case 'subscription':
      return subscriptionProcess(order);
    case 'standard':
      return standardProcess(order);
    // No default needed - TypeScript ensures exhaustiveness
  }
}
```

**Final Achievement:**
- 🔬: Multiple valid expressions (string/JSON/form)
- 🪢: Clear transformation pipeline maintained
- 💠: Invalid states impossible at compile time

## Key Insights

### The Order Matters
1. **First stabilize errors** - You can't refactor if crashes are random
2. **Then untangle dependencies** - Create clear boundaries
3. **Finally build abstractions** - Types on top of clean structure

### Intermediate States Are Okay
- <🙈🧶💧> is better than <🙈🧶🌊>
- <👓🪢💧> is better than <🙈🧶💧>
- Progress > Perfection

### Tools That Help
- **Stage 2→3**: Extract method, centralize knowledge
- **Stage 3→4**: Add TypeScript gradually
- **Stage 4→5**: Schema libraries (Zod, io-ts, Yup)

### Signs You're Ready for Next Stage
- **For error stabilization**: Crashes are predictable
- **For untangling**: You know where all parsing happens
- **For types**: Structure is stable
- **For crystalline**: Team understands type benefits

## Common Pitfalls

### Trying to Skip Stages
```typescript
// DON'T: Jump from chaos to Zod
// The schema doesn't match reality yet
const OrderSchema = z.object({...});
// Breaks on first real order
```

### Over-Engineering
```typescript
// DON'T: Create 15 layers of abstraction
class OrderParserFactoryBuilderStrategy {...}
```

### Stopping Too Early
```javascript
// DON'T: Leave at "good enough"
// Those try-catches hide future pain
```

## Exercises

1. **Find Your Strings**: Where in your codebase is data stringly-typed?
2. **Trace the Parsing**: How many places parse the same format?
3. **Count the Catches**: How many try-catch blocks handle format errors?
4. **Imagine Types**: What would the ideal type look like?
5. **Plan the Journey**: Which stage should you tackle first?

## Related Transformations
- [[parse-dont-validate]] - Core principle
- [[config-evolution]] - Similar journey for configuration
- [[api-evolution]] - From strings to contracts

---

Remember: This transformation often takes weeks or months. Each stage provides value. The journey itself teaches the destination's worth.
