# VIBES Tool Integration Guide

## Overview

VIBES assessments work best when integrated into existing workflows. This guide shows how to add VIBES to your development process without disrupting flow.

## Integration Patterns

### 1. IDE Integration (VS Code Example)

```jsonc
// .vscode/settings.json
{
  "vibes.enableCodeLens": true,
  "vibes.showInlineHints": true,
  "vibes.assessmentTriggers": ["onSave", "onCommit"],
  
  // Custom snippets for common improvements
  "vibes.snippets": {
    "detangle": {
      "prefix": "!vibes-detangle",
      "body": [
        "// VIBES: Converting 🧶→🪢",
        "// Before: ${1:description}",
        "// After: Linear dependency flow"
      ]
    }
  }
}
```

**Code Lens Display:**
```typescript
// VIBES: <👓🧶💧> - Consider refactoring        [Quick Fix Available]
class UserService {
  constructor() {
    this.db = globalDB;  // 🧶 Hidden dependency
    this.cache = globalCache;
  }
}

// Quick Fix transforms to:
class UserService {
  constructor(db: Database, cache: Cache) {  // 🪢 Explicit dependencies
    this.db = db;
    this.cache = cache;
  }
}
```

### 2. CI/CD Integration

#### GitHub Actions
```yaml
name: VIBES Assessment

on: [push, pull_request]

jobs:
  vibes-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: VIBES Assessment
        uses: vibespec/assess-action@v1
        with:
          # Fail if any pattern rates below threshold
          min-expressive: '👓'  # At least Readable
          min-context: '🧶'     # At least Coupled
          min-error: '💧'       # At least Liquid
          
          # Paths to assess
          include: 'src/**/*.ts'
          exclude: 'src/**/*.test.ts'
          
      - name: Comment PR
        if: github.event_name == 'pull_request'
        uses: vibespec/comment-action@v1
        with:
          summary: true
          suggestions: true
```

#### Output Example
```
VIBES Assessment Summary:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ 15 patterns assessed
⚠️  3 patterns below threshold:

src/config/global.ts: <🙈🧶💧>
  Suggestion: Extract configuration to context
  
src/api/handlers.ts: <👓🌀💧>  
  Suggestion: Break circular dependencies
  
src/utils/parser.ts: <🙈🧶🌊>
  Suggestion: Add type safety to prevent cascading errors
```

### 3. Git Hooks

```bash
#!/bin/bash
# .git/hooks/pre-commit

# VIBES pre-commit check
echo "Running VIBES assessment..."

# Check staged files
STAGED_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep -E '\.(js|ts|py)$')

if [ -n "$STAGED_FILES" ]; then
  vibes assess $STAGED_FILES --format=simple
  
  if [ $? -ne 0 ]; then
    echo "❌ VIBES check failed. Patterns detected:"
    vibes assess $STAGED_FILES --format=detailed
    
    read -p "Commit anyway? (y/n) " -n 1 -r
    echo
    if [[ ! $REPLY =~ ^[Yy]$ ]]; then
      exit 1
    fi
  fi
fi
```

### 4. Code Review Integration

#### Pull Request Template
```markdown
## VIBES Self-Assessment

Before submitting, rate your changes:

**Expressive Power**: 
- [ ] 🙈 Noise - Can't express needed patterns
- [ ] 👓 Readable - Single rigid path  
- [ ] 🔍 Structured - Multiple valid approaches
- [ ] 🔬 Crystalline - Precise, flexible expressions

**Context Flow**:
- [ ] 🌀 Entangled - Circular dependencies
- [ ] 🧶 Coupled - Hidden connections
- [ ] 🪢 Pipeline - Linear flow
- [ ] 🎀 Independent - No dependencies

**Error Surface**:
- [ ] 🌊 Ocean - Errors cascade unpredictably
- [ ] 💧 Liquid - Runtime error handling
- [ ] 🧊 Ice - Startup validation
- [ ] 💠 Crystal - Compile-time safety

### Transformation Applied
If improving existing code, describe the transformation:
- **From**: `<___/_/_>`
- **To**: `<___/_/_>`
- **How**: [Describe the key change]
```

### 5. Monitoring & Metrics

```typescript
// vibes-metrics.ts
import { VIBESCollector } from '@vibespec/metrics';

// Track VIBES evolution over time
const collector = new VIBESCollector({
  repository: 'myapp',
  
  // Alert on regressions
  alerts: {
    regression: {
      threshold: -1,  // Any axis getting worse
      notify: ['#dev-alerts']
    },
    toxic: {
      pattern: '<🙈🌀🌊>',
      notify: ['@security-team']
    }
  }
});

// Dashboard query examples
const queries = {
  // Track improvement over time
  improvement: `
    SELECT date, 
           AVG(expressive_score) as expr,
           AVG(context_score) as ctx,
           AVG(error_score) as err
    FROM vibes_assessments
    GROUP BY date
    ORDER BY date
  `,
  
  // Find problem areas
  hotspots: `
    SELECT file, rating, COUNT(*) as occurrences
    FROM vibes_assessments
    WHERE rating < '👓🧶💧'
    GROUP BY file, rating
    ORDER BY occurrences DESC
  `
};
```

### 6. Editor Plugins

#### IntelliJ IDEA / WebStorm
```xml
<!-- .idea/inspectionProfiles/VIBES.xml -->
<profile version="1.0">
  <inspection_tool class="VIBESAssessment" enabled="true" level="WARNING">
    <option name="checkExpressive" value="true" />
    <option name="checkContext" value="true" />
    <option name="checkErrors" value="true" />
    
    <!-- Highlight anti-patterns -->
    <antipatterns>
      <pattern name="GlobalState" rating="🙈🌀🌊" />
      <pattern name="StringlyTyped" rating="🙈🧶💧" />
    </antipatterns>
  </inspection_tool>
</profile>
```

#### Vim/Neovim
```vim
" ~/.config/nvim/after/plugin/vibes.vim

" Show VIBES rating in statusline
set statusline+=%{VIBESRating()}

" Quick assessment
nnoremap <leader>va :VIBESAssess<CR>

" Navigate to next improvable pattern
nnoremap ]v :VIBESNext<CR>
nnoremap [v :VIBESPrev<CR>

" Apply suggested transformation
nnoremap <leader>vt :VIBESTransform<CR>
```

## Language-Specific Integration

### TypeScript/JavaScript
```json
{
  "extends": ["@vibespec/eslint-config"],
  "rules": {
    "@vibespec/no-global-state": "error",     // Prevents 🌀
    "@vibespec/explicit-deps": "warn",        // Prevents 🧶
    "@vibespec/parse-dont-validate": "error"  // Encourages 💠
  }
}
```

### Python
```toml
# pyproject.toml
[tool.vibes]
min_rating = "👓🧶💧"
exclude = ["tests/", "migrations/"]

[tool.vibes.transformations]
enabled = true
suggest_only = false  # Actually apply fixes

[tool.vibes.patterns]
# Custom patterns for your domain
dataclass_preferred = true
type_hints_required = true
```

### Rust
```toml
# Cargo.toml
[dev-dependencies]
vibes = "0.1"

# clippy.toml
vibes-integration = true
warn-on = ["🙈", "🌀", "🌊"]
deny-on = ["🙈🌀🌊"]  # Full toxic pattern
```

## Adoption Strategy

### Phase 1: Awareness (Weeks 1-2)
- Install IDE plugins
- Enable comments only (no blocking)
- Team discussion of patterns

### Phase 2: Measurement (Weeks 3-4)
- Enable metrics collection
- Track but don't enforce
- Identify hotspots

### Phase 3: Improvement (Weeks 5-8)
- Set minimum thresholds
- Enable pre-commit hooks
- Celebrate improvements

### Phase 4: Enforcement (Week 9+)
- Block PRs below threshold
- Require improvement for changes
- Track regression alerts

## Common Objections & Responses

**"This will slow us down"**
- Start with awareness only
- Quick fixes often < 5 minutes
- Prevents hours of debugging later

**"Our codebase is too messy"**
- Only assess new code initially
- Grandfather existing patterns
- Improve incrementally 

**"Too subjective"**
- Use consensus protocol
- Focus on clear anti-patterns first
- Adjust thresholds per team

## Success Metrics

Track these to show VIBES value:
- Bug reduction in improved code
- Time to debug issues
- Code review time
- Developer confidence scores
- Regression frequency

## Next Steps

1. Choose 1-2 integration points to start
2. Run assessment on small module
3. Discuss results with team
4. Iterate on thresholds
5. Expand gradually

Remember: VIBES is a vocabulary for discussion, not a hammer for judgment. Use it to build shared understanding of code quality.
