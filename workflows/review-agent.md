<required_reading>
Before reviewing, read:
- references/agent-configuration.md
- references/best-practices.md
</required_reading>

<objective>
Audit an existing subagent configuration against best practices and identify improvements.
</objective>

<process>
## Step 1: Read the Agent File

Get the agent file path and read its contents.

## Step 2: Validate YAML Frontmatter

Check required fields:
- [ ] `name`: Present, lowercase, hyphens only
- [ ] `description`: Present, describes when to use

Check optional fields if present:
- [ ] `tools`: Valid comma-separated list
- [ ] `model`: Valid value (sonnet, opus, haiku, inherit)
- [ ] `permissionMode`: Valid value if present
- [ ] `skills`: Valid comma-separated list if present

## Step 3: Evaluate Description Quality

Good descriptions:
- State what the agent does
- State when Claude should use it
- Use action words ("Use proactively", "Invoke when")

Flag if description:
- Is vague ("helps with code")
- Doesn't indicate when to use
- Is too long (should be 1-2 sentences)

## Step 4: Assess Tool Selection

Check if tools match purpose:
- **Over-permissioned**: Has Write/Edit but only analyzes
- **Under-permissioned**: Missing tools needed for stated purpose
- **Optimal**: Has exactly what's needed

Common issues:
- Read-only agents with Edit/Write tools
- Analysis agents with Bash execution
- Missing Grep/Glob for search-focused agents

## Step 5: Review System Prompt

Quality checklist:
- [ ] Clear role definition
- [ ] Specific first action (what to do immediately)
- [ ] Step-by-step procedure
- [ ] Domain-specific guidance
- [ ] Output format specification
- [ ] Success criteria or checklist

Common issues:
- No clear starting action
- Too vague ("be helpful")
- Missing output format
- No domain expertise included

## Step 6: Check Model Selection

Evaluate model choice:
- `haiku` for simple/fast tasks
- `sonnet` for balanced tasks
- `opus` for complex reasoning
- `inherit` for consistency with main conversation

Flag if:
- Complex task using haiku
- Simple task using opus (wasteful)
- No model specified when inherit would be better

## Step 7: Generate Report

Structure findings:

```markdown
## Agent Review: [name]

### Summary
[Overall assessment]

### Issues Found
1. [Critical issues - must fix]
2. [Warnings - should fix]
3. [Suggestions - nice to have]

### Recommended Changes
[Specific changes with examples]

### Strengths
[What's working well]
```
</process>

<success_criteria>
Review is complete when:
- [ ] All YAML fields validated
- [ ] Description quality assessed
- [ ] Tool selection evaluated
- [ ] System prompt reviewed for completeness
- [ ] Model selection checked
- [ ] Report generated with actionable recommendations
</success_criteria>
