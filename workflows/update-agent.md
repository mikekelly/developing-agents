<required_reading>
Before updating, read:
- references/agent-configuration.md
- references/available-tools.md
</required_reading>

<objective>
Modify an existing subagent's configuration, tools, or system prompt.
</objective>

<process>
## Step 1: Read Current Agent

Get the agent file path and read its contents to understand current configuration.

## Step 2: Identify Change Type

Ask: "What would you like to change?"

Common updates:
- Add/remove tools
- Change model
- Improve system prompt
- Update description
- Add skills

## Step 3: Make Changes

### Updating Tools

Current tools → New tools mapping:

To add tools:
```yaml
tools: Read, Grep, Glob  # before
tools: Read, Grep, Glob, Edit, Write  # after
```

To remove tools:
```yaml
tools: Read, Edit, Write, Bash  # before
tools: Read, Grep, Glob  # after (read-only)
```

To inherit all tools:
```yaml
# Remove the tools line entirely
```

### Updating Model

Valid values: `sonnet`, `opus`, `haiku`, `inherit`

```yaml
model: sonnet  # before
model: opus    # after (for more complex reasoning)
```

### Updating Description

Ensure description includes:
1. What the agent does
2. When to use it
3. Trigger words if needed ("proactively", "automatically")

### Updating System Prompt

When improving prompts:
- Add specific first action if missing
- Add step-by-step procedure if vague
- Add output format if not specified
- Add domain expertise for specialized agents

### Adding Skills

```yaml
skills: skill1, skill2
```

Note: Subagents don't inherit skills from parent conversation.

## Step 4: Validate Changes

Check:
- [ ] YAML syntax is valid
- [ ] Name hasn't changed (creates new agent otherwise)
- [ ] Tools are comma-separated
- [ ] Model is valid value
- [ ] System prompt is below the YAML frontmatter

## Step 5: Save and Test

Save the updated file.

Test with explicit invocation:
```
> Use the [agent-name] agent to [task matching new capabilities]
```

Verify:
- New tools are accessible
- Model change is reflected
- System prompt changes affect behavior
</process>

<success_criteria>
Update is complete when:
- [ ] Changes made to correct fields
- [ ] YAML syntax validated
- [ ] File saved to same location
- [ ] Agent tested with new configuration
- [ ] Behavior matches expected changes
</success_criteria>
