# VIBES Integration Guide

> "The best tool is the one that gets used." - Stewart Brand

## Overview

VIBES integration isn't about adding another linter or metric. It's about changing how your team sees and discusses code quality. This directory contains practical guides for weaving VIBES assessments into your existing workflow.

## Quick Start

**Minimum Viable Integration (5 minutes)**
```bash
# 1. Add to your PR template
echo "VIBES self-assessment: <___/___/___>" >> .github/pull_request_template.md

# 2. Start conversations
# "This feels like 🧶 - how could we make it more 🪢?"
```

That's it. Vocabulary changes perception.

## Integration Patterns

### 1. [Tool Integration](./tool-integration.md)
Comprehensive guide to IDE plugins, CI/CD pipelines, git hooks, and monitoring. Shows how to make VIBES assessment automatic and frictionless.

**Key insight**: Start with awareness, evolve to enforcement.

### 2. Build Your Own Tools
VIBES is open - create what serves your team:

```python
# Simple Python VIBES checker
def assess_vibes(code_file):
    """Quick heuristic assessment"""
    content = open(code_file).read()
    
    # Expressive axis
    if 'eval(' in content or 'exec(' in content:
        expressive = '🙈'  # String evaluation = noise
    elif content.count('def ') > 20:
        expressive = '🔍'  # Many functions = structured
    else:
        expressive = '👓'  # Default readable
    
    # Context axis  
    if 'global ' in content:
        context = '🌀'  # Global state = entangled
    elif 'import ' in content:
        context = '🪢'  # Imports = pipeline
    else:
        context = '🎀'  # No deps = independent
        
    # Error axis
    if 'try:' in content:
        errors = '💧'  # Runtime handling
    else:
        errors = '🌊'  # Unhandled cascading
        
    return f"<{expressive}{context}{errors}>"
```

### 3. Cultural Integration

**Morning Standup Addition**
"Yesterday I refactored the config from 🙈🧶 to 👓🪢. Today I'm working on the error handling to reach 🧊."

**Code Review Vocabulary**
- "This is getting tangled (🧶), could we untangle it?"
- "Nice transformation from 💧 to 🧊!"
- "What would it take to make this 🔬?"

**Retrospective Questions**
- What patterns keep appearing in our codebase?
- Which transformations gave us the most value?
- Where are we accepting poor VIBES unnecessarily?

## Integration Philosophy

### Start Where You Are
- Don't try to fix everything at once
- Use VIBES to describe, not prescribe
- Let understanding emerge through usage

### Progress Over Perfection
- <👓🧶💧> is better than <🙈🌀🌊>
- Celebrate incremental improvements
- Document your journey

### Context Matters
- Library code has different needs than application code
- Prototypes can have different standards than production
- Team agreement matters more than perfect ratings

## Success Stories

### Team A: Vocabulary Shift
"We started just using the emoji in PR comments. Within a month, 'let's untangle this' became our shorthand for dependency cleanup. Coupling-related bugs dropped 40%."

### Team B: Gradual Adoption
"We only assess new code. Our legacy system is still 🙈🌀🌊 but new modules are trending toward 🔍🪢🧊. It's motivating to see progress."

### Team C: Tool Building
"Our junior dev built a VS Code extension that shows VIBES ratings inline. Now everyone thinks about patterns while coding, not just during review."

## Common Concerns

**"This feels like busywork"**
Start with just vocabulary. No tools, no process. If it doesn't provide value in conversations, don't force it.

**"We disagree on ratings"**
Perfect! Disagreement reveals different mental models. Use the [consensus protocol](../validation/consensus-protocol.md) to explore why.

**"Our code is too unique"**
VIBES describes universal patterns. Even domain-specific code has dependencies, errors, and expressiveness.

## Next Steps

1. **Today**: Add VIBES notation to one PR comment
2. **This Week**: Assess one module with your team
3. **This Month**: Build or adopt one integration
4. **This Quarter**: Measure impact on code quality metrics

## Resources

- [Tool Integration](./tool-integration.md) - Detailed technical integration
- [Assessment Guide](../guides/assessment.md) - How to rate consistently
- [Consensus Protocol](../validation/consensus-protocol.md) - Handling disagreements
- [Examples](../examples/) - See VIBES in action

## Contributing

Have an integration story? Built a tool? Share it here:
1. Add your tool to `/integration/community/`
2. Share success stories in `/integration/case-studies/`
3. Propose new patterns via PR

---

Remember: VIBES is a lens, not a law. Use it to see more clearly, not to judge harshly. The goal is shared understanding that leads to better systems.

*"You can't change what you can't name."* - VIBES gives us names for the patterns we live with every day.
