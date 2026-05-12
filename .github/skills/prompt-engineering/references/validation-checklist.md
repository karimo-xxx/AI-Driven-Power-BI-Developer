# Validation Checklist

Use this checklist to validate any Copilot customization artifact before finalizing.

## Universal Checks (All Artifacts)

- [ ] **File location**: Is the file in the correct directory for its type?
- [ ] **YAML frontmatter**: Starts with `---` on line 1, ends with `---`?
- [ ] **No tabs**: Frontmatter uses spaces only (tabs cause silent failures)?
- [ ] **Description present**: Is `description` filled with keyword-rich trigger phrases?
- [ ] **"Use when..." pattern**: Does the description follow the recommended pattern?
- [ ] **Colons quoted**: Are descriptions containing `:` wrapped in quotes?
- [ ] **Single concern**: Does the file address one focused topic?
- [ ] **No duplication**: Are existing docs linked instead of copied?

## Agent Instructions (copilot-instructions.md / AGENTS.md)

- [ ] **Only one file**: Not using both `copilot-instructions.md` AND `AGENTS.md`?
- [ ] **Minimal content**: Only rules that apply to EVERY interaction?
- [ ] **No linter rules**: Nothing already enforced by tooling?
- [ ] **Links over content**: References to detailed docs instead of embedding?
- [ ] **Actionable**: Every line guides concrete behavior?

## File Instructions (.instructions.md)

- [ ] **Correct extension**: File ends with `.instructions.md`?
- [ ] **applyTo specificity**: Glob pattern is as narrow as possible (not `"**"` unless justified)?
- [ ] **Discovery mode clear**: Using description (on-demand), applyTo (explicit), or both?
- [ ] **Code examples**: Includes brief examples instead of just rules?

## Prompt Files (.prompt.md)

- [ ] **Correct extension**: File ends with `.prompt.md`?
- [ ] **Single task**: Prompt handles exactly one focused task?
- [ ] **Agent routing**: `agent` field set appropriately (agent, ask, plan, or custom)?
- [ ] **Tool restrictions**: Only necessary tools listed?
- [ ] **Output clarity**: Expected output format described or shown?

## Custom Agents (.agent.md)

- [ ] **Correct extension**: File ends with `.agent.md`?
- [ ] **Role statement**: Body starts with "You are a [role]. Your job is to..."?
- [ ] **Constraints**: Explicit DO NOT / ONLY rules present?
- [ ] **Minimal tools**: Only tools the role actually needs?
- [ ] **No circular handoffs**: Subagent chains don't loop?
- [ ] **Description matches body**: Description trigger words align with actual role?

## Skills (SKILL.md)

- [ ] **Name matches folder**: `name` field exactly matches containing folder name?
- [ ] **Name format**: Lowercase, alphanumeric + hyphens only, 1-64 characters?
- [ ] **Body under 500 lines**: Detailed content in `references/` subfolder?
- [ ] **Relative paths**: All file references use `./` prefix?
- [ ] **Procedures present**: Step-by-step numbered instructions, not just descriptions?
- [ ] **Self-contained**: All needed knowledge included or referenced?

## Hooks (.json)

- [ ] **Valid JSON**: File parses without errors?
- [ ] **type is "command"**: Every hook entry has `type: "command"`?
- [ ] **Timeout set**: All hooks have a timeout to prevent hanging?
- [ ] **Scripts exist**: Referenced command scripts actually exist?
- [ ] **Platform overrides**: Windows/Linux/macOS handled if needed?
- [ ] **No secrets**: No hardcoded credentials in commands or scripts?

## Post-Creation Testing

- [ ] **Invoke**: Test the artifact in Copilot Chat
- [ ] **Discovery**: For skills/instructions, verify they are auto-loaded when relevant
- [ ] **Behavior**: Confirm the agent/prompt/instruction produces expected results
- [ ] **No silent failures**: Check VS Code Output panel for frontmatter parsing errors
