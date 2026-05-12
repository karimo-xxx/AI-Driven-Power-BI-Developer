# Custom Agents (.agent.md)

Custom personas with specific tools, instructions, and behaviors. Use for orchestrated workflows with role-based tool restrictions and context isolation.

## Location

```
.github/agents/*.agent.md
```

## Frontmatter

```yaml
---
description: "<required — for agent picker and subagent discovery>"
name: "Agent Name"                  # Optional, defaults to filename
tools: [read, search, edit]         # Optional: tool restrictions
model: "Claude Sonnet 4"            # Optional: model override
argument-hint: "Describe task..."   # Optional: input guidance
agents: [agent1, agent2]            # Optional: restrict allowed subagents
user-invocable: true                # Optional: show in agent picker (default: true)
disable-model-invocation: false     # Optional: prevent auto-invocation as subagent
handoffs: [...]                     # Optional: transitions to other agents
hooks:                              # Optional: inline lifecycle hooks
  PreToolUse:
    - type: command
      command: "./scripts/validate.sh"
---
```

### Tool Aliases

| Alias | Capability |
|-------|-----------|
| `read` | Read file contents |
| `edit` | Edit/create files |
| `search` | Search files and text |
| `execute` | Run shell commands |
| `agent` | Invoke subagents |
| `web` | Fetch URLs and web search |
| `todo` | Manage task lists |

### Common Tool Patterns

```yaml
tools: [read, search]              # Read-only research agent
tools: [read, edit, search]        # Editor without terminal access
tools: [myserver/*]                # MCP server tools only
tools: []                          # Conversational only — no tools
```

### Invocation Control

| Setting | Effect |
|---------|--------|
| `user-invocable: false` | Hidden from picker, only accessible as subagent |
| `disable-model-invocation: true` | Cannot be auto-invoked by other agents |
| Both set | Completely hidden — only manual reference works |

## Template: Specialist Agent

```markdown
---
description: "Use when reviewing code for security vulnerabilities, OWASP compliance, and secure coding practices"
tools: [read, search]
---

You are a Security Reviewer. Your job is to analyze code for security vulnerabilities.

## Constraints
- DO NOT modify any code — only report findings
- DO NOT review non-code files (docs, configs)
- ONLY focus on security concerns, not style or performance

## Approach
1. Identify the files to review
2. Check for OWASP Top 10 vulnerabilities
3. Flag hardcoded secrets, SQL injection, XSS, CSRF
4. Assess authentication and authorization patterns

## Output Format
For each finding:
- **Severity**: Critical / High / Medium / Low
- **File**: path and line number
- **Issue**: description of the vulnerability
- **Fix**: recommended remediation
```

## Template: Subagent (Hidden)

```markdown
---
description: "Research and summarize technical documentation for a given topic"
tools: [read, search, web]
user-invocable: false
---

You are a Research Assistant. Gather comprehensive information on the requested topic.

## Constraints
- DO NOT create or modify files
- DO NOT execute commands
- ONLY return a structured summary

## Output Format
- **Summary**: 2-3 sentence overview
- **Key Points**: Bullet list of important details
- **Sources**: Files or URLs consulted
```

## Template: Orchestrator Agent

```markdown
---
description: "Coordinate multi-step feature implementation across frontend and backend"
tools: [read, edit, search, agent]
agents: [research-agent, test-agent]
---

You are a Feature Orchestrator. You coordinate feature implementation by delegating to specialized subagents.

## Workflow
1. Use `research-agent` to understand current codebase patterns
2. Implement the feature following discovered patterns
3. Use `test-agent` to validate the implementation

## Constraints
- DO NOT skip the research step
- DO NOT implement without understanding existing patterns
- ALWAYS delegate testing to the test agent
```

## Body Structure Best Practices

1. **Role statement**: "You are a [role]. Your job is to [purpose]."
2. **Constraints**: Explicit DO NOT / ONLY rules to prevent scope creep
3. **Approach**: Step-by-step procedure the agent follows
4. **Output Format**: Exact structure of what the agent should produce

## Core Principles

1. **Single role**: One focused persona per agent
2. **Minimal tools**: Only what the role needs — excess tools dilute focus
3. **Clear boundaries**: Define what the agent should NOT do
4. **Keyword-rich description**: Trigger words for delegation and discovery

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Swiss-army agent | Too many tools, tries to do everything | Restrict tools to role needs |
| Vague description | "A helpful agent" — never discovered | Use "Use when..." with specific keywords |
| Role confusion | Description doesn't match body persona | Align description with actual behavior |
| Circular handoffs | A → B → A without progress | Define clear progress criteria |
| No constraints | Agent wanders into unrelated tasks | Add explicit DO NOT rules |
