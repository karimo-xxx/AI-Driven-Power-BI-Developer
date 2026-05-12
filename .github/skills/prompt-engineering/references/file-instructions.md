# File Instructions (.instructions.md)

Guidelines loaded on-demand when relevant to the current task, or automatically when files match a pattern.

## Location

```
.github/instructions/*.instructions.md
```

## Frontmatter

```yaml
---
description: "<required — keyword-rich for on-demand discovery>"
name: "Optional Display Name"
applyTo: "**/*.ts"           # Optional — auto-attach for matching files
---
```

## Discovery Modes

| Mode | Config | When Loaded |
|------|--------|------------|
| **On-demand** | `description` only | Agent detects task relevance from description keywords |
| **Explicit** | `applyTo` pattern | Files matching glob are in context (create/edit, not read-only) |
| **Manual** | Neither | User attaches via `Add Context → Instructions` |

## Template: On-Demand (Task-Based)

```markdown
---
description: "Use when writing database migrations, schema changes, or data transformations. Covers safety checks, reversibility, and rollback patterns."
---

# Migration Guidelines

- Always create reversible migrations with up/down methods
- Test rollback before merging
- Never drop columns in the same release as code removal
- Include data backfill scripts when changing column types
```

## Template: Explicit (File-Based)

```markdown
---
description: "TypeScript coding standards for React components"
applyTo: "src/components/**/*.tsx"
---

# React Component Standards

- Use functional components with hooks
- Props interface named `{ComponentName}Props`
- One component per file
- Co-locate tests: `ComponentName.test.tsx`
```

## applyTo Patterns

```yaml
applyTo: "**/*.py"                    # All Python files
applyTo: "src/api/**/*.ts"            # Specific folder + extension
applyTo: ["src/**", "lib/**"]         # Multiple patterns (OR logic)
applyTo: "**"                         # ALWAYS included (use with extreme caution)
```

**Warning**: `applyTo: "**"` loads the instruction for EVERY file interaction, consuming context even when irrelevant.

## Best Practices

1. **"Use when..." descriptions**: Include specific trigger words for on-demand discovery
2. **One concern per file**: Don't mix testing, styling, and API design
3. **Show, don't tell**: Brief code examples over lengthy explanations
4. **Concise**: Instructions share the context window — keep focused
5. **Specific applyTo**: Target the narrowest glob that covers your use case

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Vague description | "Helpful coding tips" — never discovered | Use "Use when..." with specific keywords |
| `applyTo: "**"` | Always loaded, wastes context | Use specific file patterns |
| Mixing concerns | Testing + API + styling in one file | Split into separate instruction files |
| Duplicating docs | Copying README content | Link to existing docs |
| No description | Only `applyTo` — misses task-based discovery | Always include a meaningful description |
