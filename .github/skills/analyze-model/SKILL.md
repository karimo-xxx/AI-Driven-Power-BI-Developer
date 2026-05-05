---
name: analyze-model
description: Perform a model health check on the AdventureWorks semantic model. Use when asked to analyze, review, or check the model for issues.
---

# Analyze Model

## When to Use

- User asks to "analyze", "review", or "check" the semantic model
- User wants a health report or quality overview
- User asks about documentation gaps, naming issues, or relationship problems

## Procedure

When asked to analyze the model, produce a structured health report:

1. **Discover tables** — Scan all `.tmdl` files in `definition/tables/` to identify tables dynamically
2. **Schema overview** — List all tables with their role (Fact/Dimension/Bridge) and row context
3. **Relationships** — Summarize active/inactive relationships, cross-filter directions, any missing links
4. **Documentation gaps** — Tables and measures missing `///` descriptions
5. **Naming violations** — Measures not following PascalCase-with-spaces convention (reference `.github/copilot-instructions.md`)
6. **Format consistency** — Any format strings deviating from the standard patterns

## Output Format

Output as a markdown checklist with ✅ / ⚠️ / ❌ indicators.

Reference the conventions defined in `.github/copilot-instructions.md` for evaluation criteria.
