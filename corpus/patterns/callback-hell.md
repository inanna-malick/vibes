# Pattern: Callback Hell

## VIBES Rating
`<👓🌀💧>`

## Description
Deeply nested callbacks create a "pyramid of doom" where control flow becomes incomprehensible and error handling spreads across multiple scopes.

## Example
```javascript
// The pyramid of doom
getUser(userId, (err, user) => {
  if (err) {
    handleError(err);
  } else {
    getOrders(user.id, (err, orders) => {
      if (err) {
        handleError(err);
      } else {
        getOrderItems(orders[0].id, (err, items) => {
          if (err) {
            handleError(err);
          } else {
            calculateTotal(items, (err, total) => {
              if (err) {
                handleError(err);
              } else {
                applyDiscount(total, user.memberLevel, (err, finalTotal) => {
                  if (err) {
                    handleError(err);
                  } else {
                    renderInvoice(user, orders[0], items, finalTotal);
                  }
                });
              }
            });
          }
        });
      }
    });
  }
});
```

## Why This Rating

### Expressive Power: 👓
- Purpose is clear: async operations in sequence
- But only one way to express: nested callbacks
- No guidance toward better patterns
- Easy to write incorrectly (forget error handling)

### Context Flow: 🌀
- Circular between error handlers and success paths
- Each level depends on all outer levels
- Variable scope bleeds across boundaries
- Mental model requires tracking multiple contexts

### Error Surface: 💧
- Errors handled at runtime in each callback
- Easy to miss error cases
- Error handling logic duplicated
- No unified error strategy

## Common Occurrence
- Legacy Node.js applications
- Browser JavaScript before Promises
- Event-driven architectures
- Anywhere callbacks are the only async primitive

## Impact
- LLMs struggle with deep nesting
- Difficult to modify without breaking
- Error handling often incomplete
- Testing requires extensive mocking

## Related Patterns
- [promise-pipeline.md](./promise-pipeline.md) - The cure
- [global-state-mutation.md](./global-state-mutation.md) - Often found together
- [event-soup.md](./event-soup.md) - Similar complexity pattern

## Consensus Data
- Model Agreement: 93%
- Disputed Aspects: Some models rate 🧶 for context when callbacks share state
- Test Date: July 2025
