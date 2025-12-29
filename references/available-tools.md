# Available Tools Reference

## Core Tools

These tools are always available in Claude Code:

### File Operations

| Tool | Description | Use Case |
|------|-------------|----------|
| `Read` | Read file contents | Viewing code, configs, docs |
| `Write` | Create/overwrite files | Creating new files |
| `Edit` | Modify existing files | Code changes, fixes |
| `Glob` | Find files by pattern | Locate files by name/path |
| `Grep` | Search file contents | Find code patterns |

### System Operations

| Tool | Description | Use Case |
|------|-------------|----------|
| `Bash` | Execute shell commands | Run tests, builds, git |
| `Task` | Launch subagents | Delegate to specialists |

### Information Tools

| Tool | Description | Use Case |
|------|-------------|----------|
| `WebFetch` | Fetch web content | Documentation, APIs |
| `WebSearch` | Search the web | Research, current info |
| `LSP` | Language server operations | Go to definition, references |

### Interaction Tools

| Tool | Description | Use Case |
|------|-------------|----------|
| `AskUserQuestion` | Ask user for input | Clarification, choices |
| `TodoWrite` | Manage task lists | Progress tracking |

### Notebook Tools

| Tool | Description | Use Case |
|------|-------------|----------|
| `NotebookEdit` | Edit Jupyter notebooks | Data science workflows |

## Tool Sets by Purpose

### Read-Only Analysis

For agents that should never modify files:

```yaml
tools: Read, Grep, Glob
```

Extended read-only with limited bash:
```yaml
tools: Read, Grep, Glob, Bash
```

Note: Bash in read-only agents should only run commands like:
- `ls`, `find`, `cat`, `head`, `tail`
- `git status`, `git log`, `git diff`
- `npm list`, `pip list`

### Code Modification

For agents that need to change code:

```yaml
tools: Read, Edit, Write, Grep, Glob, Bash
```

### Full Exploration

For agents that research and navigate:

```yaml
tools: Read, Grep, Glob, Bash, WebFetch, WebSearch
```

### Minimal Set

For highly constrained agents:

```yaml
tools: Read, Grep
```

## MCP Tools

Agents can access MCP (Model Context Protocol) tools from configured servers.

**Behavior:**
- If `tools` field is omitted: Agent inherits all MCP tools
- If `tools` field is specified: Must explicitly list MCP tools needed

**Format for MCP tools:**
```yaml
tools: Read, Grep, mcp-server-name:tool-name
```

## Tool Selection Guidelines

### Principle of Least Privilege

Grant only tools needed for the agent's purpose:

| Agent Type | Recommended Tools |
|------------|-------------------|
| Code reviewer | Read, Grep, Glob, Bash (read commands) |
| Test runner | Read, Bash, Grep, Glob |
| Bug fixer | Read, Edit, Grep, Glob, Bash |
| Documentation | Read, Write, Grep, Glob |
| Explorer | Read, Grep, Glob |

### Security Considerations

**High-risk tools:**
- `Bash` - Can execute arbitrary commands
- `Write` - Can create files anywhere
- `Edit` - Can modify any file

**Mitigation:**
- Limit Bash to read-only commands in prompts
- Specify exact tools needed
- Use `permissionMode` for additional control

### Inheritance vs Explicit

**Inherit all (omit tools field):**
- Pros: Agent has full capabilities, includes MCP
- Cons: May have more power than needed

**Explicit list:**
- Pros: Clear boundaries, more secure
- Cons: Must update if new tools needed

**Recommendation:** Use explicit lists for production agents, inherit for testing.

## Built-in Agent Tool Sets

### Explore Agent (Haiku)

```yaml
tools: Read, Grep, Glob, Bash
```

Bash limited to: ls, git status, git log, git diff, find, cat, head, tail

### General-Purpose Agent (Sonnet)

```yaml
# Inherits all tools
```

### Plan Agent (Sonnet)

```yaml
tools: Read, Glob, Grep, Bash
```

Read-only research mode.
