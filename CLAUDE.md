# Using VIBES with Claude

This guide explains how to leverage VIBES assessments and transformations within Claude conversations, including special techniques that work well with Claude's capabilities.

## Quick Assessment Request

For rapid VIBES assessment, provide code and ask:

```
Please assess this code using VIBES ratings:
- Expressive Power (🙈→👓→🔍→🔬): How well does it afford valid expressions?
- Context Flow (🌀→🧶→🪢→🎀): Does it afford order-independent traversal?
- Error Surface (🌊→💧→🧊→💠): When does it afford error detection?

[paste code here]
```

## Detailed Assessment Protocol

For thorough analysis:

```
I'd like a VIBES assessment of this system. Please:

1. Rate each axis with rationale
2. Identify what actions the pattern affords/prevents for LLMs
3. Suggest the highest-impact improvements
4. Show what the code would look like after transformation

Consider both what this code affords to Claude and what it might fail to afford other LLMs.

[paste code here]
```

## Transformation Requests

### Single-Axis Improvement
```
This code is rated <🔍🧶💧>. Please show how to improve just the error surface from 💧 to 🧊 while keeping other axes unchanged.

Focus on making the pattern afford error detection at initialization rather than runtime.
```

### Full Transformation Journey
```
Please transform this code from its current VIBES rating to <🔍🎀🧊>.

Show:
1. Current state with rating
2. Each transformation step following the order (Errors → Dependencies → Model)
3. Final state with improved rating

Explain what new actions each transformation affords.
```

## Claude-Specific Techniques

### 1. Stance-Based Assessment

Imagine you are **Donald Norman at Apple, evaluating interfaces through the lens of cognitive affordances** - Sometimes viewing code through different expert perspectives reveals hidden affordances:

```
Please assess this API design from multiple perspectives:
- As a type theorist (focus on what expressions it affords)
- As a systems architect (focus on what flows it affords)
- As a reliability engineer (focus on what safety it affords)

How would each perspective rate the VIBES differently?
```

### 2. Comparative Analysis

```
Here are two implementations of the same functionality. Please:
1. Rate each using VIBES
2. Explain which affords better LLM interaction and why
3. Create a third version that affords the best of both

Implementation A: [code]
Implementation B: [code]
```

### 3. Domain-Specific Assessment

```
This is a [web API/CLI tool/config file/data pipeline]. 

Please assess its VIBES rating considering what domain-specific patterns typically afford. What affordances are standard for this domain, and how does this compare?
```

## Advanced Patterns

### Pattern Recognition Request
```
I'm seeing this error pattern repeatedly in our codebase:
[paste error logs or problematic code]

What VIBES anti-pattern does this represent? What actions does it fail to afford?
```

### Incremental Improvement Planning
```
We have a large codebase rated approximately <👓🌀💧>. We can only make incremental changes. 

Please suggest a 6-month improvement roadmap with:
- Month 1-2: Quick wins (which affordances to add first)
- Month 3-4: Medium complexity improvements
- Month 5-6: Deeper architectural changes

Goal is to afford at least <🔍🪢🧊> patterns.
```

### LLM Tool Design
```
We're building a tool specifically for LLM use. Current human-optimized version:
[paste current tool]

Please redesign following VIBES principles for LLM affordances. Consider what actions it should:
- Afford through semantic clarity
- Prevent through structural constraints  
- Enable through multiple valid paths
- Guide through visual domains
```

## Working with Claude's Capabilities

### 1. Extended Analysis
Claude can maintain context across long conversations, so you can:

```
Let's do a deep VIBES analysis of this system. First, let's assess what the current state affords, then explore multiple transformation paths.

[paste large codebase or system description]

We'll work through this systematically - please start with an overall assessment of affordances, then we'll dive into specific components.
```

### 2. Artifact Creation
For substantial transformations:

```
Please create an artifact showing the complete transformation of this code from <🙈🌀🌊> to <🔍🎀🧊>. 

Include:
- Before state (what it affords/prevents)
- Each intermediate transformation
- After state (new affordances)
- Summary of affordance changes

Make it a reference document I can share with my team.
```

### 3. Interactive Refinement
```
Here's my attempt at improving what the Context Flow affords:
[paste your attempt]

Did I correctly change it from 🧶 to 🪢? What affordances did I miss? Please refine my transformation.
```

## Common VIBES Conversations

### "Is this good enough?"
```
Our current system is rated <🔍🧶💧>. Given our constraints:
- Small team
- Legacy integration requirements  
- Performance critical

Do these affordances suffice or should we push for improvements? What's the minimum viable VIBES for our context?
```

### "Help me understand the rating"
```
You rated this as 🧶 for Context Flow, but I think it's 🪢. Can you show me specific hidden dependencies that prevent order-independent traversal?

Walk me through what the pattern actually affords vs what I think it affords.
```

### "Pattern Library Building"
```
We're building our own pattern library. Can you:
1. Assess what this pattern affords/prevents
2. Create a VIBES pattern file following the template
3. Suggest related patterns we should document

Pattern: [paste code pattern]
```

## Tips for Best Results

1. **Provide Context**: Tell Claude about your domain, constraints, and goals
2. **Focus on Affordances**: Ask what actions patterns enable/prevent
3. **Request Examples**: Ask for concrete code showing the affordance changes
4. **Iterate**: Use Claude's responses to refine your understanding
5. **Compare Options**: Have Claude show multiple approaches and trade-offs

## Meta-Assessment

You can even ask Claude to assess the VIBES framework itself:

```
How would you rate the VIBES framework documentation using VIBES itself?
- Expressive: Does it afford proper usage patterns?
- Context: Does it afford independent understanding?
- Error: When does it afford error detection?

What improvements would increase its affordances?
```

## Troubleshooting Common Issues

### Inconsistent Ratings
If Claude gives different ratings for the same code:
```
You previously rated this as <🔍🧶💧> but now say <👓🧶💧>. Can you:
1. Explain what changed in your affordance assessment
2. Identify the boundary between what 👓 and 🔍 afford
3. Give me concrete criteria to distinguish them
```

### Unclear Transformations
```
Your transformation seems to jump from 🌀 to 🎀 directly. Can you:
1. Show what intermediate states afford (🧶 and 🪢)
2. Explain why we can't skip affordance levels
3. Make each transformation more granular
```

### Domain Mismatches
```
This is embedded C code for a microcontroller. The standard VIBES examples seem web-focused. Can you:
1. Adjust the affordance criteria for embedded constraints
2. Show what good VIBES affords in this domain
3. Suggest embedded-specific patterns
```

## Contributing Back

When you discover new patterns or transformations with Claude:

```
We found this pattern in our codebase and transformed it successfully. Can you:
1. Document what it affords/prevents
2. Create a pattern file following the VIBES template
3. Identify what makes these affordances unique

This could help others facing similar challenges.
```

## Remember

- VIBES measures affordances, not comfort
- Different models may perceive different affordances
- The goal is better LLM affordances, not perfect scores
- Sometimes "sufficient affordances" is the right target

The framework evolves through use. Your conversations with Claude about what patterns afford help refine and expand the framework itself.

## Navigation

- [Back to Root](./README.md)
- [VIBES Axes](./VIBES-AXES.md)
- [Pattern Library](./corpus/patterns/)
- [Transformation Guides](./guides/)
