# Event-Driven Architecture Pattern

## Pattern: <🔍🧶💧>

```javascript
class EventBus {
  constructor() {
    this.handlers = {};
  }
  
  on(event, handler) {
    if (!this.handlers[event]) this.handlers[event] = [];
    this.handlers[event].push(handler);
  }
  
  emit(event, data) {
    if (!this.handlers[event]) return;
    this.handlers[event].forEach(h => {
      try {
        h(data);
      } catch (e) {
        console.error(`Handler error for ${event}:`, e);
      }
    });
  }
}
```

## VIBES Analysis

- **Expressive** (🔍): Multiple ways to structure events, flexible handler patterns
- **Context** (🧶): Handlers can affect each other, hidden dependencies through shared state
- **Error** (💧): Runtime errors contained per handler, but effects cascade

## Why This Rating

Event systems provide flexibility in how components communicate, but create hidden coupling through event chains. Errors are caught at runtime but one handler's side effects can impact others unpredictably.

## Anti-Pattern Version

```javascript
// <🙈🌀🌊>
window.events = {};
function on(e, f) { 
  eval(`window.${e} = ${f}`);  // String-based event names
}
function emit(e, d) {
  window[e](d);  // No error handling, global pollution
}
```

## Transformation to Better

→ <🔍🪢🧊>: Use typed event system with clear dependencies
→ <🔬🎀💠>: Use reactive streams with type-safe composition
