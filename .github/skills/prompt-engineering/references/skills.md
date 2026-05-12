# Agent Skills (SKILL.md)

Folders of instructions, scripts, and resources that agents load on-demand for specialized tasks. Skills enable progressive loading — only consuming context when needed.

## Structure

```
.github/skills/<skill-name>/
├── SKILL.md           # Required entry point (name must match folder)
├── scripts/           # Executable code
├── references/        # Docs loaded as needed
└── assets/            # Templates, boilerplate
```

## Location

```
.github/skills/<name>/SKILL.md
```

## Frontmatter

```yaml
---
name: skill-name                    # Required: 1-64 chars, lowercase, alphanumeric + hyphens
description: 'What and when to use. Max 1024 chars.'
argument-hint: 'Optional hint for slash invocation'
user-invocable: true                # Optional: show as /slash command (default: true)
disable-model-invocation: false     # Optional: disable auto-loading by model
---
```

### Critical: Name Must Match Folder

```
.github/skills/my-skill/     ← folder name
└── SKILL.md                  ← name: my-skill  ✓
                              ← name: mySkill    ✗ WILL FAIL SILENTLY
```

## Template

```markdown
---
name: database-migrations
description: 'Create and manage database migrations safely. Use when adding tables, modifying columns, creating indexes, or handling data transformations. Covers rollback strategies and zero-downtime patterns.'
---

# Database Migration Management

## When to Use
- Adding or modifying database tables
- Creating or updating indexes
- Data transformations or backfills

## Procedure
1. Review current schema: `[schema](./references/current-schema.md)`
2. Generate migration from [template](./assets/migration-template.sql)
3. Run validation: `[validate script](./scripts/validate-migration.sh)`
4. Test rollback before committing

## Safety Rules
- Never drop columns in the same release as code changes
- Always include rollback/down migration
- Test with production-like data volumes
```

## Progressive Loading (How Copilot Uses Skills)

| Phase | What Loads | Token Budget |
|-------|-----------|-------------|
| **Discovery** | `name` + `description` | ~100 tokens |
| **Instructions** | Full SKILL.md body | <5000 tokens |
| **Resources** | Referenced files (scripts, references) | On demand |

This means:
- **Description** must be keyword-rich — it's the only thing Copilot sees initially
- **SKILL.md body** should be under 500 lines — use references for details
- **File references** should be one level deep from SKILL.md

## Slash Command Behavior

| Configuration | As `/` command | Auto-loaded by model |
|---|---|---|
| Default (both omitted) | Yes | Yes |
| `user-invocable: false` | No | Yes |
| `disable-model-invocation: true` | Yes | No |
| Both set | No | No |

## Referencing Resources

Use relative markdown links from SKILL.md:

```markdown
- Run the [setup script](./scripts/setup.sh)
- See [API reference](./references/api-docs.md)
- Use the [component template](./assets/component.tsx)
```

## Best Practices

1. **Keyword-rich descriptions**: Include all trigger words for discovery
2. **Progressive loading**: Keep SKILL.md body concise; put details in `references/`
3. **Relative paths**: Always use `./` for skill resources
4. **Self-contained**: Include all procedural knowledge to complete the task
5. **Actionable steps**: Numbered procedures, not vague guidance
6. **One skill = one workflow**: Don't bundle unrelated tasks

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Vague description | "A helpful skill" — never discovered | Use specific trigger keywords |
| Monolithic SKILL.md | Everything in one file | Split into `references/` |
| Name mismatch | Folder name ≠ `name` field — silent failure | Ensure exact match |
| Missing procedures | Description without step-by-step guidance | Add numbered procedures |
| Deep nesting | References reference other references | Keep one level deep |
