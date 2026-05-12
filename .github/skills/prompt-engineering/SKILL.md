---
name: prompt-engineering
description: "Best practices, templates, and validation checklists for creating VS Code Copilot customization artifacts. Use when creating or reviewing custom instructions, file instructions, prompt files, custom agents, agent skills, or hooks. Covers frontmatter syntax, description patterns, directory structure, anti-patterns, and progressive loading strategies."
---

# Prompt Engineering for VS Code Copilot Customization

This skill provides detailed templates, best practices, and checklists for all six Copilot customization primitives.

## Quick Decision Matrix

| Need | Primitive | Key Question |
|------|-----------|-------------|
| Project-wide standards | [Agent Instructions](./references/custom-instructions.md) | Should this apply to EVERY interaction? |
| File-type or topic rules | [File Instructions](./references/file-instructions.md) | Does this apply to specific files or tasks? |
| Reusable task template | [Prompt Files](./references/prompt-files.md) | Is this a single focused task with inputs? |
| Role-based persona | [Custom Agents](./references/custom-agents.md) | Does this need restricted tools or distinct identity? |
| Multi-step workflow | [Skills](./references/skills.md) | Does this need bundled scripts/templates/docs? |
| Deterministic enforcement | [Hooks](./references/hooks.md) | Must this be guaranteed, not just guided? |

## Universal Best Practices

### 1. Description Is Everything

The `description` field is how Copilot discovers and loads artifacts. Poor descriptions = never loaded.

**Pattern**: `"Use when [trigger condition]. Covers [scope]. Includes [key capabilities]."`

```yaml
# GOOD
description: "Use when writing Python unit tests. Covers pytest patterns, fixtures, mocking, and assertion styles."

# BAD
description: "Testing guidelines"
```

### 2. YAML Frontmatter Rules

- Delimiters: exactly `---` on their own line (first line of file)
- Indentation: spaces only, never tabs
- Strings with colons: must be quoted (`description: "Use when: X"`)
- Arrays: `[item1, item2]` or multi-line with `-`
- The `name` field in skills MUST match the folder name

### 3. File Placement

| Artifact | Workspace Path | User Path |
|----------|---------------|-----------|
| Agent Instructions | `.github/copilot-instructions.md` | — |
| File Instructions | `.github/instructions/*.instructions.md` | `<profile>/instructions/` |
| Prompt Files | `.github/prompts/*.prompt.md` | `<profile>/prompts/` |
| Custom Agents | `.github/agents/*.agent.md` | `<profile>/agents/` |
| Skills | `.github/skills/<name>/SKILL.md` | `~/.copilot/skills/<name>/` |
| Hooks | `.github/hooks/*.json` | — |

### 4. Scope Minimalism

- One concern per file
- Minimal tool sets for agents
- Specific `applyTo` patterns (not `"**"` unless truly universal)
- Link to docs instead of embedding them

## Creation Procedure

1. **Identify the need** → Use the Decision Matrix above
2. **Read the reference** → Load the appropriate [reference file](./references/) for templates
3. **Draft the artifact** → Follow the template, customize for the project
4. **Validate** → Run through the [validation checklist](./references/validation-checklist.md)
5. **Test** → Invoke the artifact in Copilot Chat and verify behavior
