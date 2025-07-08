# Configuration Pattern

## Pattern: <👓🧶💧>

```yaml
# application.yml
server:
  port: ${SERVER_PORT:8080}
  host: ${SERVER_HOST:localhost}

database:
  url: ${DATABASE_URL}
  pool:
    size: ${DB_POOL_SIZE:10}
    timeout: ${DB_TIMEOUT:30}

features:
  new-ui: ${FEATURE_NEW_UI:false}
  analytics: ${FEATURE_ANALYTICS:true}
  
# Separate file: application-prod.yml
server:
  port: 443
  host: 0.0.0.0

# Oh wait, which takes precedence?
```

## VIBES Analysis

- **Expressive** (👓): One way - environment variables with defaults
- **Context** (🧶): Hidden coupling between files, load order, env vars
- **Error** (💧): Runtime - missing required vars fail at startup

## The Norman Door Experience

This is configuration's Norman door. It *looks* like it makes sense:
- Environment variables for flexibility ✓
- Defaults for convenience ✓  
- Override files for environments ✓

But developers constantly push wrong side:
- Forget which wins: env var or override file?
- Default in wrong place, production uses dev database
- Required var has default, fails in production only

## Visceral Moment

That sinking feeling when production crashes because `DATABASE_URL` wasn't set, but it worked locally because someone's `.env` file had it. The 3am page. The scramble through deployment scripts. The "but it worked in staging!"

## Anti-Pattern Version

```ini
# <🙈🌀🌊>
[server]
port = 8080  ; TODO: change for prod

[database]  
; IMPORTANT: Update these before deploying!!!
url = localhost:5432/dev_db
; url = prod-db.company.com:5432/prod_db  ; <-- UNCOMMENT FOR PROD

[features]
; Dave: I turned this on for testing, please turn off
new_ui = true
```

## Better Pattern

```typescript
// <🔍🪢🧊>
interface Config {
  server: { port: number; host: string };
  database: { url: string; pool: PoolConfig };
  features: FeatureFlags;
}

const config = loadConfig<Config>({
  required: ['database.url'],  // Fails fast if missing
  sources: [
    environmentVariables(),     // 1. Highest precedence
    fileConfig('config.json'),  // 2. Clear precedence order
    defaults()                  // 3. Explicit defaults
  ],
  validate: (c) => {
    if (c.server.port < 1024 && process.getuid() !== 0) {
      throw new Error('Privileged port requires root');
    }
  }
});
```

## Why This Resonates

Every developer has been burned by configuration. It's the Norman door of software - seems obvious until someone deploys development settings to production. The pattern above isn't toxic (👓🧶💧 is workable) but it's a daily friction point.

The better pattern makes precedence explicit, fails fast on missing required values, and validates semantic constraints. It transforms a Norman door into a well-designed entrance.
