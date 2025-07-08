# Transformation: Callback Hell to Typed Async

## Journey: `<👓🌀💧>` → `<🔍🪢💠>`

This transformation shows how to evolve from callback chaos to type-safe async/await with compile-time guarantees.

## Starting Point: `<👓🌀💧>`

```javascript
// Nested callbacks with circular error handling
const OrderService = {
  processOrder(orderId, callback) {
    getOrder(orderId, (err, order) => {
      if (err) return callback(err);
      
      validateInventory(order.items, (err, available) => {
        if (err) return callback(err);
        
        if (!available) {
          updateOrderStatus(orderId, 'failed', (err) => {
            callback(err || new Error('Inventory unavailable'));
          });
          return;
        }
        
        chargePayment(order.payment, order.total, (err, charge) => {
          if (err) {
            // Circular: need to reverse inventory
            releaseInventory(order.items, (invErr) => {
              callback(err); // Original error, might lose invErr
            });
            return;
          }
          
          shipOrder(order, (err, tracking) => {
            if (err) {
              // More circular cleanup
              refundPayment(charge.id, (refundErr) => {
                releaseInventory(order.items, (invErr) => {
                  callback(err); // Error cascade
                });
              });
              return;
            }
            
            callback(null, { order, charge, tracking });
          });
        });
      });
    });
  }
};
```

**Why `<👓🌀💧>`:**
- 👓: Purpose clear (process order) but rigid callback structure
- 🌀: Circular dependencies in error handling paths
- 💧: All errors handled at runtime with duplication

## Step 1: Flatten with Promises `<👓🌀💧>` → `<👓🧶💧>`

First, escape callback hell by promisifying:

```javascript
// Promisified but still tangled
const OrderService = {
  async processOrder(orderId) {
    try {
      const order = await getOrder(orderId);
      const available = await validateInventory(order.items);
      
      if (!available) {
        await updateOrderStatus(orderId, 'failed');
        throw new Error('Inventory unavailable');
      }
      
      let charge;
      try {
        charge = await chargePayment(order.payment, order.total);
      } catch (paymentErr) {
        await releaseInventory(order.items);
        throw paymentErr;
      }
      
      let tracking;
      try {
        tracking = await shipOrder(order);
      } catch (shipErr) {
        // Still have cleanup tangle
        await Promise.all([
          refundPayment(charge.id),
          releaseInventory(order.items)
        ]);
        throw shipErr;
      }
      
      return { order, charge, tracking };
    } catch (err) {
      throw new OrderProcessingError(err.message, { orderId });
    }
  }
};
```

**Progress**: Callbacks eliminated, but error handling still creates complex paths (🧶).

## Step 2: Linearize with Result Types `<👓🧶💧>` → `<🔍🪢💧>`

Introduce Result types to make error handling linear:

```javascript
// Result type for explicit error handling
class Result {
  constructor(value, error = null) {
    this.value = value;
    this.error = error;
  }
  
  static ok(value) { return new Result(value); }
  static err(error) { return new Result(null, error); }
  
  isOk() { return this.error === null; }
  isErr() { return this.error !== null; }
  
  map(fn) {
    return this.isOk() ? Result.ok(fn(this.value)) : this;
  }
  
  flatMap(fn) {
    return this.isOk() ? fn(this.value) : this;
  }
}

// Linear flow with explicit error handling
const OrderService = {
  async processOrder(orderId) {
    const pipeline = await Result.ok(orderId)
      .flatMap(id => this.getOrderSafe(id))
      .flatMap(order => this.validateInventorySafe(order))
      .flatMap(order => this.chargePaymentSafe(order))
      .flatMap(({order, charge}) => this.shipOrderSafe(order, charge));
    
    if (pipeline.isErr()) {
      await this.handleError(pipeline.error);
      return pipeline;
    }
    
    return pipeline;
  },
  
  async getOrderSafe(orderId) {
    try {
      const order = await getOrder(orderId);
      return Result.ok(order);
    } catch (err) {
      return Result.err({ type: 'ORDER_NOT_FOUND', orderId, err });
    }
  },
  
  async validateInventorySafe(order) {
    const available = await validateInventory(order.items);
    if (!available) {
      await updateOrderStatus(order.id, 'failed');
      return Result.err({ type: 'INVENTORY_UNAVAILABLE', order });
    }
    return Result.ok(order);
  },
  
  async chargePaymentSafe(order) {
    try {
      const charge = await chargePayment(order.payment, order.total);
      return Result.ok({ order, charge });
    } catch (err) {
      await releaseInventory(order.items);
      return Result.err({ type: 'PAYMENT_FAILED', order, err });
    }
  },
  
  async shipOrderSafe(order, charge) {
    try {
      const tracking = await shipOrder(order);
      return Result.ok({ order, charge, tracking });
    } catch (err) {
      await Promise.all([
        refundPayment(charge.id),
        releaseInventory(order.items)
      ]);
      return Result.err({ type: 'SHIPPING_FAILED', order, charge, err });
    }
  }
};
```

**Progress**: Linear flow achieved (🪢) and conceptual model improved (🔍), but still runtime errors.

## Step 3: Add TypeScript Types `<🔍🪢💧>` → `<🔍🪢🧊>`

Type the system to catch errors earlier:

```typescript
// Domain types with validation
type OrderId = string & { readonly brand: unique symbol };
type OrderStatus = 'pending' | 'processing' | 'completed' | 'failed';

interface Order {
  id: OrderId;
  items: OrderItem[];
  payment: PaymentMethod;
  total: Money;
  status: OrderStatus;
}

interface OrderItem {
  sku: SKU;
  quantity: PositiveInteger;
  price: Money;
}

// Result types with discriminated unions
type OrderResult = 
  | { kind: 'success'; order: Order }
  | { kind: 'not_found'; orderId: OrderId }
  | { kind: 'invalid_state'; reason: string };

type InventoryResult =
  | { kind: 'available'; order: Order }
  | { kind: 'unavailable'; order: Order; items: OrderItem[] };

type PaymentResult =
  | { kind: 'charged'; order: Order; charge: Charge }
  | { kind: 'declined'; order: Order; reason: string }
  | { kind: 'error'; order: Order; error: Error };

// Service with compile-time type checking
class OrderService {
  constructor(
    private orderRepo: OrderRepository,
    private inventory: InventoryService,
    private payment: PaymentService,
    private shipping: ShippingService
  ) {
    // Dependencies validated at construction
    if (!orderRepo || !inventory || !payment || !shipping) {
      throw new Error('Missing required dependencies');
    }
  }
  
  async processOrder(orderId: OrderId): Promise<ProcessResult> {
    const orderResult = await this.orderRepo.findById(orderId);
    
    switch (orderResult.kind) {
      case 'not_found':
        return { kind: 'failed', reason: 'Order not found' };
      
      case 'invalid_state':
        return { kind: 'failed', reason: orderResult.reason };
      
      case 'success':
        return this.processValidOrder(orderResult.order);
    }
  }
  
  private async processValidOrder(order: Order): Promise<ProcessResult> {
    // Type system ensures order is valid here
    const inventoryResult = await this.inventory.validate(order);
    
    if (inventoryResult.kind === 'unavailable') {
      await this.orderRepo.updateStatus(order.id, 'failed');
      return { kind: 'failed', reason: 'Inventory unavailable' };
    }
    
    // Continue with typed pipeline...
  }
}
```

**Progress**: Errors caught at startup through dependency injection (🧊).

## Step 4: Functional Core with Type Safety `<🔍🪢🧊>` → `<🔍🪢💠>`

Move to fully type-safe functional pipeline:

```typescript
// Phantom types for compile-time guarantees
interface Validated { readonly _brand: unique symbol }
interface Charged { readonly _brand: unique symbol }
interface Shipped { readonly _brand: unique symbol }

type ValidatedOrder = Order & Validated;
type ChargedOrder = Order & Validated & Charged;
type ShippedOrder = Order & Validated & Charged & Shipped;

// Type-safe pipeline functions
const validateOrder = (order: Order): Result<ValidatedOrder, ValidationError> => {
  if (order.items.length === 0) {
    return err(new ValidationError('Order has no items'));
  }
  if (order.total.amount <= 0) {
    return err(new ValidationError('Invalid order total'));
  }
  return ok(order as ValidatedOrder);
};

const checkInventory = async (
  order: ValidatedOrder
): Promise<Result<ValidatedOrder, InventoryError>> => {
  const available = await inventoryService.checkAvailability(order.items);
  return available 
    ? ok(order)
    : err(new InventoryError('Items unavailable', order.items));
};

const chargePayment = async (
  order: ValidatedOrder
): Promise<Result<ChargedOrder, PaymentError>> => {
  const charge = await paymentService.charge(order.payment, order.total);
  return charge.success
    ? ok({ ...order, chargeId: charge.id } as ChargedOrder)
    : err(new PaymentError(charge.declineReason));
};

const shipOrder = async (
  order: ChargedOrder
): Promise<Result<ShippedOrder, ShippingError>> => {
  const tracking = await shippingService.ship(order);
  return ok({ ...order, trackingNumber: tracking } as ShippedOrder);
};

// Compose pipeline with type-safe stages
const processOrder = (orderId: OrderId) =>
  pipe(
    findOrder(orderId),
    chain(validateOrder),
    chainAsync(checkInventory),
    chainAsync(chargePayment),
    chainAsync(shipOrder),
    mapErr(handleError)
  );

// Usage - type system prevents calling functions out of order
// shipOrder(order); // Type error: Expected ChargedOrder, got Order
// chargePayment(shippedOrder); // Type error: Already shipped
```

## Final Result: `<🔍🪢💠>`

The transformation is complete:

- **🔍 (Conceptually Coherent)**: Clear functional pipeline model
- **🪢 (Pipeline)**: Perfect linear flow with type-safe stages
- **💠 (Crystal)**: Invalid states impossible at compile time

## Key Insights

1. **Incremental Progress**: Each step improved one axis while maintaining others
2. **Type-Driven Design**: Types guide the transformation naturally
3. **Functional Patterns**: Pipeline composition eliminates complex control flow
4. **Compile-Time Safety**: Phantom types make invalid operations impossible

## Alternative Path for Dynamic Languages

If using JavaScript instead of TypeScript, the path would be:
1. Promises to escape callback hell
2. Result objects for explicit error handling  
3. Validation functions at boundaries
4. Object freezing and contracts for safety

The end state would be `<🔍🪢🧊>` - as good as possible without compile-time types.

## Common Pitfalls

1. **Trying to add types too early** - First fix the control flow
2. **Over-abstracting** - Keep domain logic clear
3. **Ignoring error paths** - They need the same attention as happy paths
4. **All-or-nothing approach** - Each step should be shippable

## Navigation

- [Back to Transformations](./README.md)
- [Callback Hell Pattern](../patterns/callback-hell.md)
- [Promise Pipeline Pattern](../patterns/promise-pipeline.md)
- [Typed Schema Pattern](../patterns/typed-schema.md)
