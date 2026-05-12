# Agent Instructions (copilot-instructions.md / AGENTS.md)

Project-wide guidelines that automatically apply to ALL chat interactions. These are always loaded into context.

## Always-On Instruction Files

| File | Location | Best For |
|------|----------|----------|
| `copilot-instructions.md` | `.github/` | Standard projects, cross-editor compatibility |
| `AGENTS.md` | Root or subfolders | Multi-agent workflows, monorepos with subfolder-level rules |
| `CLAUDE.md` | Root, `.claude/`, or `~/` | Compatibility with Claude Code and other Claude-based tools |

These files **can coexist** — VS Code combines all instruction files into the chat context. Start with `copilot-instructions.md` for project-wide standards. Add `AGENTS.md` if you work with multiple AI agents or need subfolder-level instructions. Avoid duplicating the same rules across files.

## Template

```markdown
# Project Guidelines

## Code Style
- Use [language] with [framework]
- Follow [pattern] for [components]
- See [./docs/style-guide.md] for detailed examples

## Architecture
- [Component A] handles [responsibility]
- [Component B] communicates via [mechanism]
- See [./docs/architecture.md] for diagrams

## Build & Test
- Install: `npm install`
- Test: `npm test`
- Build: `npm run build`

## Conventions
- [Specific convention that differs from common practice]
- [Another project-specific rule]
```

## What to Include

- Language and framework preferences
- Architecture overview (components, boundaries)
- Build/test/deploy commands
- Naming conventions that differ from defaults
- Critical "gotchas" not documented elsewhere

## What NOT to Include

- Rules already enforced by linters (ESLint, Prettier, etc.)
- Entire README contents — link instead
- File-specific rules — use `.instructions.md` for those
- Temporary instructions — use prompt files instead

## Best Practices

1. **Keep it concise**: Every line should guide behavior — no filler
2. **Link, don't embed**: `See docs/TESTING.md for test conventions` instead of copying
3. **Update regularly**: Stale instructions cause more harm than none
4. **Test impact**: After creating, ask Copilot a relevant question and verify it follows the guidelines

## AGENTS.md Hierarchy (Monorepos)

```
root/
├── AGENTS.md              # Global rules
├── frontend/
│   └── AGENTS.md          # Frontend-specific rules (inherits global)
└── backend/
    └── AGENTS.md          # Backend-specific rules (inherits global)
```

Subfolder `AGENTS.md` files inherit from parent — only add what differs.

## Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Kitchen sink | Wastes context on irrelevant rules | Only what matters for EVERY task |
| Duplicating README | Double maintenance burden | Link to README sections |
| Duplicated rules | Same rules in `copilot-instructions.md` and `AGENTS.md` | Keep each rule in one file only |
| Obvious rules | "Use descriptive variable names" | Remove — linters handle this |
