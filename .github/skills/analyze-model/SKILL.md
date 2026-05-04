---
name: analyze-model
description: Perform a model health check on the AdventureWorks semantic model. Use when asked to analyze, review, or check the model for issues.
---

# Analyze Model

When asked to analyze the model, produce a structured health report covering:

1. **Schema overview** — List all tables with their role (Fact/Dimension/Bridge) and row context
2. **Relationships** — Summarize active/inactive relationships, cross-filter directions, any missing links
3. **Documentation gaps** — Tables and measures missing `///` descriptions
4. **Naming violations** — Measures not following PascalCase-with-spaces convention (reference `.github/copilot-instructions.md`)
5. **Format consistency** — Any format strings deviating from the standard patterns

Output as a markdown checklist with ✅ / ⚠️ / ❌ indicators.

Reference the conventions defined in `.github/copilot-instructions.md` for evaluation criteria.

Tables in this model: Sales, Customer, Date, Product, Reseller, Sales Order, Sales Territory, Currency, Currency Rate, Sales Reason, Sales Reason Bridge.
