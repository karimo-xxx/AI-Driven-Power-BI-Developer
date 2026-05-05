---
name: document-measure
description: Generate a description for a selected DAX measure based on its formula and usage in the model. Use when asked to document, describe, or add a description to a measure.
---

# Document Measure

## When to Use

- User asks to "document", "describe", or "add a description" to a measure
- User selects a measure and wants a `///` comment generated
- User asks what a measure does and wants it captured as documentation

## Procedure

When asked to document a measure, follow these steps:

1. **Read** the measure's DAX formula from the TMDL file
2. **Identify** which tables/columns it references and its business context
3. **Draft** a 1-2 sentence `///` description that explains:
   - What the measure calculates (business meaning, not DAX syntax)
   - When to use it (e.g. "Use for cumulative performance tracking")
4. **Present** the proposed description to the user for approval before editing

Format the output as a TMDL snippet showing the `///` line above the measure definition.

Example output:
```
/// Year-to-date sales based on the fiscal calendar. Use for cumulative performance tracking.
measure 'Sales YTD' = TOTALYTD([Total Sales], 'Date'[Date])
```

Do NOT edit files without user confirmation.
