---
name: refactor-measure-naming
description: Propose consistent renaming for measures that violate naming conventions. Use when asked to refactor, rename, or fix measure names.
---

# Refactor Measure Naming

When asked to refactor measure names, follow this workflow:

1. **Scan** all measures in the Sales table TMDL file
2. **Compare** each name against the conventions in `.github/copilot-instructions.md`:
   - PascalCase with spaces (no underscores)
   - No abbreviations (spell out Quantity, Average, Count)
   - Concise but descriptive
3. **Present a rename plan** as a table:

| Current Name | Proposed Name | Reason |
|---|---|---|
| `Sales_MTD` | `Sales MTD` | Underscore violates convention |
| `Order Qty` | `Order Quantity` | Abbreviation |
| `Avg Order Value` | `Average Order Value` | Abbreviation |

4. **Wait for user approval** before making any changes
5. If approved, rename the measure in the TMDL file and update any DAX references

NEVER rename measures without explicit user confirmation.
