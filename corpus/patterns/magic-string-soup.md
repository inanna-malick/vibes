# Pattern: Magic String Soup

## VIBES Rating
`<🙈🧶🌊>`

## Description
Systems where everything is strings - configuration, identifiers, commands, data. No types guide usage, dependencies hide in string matching, and typos become runtime explosions.

## Example
```javascript
// Everything is strings, nothing is safe
const app = {
  config: {},
  handlers: {},
  middleware: []
};

// Configuration loaded from environment strings
app.config.mode = process.env.APP_MODE || "development";
app.config.db = process.env.DATABASE || "mysql://localhost/app";
app.config.features = (process.env.FEATURES || "").split(",");

// String-based routing
function route(method, path, handler) {
  app.handlers[`${method}:${path}`] = handler;
}

route("GET", "/users/:id", (req, res) => {
  const userId = req.params.id;
  
  // String-based database queries
  db.query(`SELECT * FROM users WHERE id = '${userId}'`, (err, rows) => {
    if (err) {
      // String-based error handling
      if (err.message.includes("Connection refused")) {
        res.status(503).send("Database unavailable");
      } else if (err.message.includes("Syntax error")) {
        res.status(500).send("Query failed");
      } else {
        res.status(500).send("Unknown error");
      }
    } else {
      // String-based feature flags
      if (app.config.features.includes("new-user-format")) {
        res.json(transformUserNew(rows[0]));
      } else {
        res.json(transformUserLegacy(rows[0]));
      }
    }
  });
});

// String-based middleware system
function addMiddleware(name, fn) {
  app.middleware.push({ name, fn });
}

addMiddleware("auth", (req, res, next) => {
  // String parsing for auth
  const auth = req.headers.authorization || "";
  if (auth.startsWith("Bearer ")) {
    const token = auth.substring(7);
    // More string operations...
  }
});

// String-based command dispatch
function handleCommand(cmd) {
  const parts = cmd.split(" ");
  const command = parts[0];
  const args = parts.slice(1);
  
  switch(command) {
    case "user:create":
      createUser(args.join(" "));
      break;
    case "user:delete":
      deleteUser(args[0]);
      break;
    case "cache:clear":
      clearCache(args[0] || "all");
      break;
    default:
      console.error("Unknown command: " + command);
  }
}

// String-based event system
function emit(event, data) {
  const handlers = app.handlers[`event:${event}`] || [];
  handlers.forEach(h => h(data));
}

// Usage - spot the typos and SQL injection
route("GET", "/user/:id", handler);  // Typo: should be /users/:id
handleCommand("usr:create John Doe"); // Typo: should be user:create
emit("user-created", {id: 123});     // Will anyone listen to this exact string?
```

## Why This Rating

### Expressive Power: 🙈
- No guidance whatsoever - any string might be valid
- Typos are valid code until runtime
- No IDE support or autocomplete
- Zero type safety or contracts

### Context Flow: 🧶
- Dependencies hidden in string matching
- Event names must match exactly across files
- Configuration keys scattered everywhere
- Refactoring is grep-and-pray

### Error Surface: 🌊
- SQL injection vulnerabilities
- Typos cause runtime failures
- Missing configuration crashes later
- Errors cascade through string parsing

## Common Occurrence
- Old PHP applications
- Environment-based configuration
- Event systems without types
- Command parsers
- Dynamic language "flexibility"
- Anything with `eval()` or dynamic code

## Impact
- LLMs generate plausible but wrong strings
- No validation until runtime
- Security vulnerabilities everywhere
- Refactoring nearly impossible
- Testing requires extensive string mocking

## Related Patterns
- [typed-schema.md](./typed-schema.md) - The cure
- [global-state-mutation.md](./global-state-mutation.md) - Often combined
- [kubernetes-yaml.md](./kubernetes-yaml.md) - YAML is structured strings

## Consensus Data
- Model Agreement: 96%
- Disputed Aspects: None - universally recognized as problematic
- Test Date: July 2025
