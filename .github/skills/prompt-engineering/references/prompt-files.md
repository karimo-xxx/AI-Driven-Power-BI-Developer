# Prompt Files (.prompt.md)

Reusable task templates triggered on-demand in chat. Each prompt represents a single focused task with optional parameterized inputs.

## Location

```
.github/prompts/*.prompt.md
```

## Frontmatter

```yaml
---
description: "<recommended — improves discoverability>"
name: "Display Name"             # Optional, defaults to filename
argument-hint: "describe task"   # Optional: hint shown in chat input
agent: "agent"                   # Optional: ask, agent, plan, or custom agent name
model: "GPT-5 (copilot)"        # Optional: model selection
tools: [search, web]             # Optional: tool restrictions
---
```

### Agent Routing

| Value | Behavior |
|-------|----------|
| `agent` | Full agent mode with tools |
| `ask` | Conversational, no tools |
| `plan` | Generates a plan before executing |
| `my-agent` | Routes to a custom agent by name |

### Model Fallback

```yaml
model: ['Claude Sonnet 4.5 (copilot)', 'GPT-5 (copilot)']  # First available is used
```

### Tool Priority

When both prompt and agent define tools: prompt tools win, then agent tools, then defaults.

## Template: Simple Task

```markdown
---
description: "Generate unit tests for selected code with edge cases and error scenarios"
agent: "agent"
---

Generate comprehensive test cases for the provided code:
- Include edge cases and error scenarios
- Follow existing test patterns in the codebase
- Use descriptive test names
- Mock external dependencies
```

## Template: With Context References

```markdown
---
description: "Create a README from project specifications"
agent: "agent"
tools: [read, search]
---

Generate a README.md based on:
- Project structure from [package.json](./package.json)
- Architecture notes from [docs/architecture.md](./docs/architecture.md)

Include: overview, installation, usage, API reference, contributing guidelines.
```

## Template: With Argument Hint

```markdown
---
description: "Explain a concept for different audience levels"
argument-hint: "concept to explain"
agent: "ask"
---

Explain the given concept at three levels:
1. **Beginner**: Simple analogy, no jargon
2. **Intermediate**: Technical but accessible
3. **Expert**: Full depth with edge cases
```

## Invocation Methods

1. **Chat**: Type `/` → select from prompt list
2. **Command Palette**: `Chat: Run Prompt...`
3. **Editor**: Open `.prompt.md` file → click play button

## Context References

- **Files**: `[config](./config.json)` — content loaded into context
- **Tools**: `#tool:search` — makes tool available
- **Instructions**: Link to `.instructions.md` files for reuse

## Best Practices

1. **Single task focus**: One prompt = one well-defined task
2. **Descriptive name**: Filename becomes the slash command (`/generate-tests`)
3. **Output examples**: Show expected format when structure matters
4. **Reuse instructions**: Reference `.instructions.md` files instead of duplicating rules
5. **Minimal tools**: Only include tools the task actually needs

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Multi-task prompt | "Create, test, and deploy" in one prompt | Split into separate prompts |
| Vague description | Users don't know when to use it | Include specific trigger phrases |
| Over-tooling | All tools when only search is needed | Restrict to minimum tools |
| No description | Prompt only works via manual invocation | Always add a description |
| Duplicated rules | Same rules as in instruction files | Reference the instruction file instead |
