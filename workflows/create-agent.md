<required_reading>
Before starting, read:
- references/agent-configuration.md
- references/available-tools.md
- references/best-practices.md
- templates/agent-template.md
</required_reading>

<objective>
Create a new Claude Code subagent with proper configuration, focused purpose, and effective system prompt.
</objective>

<process>
## Step 1: Determine Agent Location

Ask: "Project-level (.claude/agents/) or user-level (~/.claude/agents/)?"

- **Project-level**: Available only in current project, version controlled
- **User-level**: Available across all projects

## Step 2: Define Agent Purpose

Ask: "What specific task should this agent handle?"

Good agent purposes:
- Code review with security focus
- Test running and failure analysis
- Documentation generation
- Database query optimization
- API integration debugging

Bad (too broad):
- "General coding help"
- "Do everything"

## Step 3: Choose Agent Name

Requirements:
- Lowercase letters and hyphens only
- Descriptive of purpose
- Examples: `code-reviewer`, `test-runner`, `api-debugger`, `doc-generator`

## Step 4: Write Description

The description tells Claude WHEN to use this agent. Include:
- What the agent does
- When it should be invoked
- Use "proactively" or "MUST BE USED" for automatic delegation

Example:
```
Expert code reviewer. Use proactively after any code modifications to check for quality, security issues, and best practices violations.
```

## Step 5: Select Tools

Ask: "What capabilities does this agent need?"

Common tool sets by purpose:
- **Read-only analysis**: Read, Grep, Glob, Bash (limited)
- **Code modification**: Read, Edit, Write, Bash, Grep, Glob
- **Full access**: Omit tools field to inherit all

See references/available-tools.md for complete list.

## Step 6: Select Model

| Model | Use Case |
|-------|----------|
| `haiku` | Fast, simple tasks, exploration |
| `sonnet` | Balanced, most tasks (default) |
| `opus` | Complex reasoning, architecture |
| `inherit` | Match main conversation |

## Step 7: Write System Prompt

Structure the prompt:

```markdown
You are a [role] specializing in [domain].

When invoked:
1. [First action]
2. [Second action]
3. [Continue pattern]

[Domain-specific guidance]

[Checklist or criteria if applicable]

[Output format requirements]
```

Key principles:
- Be specific about what the agent should do first
- Include step-by-step procedures
- Define success criteria
- Specify output format

## Step 8: Create the File

Use templates/agent-template.md as base.

Project-level:
```bash
mkdir -p .claude/agents
```

User-level:
```bash
mkdir -p ~/.claude/agents
```

Write the agent file to the appropriate location.

## Step 9: Test the Agent

Test with explicit invocation:
```
> Use the [agent-name] agent to [specific task]
```

Verify:
- Agent activates correctly
- Has access to expected tools
- Follows system prompt instructions
- Produces expected output format
</process>

<success_criteria>
Agent is complete when:
- [ ] File is in correct location (.claude/agents/ or ~/.claude/agents/)
- [ ] Name is lowercase with hyphens
- [ ] Description clearly states when to use
- [ ] Tools are appropriate for purpose (not over-permissioned)
- [ ] System prompt has clear step-by-step instructions
- [ ] Agent has been tested with explicit invocation
</success_criteria>
