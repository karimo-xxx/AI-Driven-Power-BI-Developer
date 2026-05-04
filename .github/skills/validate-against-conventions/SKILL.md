---
name: validate-against-conventions
description: Validate selected TMDL code against the project conventions in copilot-instructions.md. Use when asked to validate, check conventions, or verify compliance.
---

# Validate Against Conventions

When asked to validate TMDL code, check it against `.github/copilot-instructions.md`:

1. **Naming** — Does the measure/table/column follow PascalCase with spaces?
2. **Documentation** — Is a `///` description present?
3. **Format string** — Does it match the standard patterns (Currency/Percent/Integer)?
4. **Keys** — Are key columns hidden with `summarizeBy: none`?

Output a validation report:

```
✅ Format string: matches currency standard
❌ Description: missing /// documentation
⚠️ Naming: "Order Qty" uses abbreviation → suggest "Order Quantity"
```

If all checks pass, confirm compliance. If issues are found, list them with suggested fixes but do NOT auto-apply changes.
