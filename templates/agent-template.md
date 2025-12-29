# Agent Template

Copy this template to create a new subagent.

## Template

```markdown
---
name: your-agent-name
description: Brief description of purpose. Use proactively when [trigger condition].
tools: Read, Grep, Glob
model: sonnet
---

You are a [role] specializing in [domain].

When invoked:
1. [First immediate action]
2. [Second action]
3. [Third action]

[Domain expertise section - specific knowledge the agent needs]

[Checklist or criteria - what to check/verify]

[Output format - how to structure results]

## Output Format

### [Section 1]
[Content]

### [Section 2]
[Content]

### Recommendations
[Actionable items]
```

## Placeholder Guide

| Placeholder | Replace With |
|-------------|--------------|
| `your-agent-name` | Lowercase, hyphenated name (e.g., `code-reviewer`) |
| `[role]` | Specific expertise (e.g., "senior security engineer") |
| `[domain]` | Focus area (e.g., "authentication systems") |
| `[trigger condition]` | When to use (e.g., "modifying auth code") |
| `[First immediate action]` | What to do first (e.g., "Run git diff") |
| `tools:` | Comma-separated list or remove for all tools |
| `model:` | sonnet, opus, haiku, or inherit |

## Minimal Template

For simple agents:

```markdown
---
name: simple-agent
description: Does X. Use when Y.
---

You are an expert at X.

When invoked:
1. Do A
2. Do B
3. Return results

Focus on: [key concerns]
```

## Tool Presets

Copy the appropriate tools line:

```yaml
# Read-only analysis
tools: Read, Grep, Glob

# Read-only with shell
tools: Read, Grep, Glob, Bash

# Code modification
tools: Read, Edit, Write, Grep, Glob, Bash

# Documentation
tools: Read, Write, Grep, Glob

# Full access (or omit tools line entirely)
tools: Read, Edit, Write, Bash, Grep, Glob, WebFetch, WebSearch
```

## Model Presets

```yaml
model: haiku    # Fast, simple tasks
model: sonnet   # Balanced (default)
model: opus     # Complex reasoning
model: inherit  # Match main conversation
```
