# Hooks (.json)

Deterministic lifecycle automation for agent sessions. Hooks enforce behavior via shell commands — unlike instructions which only guide behavior.

## Location

```
.github/hooks/*.json
```

## Hook Events

| Event | Trigger | Common Use |
|-------|---------|-----------|
| `SessionStart` | First prompt of a new session | Inject environment context |
| `UserPromptSubmit` | User submits a prompt | Validate or transform user input |
| `PreToolUse` | Before tool invocation | Block dangerous operations |
| `PostToolUse` | After successful tool invocation | Auto-format, auto-lint |
| `PreCompact` | Before context compaction | Preserve critical information |
| `SubagentStart` | Subagent starts | Log or restrict subagent usage |
| `SubagentStop` | Subagent ends | Validate subagent output |
| `Stop` | Agent session ends | Cleanup, reporting |

## Configuration Format

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "type": "command",
        "command": "./scripts/validate-tool.sh",
        "timeout": 15
      }
    ],
    "PostToolUse": [
      {
        "type": "command",
        "command": "./scripts/auto-format.sh"
      }
    ]
  }
}
```

## Hook Command Properties

| Property | Required | Description |
|----------|----------|-------------|
| `type` | Yes | Must be `"command"` |
| `command` | Yes | Default command to run |
| `windows` | No | Windows-specific command override |
| `linux` | No | Linux-specific command override |
| `osx` | No | macOS-specific command override |
| `cwd` | No | Working directory |
| `env` | No | Environment variables |
| `timeout` | No | Max execution time in seconds |

## Template: Block Dangerous Commands

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "type": "command",
        "command": "./scripts/block-dangerous.sh",
        "windows": "powershell -File ./scripts/block-dangerous.ps1",
        "timeout": 10
      }
    ]
  }
}
```

## Template: Auto-Format After Edits

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "type": "command",
        "command": "./scripts/auto-format.sh",
        "timeout": 30
      }
    ]
  }
}
```

## Input/Output Contract

Hooks receive JSON on **stdin** and return JSON on **stdout**.

### Common Input Fields (all events)

```json
{
  "timestamp": "2026-02-09T10:30:00.000Z",
  "cwd": "/path/to/workspace",
  "sessionId": "session-identifier",
  "hookEventName": "PreToolUse",
  "transcript_path": "/path/to/transcript.json"
}
```

### PreToolUse Input

> ⚠️ All field names use **snake_case** — not camelCase. Reading `toolName` or `toolInput` always yields `$null`.

```json
{
  "tool_name": "run_in_terminal",
  "tool_input": { "command": "Remove-Item file.tmdl" },
  "tool_use_id": "tool-123"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `tool_name` | string | Name of the tool being invoked |
| `tool_input` | object | Arguments passed to the tool |
| `tool_use_id` | string | Unique ID for this tool invocation |

**PowerShell example — correct snake_case access:**

```powershell
$toolName  = $inputJson.tool_name   # ✅ correct
$toolInput = $inputJson.tool_input  # ✅ correct
# ❌ WRONG: $inputJson.toolName / $inputJson.toolInput → always $null
```

### PreToolUse Output (Permission Control)

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "rm -rf is not allowed"
  }
}
```

| Decision | Effect |
|----------|--------|
| `allow` | Tool proceeds without user confirmation |
| `ask` | User is prompted for confirmation |
| `deny` | Tool invocation is blocked |

### Exit Codes

| Code | Effect |
|------|--------|
| `0` | Success — hook output is processed |
| `2` | Blocking error — stops the agent |
| Other | Non-blocking warning |

## Inline Hooks (In Custom Agents)

Hooks can also be defined inline in `.agent.md` frontmatter:

```yaml
---
description: "Secure code editor"
tools: [read, edit, search]
hooks:
  PreToolUse:
    - type: command
      command: "./scripts/validate.sh"
  PostToolUse:
    - type: command
      command: "./scripts/lint.sh"
---
```

These hooks are scoped to that agent only.

## Hooks vs Instructions

| Aspect | Instructions | Hooks |
|--------|-------------|-------|
| Behavior | Guidance (non-deterministic) | Enforcement (deterministic) |
| Implementation | Markdown text | Shell commands |
| Guarantee | Agent may not follow | Always executes |
| Use case | Coding style, conventions | Security, validation, formatting |

## Best Practices

1. **Keep hooks small and fast**: Long hooks block normal flow
2. **Validate inputs**: Sanitize stdin JSON before processing
3. **No hardcoded secrets**: Use environment variables
4. **Team hooks in workspace**: `.github/hooks/` for shared policy
5. **Personal hooks in user profile**: For individual preferences

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Long-running hooks | Blocks agent flow | Set timeout, keep scripts fast |
| Hooks for guidance | Over-engineering simple rules | Use instructions instead |
| Hardcoded secrets | Security risk | Use environment variables |
| No timeout | Script can hang indefinitely | Always set a timeout |
| camelCase field access (`toolName`, `toolInput`) | VS Code sends snake_case — variables are always `$null`, hook silently passes everything | Use `tool_name` and `tool_input` |
