<required_reading>
For deeper understanding, optionally read:
- references/best-practices.md
- references/agent-configuration.md
</required_reading>

<objective>
Explain how Claude Code subagents work, including the Task tool, context management, and delegation patterns.
</objective>

<process>
## Core Concepts

### What Are Subagents?

Subagents are specialized AI assistants with:
- **Separate context**: Own conversation history, doesn't pollute main thread
- **Custom configuration**: Specific tools, model, and system prompt
- **Focused expertise**: Optimized for particular task types

### How Delegation Works

1. User makes request in main conversation
2. Claude evaluates if a subagent matches the task
3. Claude invokes subagent via Task tool
4. Subagent works independently in its own context
5. Subagent returns results to main conversation
6. Claude presents results to user

### The Task Tool

The Task tool is Claude's mechanism for launching subagents.

**Required parameters:**
```json
{
  "description": "Short 3-5 word summary",
  "prompt": "Detailed instructions for the subagent",
  "subagent_type": "agent-name"
}
```

**Optional parameters:**
- `model`: Override the agent's default model
- `resume`: Continue a previous agent conversation
- `run_in_background`: Run asynchronously

### Automatic vs Explicit Invocation

**Automatic**: Claude reads agent descriptions and invokes when tasks match.

Triggers for automatic use:
- Description includes "use proactively"
- Task clearly matches agent's stated purpose
- Agent has tools needed for the task

**Explicit**: User requests specific agent:
```
> Use the code-reviewer agent to check my changes
```

### Context Management

Each subagent has:
- Fresh context window (starts clean)
- No access to main conversation history
- Own tool permissions
- Results returned as single message

Benefits:
- Main conversation stays focused
- Long explorations don't consume main context
- Specialized prompts improve quality

### Built-in Subagents

| Agent | Purpose | Model | Mode |
|-------|---------|-------|------|
| general-purpose | Complex multi-step tasks | Sonnet | Read/Write |
| Explore | Fast codebase search | Haiku | Read-only |
| Plan | Research during planning | Sonnet | Read-only |

### Resumable Agents

Agents can be resumed to continue previous work:

```json
{
  "resume": "agent-id-from-previous",
  "prompt": "Continue analyzing...",
  "subagent_type": "code-analyzer"
}
```

Use cases:
- Long-running analysis across sessions
- Iterative refinement
- Multi-step workflows with context

### Custom Agent Types

Create custom agents for:
- Domain-specific expertise (security, performance, accessibility)
- Project-specific workflows (your testing framework, your deploy process)
- Team standards (code style, documentation format)

### Best Practices Summary

1. **Single purpose**: One agent, one job
2. **Clear triggers**: Description says when to use
3. **Minimal tools**: Only what's needed
4. **Specific prompts**: Step-by-step instructions
5. **Test explicitly**: Verify before relying on automatic delegation
</process>

<common_questions>
## FAQ

**Q: When should I create a custom agent vs use built-in?**
A: Create custom when you need domain expertise, project-specific workflows, or team standards that built-ins don't provide.

**Q: Project vs user level agents?**
A: Project for team sharing and version control. User for personal productivity across projects.

**Q: Why isn't my agent being used automatically?**
A: Check description clarity. Add "use proactively" or make the trigger conditions clearer.

**Q: Can agents call other agents?**
A: No, subagents cannot spawn other subagents to prevent infinite nesting.

**Q: How do I debug agent behavior?**
A: Use explicit invocation and review the output. Check if tools and prompt match expectations.
</common_questions>

<success_criteria>
Understanding is complete when user can answer:
- [ ] What is the difference between main conversation and subagent context?
- [ ] How does the Task tool invoke subagents?
- [ ] When should I create a custom agent vs use built-in?
- [ ] What triggers automatic vs explicit invocation?
- [ ] How do I resume a previous agent conversation?
</success_criteria>
