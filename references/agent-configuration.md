# Agent Configuration Reference

## File Format

Subagents are Markdown files with YAML frontmatter:

```markdown
---
name: agent-name
description: When and why to use this agent
tools: tool1, tool2, tool3
model: sonnet
permissionMode: default
skills: skill1, skill2
---

System prompt content here...
```

## Required Fields

### name

Unique identifier for the agent.

**Requirements:**
- Lowercase letters only
- Hyphens for word separation
- No spaces or special characters
- Must be unique within scope (project or user level)

**Examples:**
```yaml
name: code-reviewer      # Good
name: test-runner        # Good
name: api-debugger       # Good
name: Code_Reviewer      # Bad - uppercase and underscore
name: my agent           # Bad - spaces
```

### description

Natural language description of the agent's purpose and when to use it.

**Purpose:**
- Tells Claude WHEN to automatically invoke the agent
- Appears in /agents listing
- Should be 1-2 sentences

**Trigger phrases for proactive use:**
- "Use proactively"
- "Use immediately after"
- "MUST BE USED when"
- "Invoke automatically"

**Examples:**
```yaml
# Good - clear trigger
description: Expert code reviewer. Use proactively after any code modifications.

# Good - specific use case
description: Database query optimizer. Use when analyzing SQL performance issues.

# Bad - no trigger indication
description: Helps with code.

# Bad - too vague
description: General purpose helper for various tasks.
```

## Optional Fields

### tools

Comma-separated list of tools the agent can access.

**Behavior:**
- If omitted: Agent inherits ALL tools from main thread (including MCP tools)
- If specified: Agent can ONLY use listed tools

**Format:**
```yaml
tools: Read, Grep, Glob, Bash
tools: Read, Edit, Write, Grep, Glob, Bash
```

**Recommendation:** Specify tools explicitly for security. Only grant what's needed.

### model

Which AI model the agent uses.

**Valid values:**
| Value | Description |
|-------|-------------|
| `sonnet` | Balanced performance (default if omitted) |
| `opus` | Most capable, complex reasoning |
| `haiku` | Fastest, simple tasks |
| `inherit` | Use same model as main conversation |

**Examples:**
```yaml
model: haiku    # Fast exploration agent
model: opus     # Complex architecture decisions
model: inherit  # Match parent conversation
```

### permissionMode

How the agent handles permission requests.

**Valid values:**
| Value | Description |
|-------|-------------|
| `default` | Normal permission handling |
| `acceptEdits` | Auto-accept file edits |
| `bypassPermissions` | Skip all permission prompts |
| `plan` | Planning mode (read-only research) |
| `ignore` | Ignore permission system |

**Default:** `default`

### skills

Skills to auto-load when agent starts.

**Important:** Subagents do NOT inherit skills from the parent conversation. You must explicitly list needed skills.

**Format:**
```yaml
skills: commit, review-pr
skills: ui-ux-pro-max
```

## System Prompt (Body)

Everything after the YAML frontmatter closing `---` is the system prompt.

**Structure recommendation:**
```markdown
---
[frontmatter]
---

You are a [role] specializing in [domain].

When invoked:
1. [First immediate action]
2. [Second action]
3. [Continue pattern]

[Domain expertise and guidance]

[Checklist or criteria]

[Output format specification]
```

## File Locations

### Project-Level Agents

**Location:** `.claude/agents/`

**Characteristics:**
- Available only in current project
- Highest priority (overrides user-level)
- Should be version controlled
- Team-shareable

### User-Level Agents

**Location:** `~/.claude/agents/`

**Characteristics:**
- Available across all projects
- Lower priority than project-level
- Personal productivity tools
- Not version controlled by default

## Priority Order

When agent names conflict:
1. CLI-defined agents (`--agents` flag) - session only
2. Project-level agents (`.claude/agents/`)
3. User-level agents (`~/.claude/agents/`)
4. Plugin agents
5. Built-in agents

## CLI-Based Configuration

Define agents via command line:

```bash
claude --agents '{
  "agent-name": {
    "description": "Description here",
    "prompt": "System prompt here",
    "tools": ["Read", "Grep"],
    "model": "sonnet"
  }
}'
```

Use for:
- Quick testing
- Session-specific agents
- Automation scripts
- Documentation examples
