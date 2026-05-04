# Power BI Modeling

This workspace contains the AdventureWorks DW 2020 semantic model in TMDL/PBIP format.

## Naming Conventions

- Measure names use **PascalCase with spaces** (e.g. `Total Sales`, `Gross Profit Margin`)
- No underscores in measure names
- No abbreviations — spell out `Quantity`, `Average`, `Count` fully
- Table and column names use spaces, not camelCase

## Documentation

- Every measure MUST have a `///` description explaining its business purpose
- Every table SHOULD have a `///` description stating its role (fact, dimension, bridge)
- Descriptions are concise (1-2 sentences), written in English

## Format Standards

- Currency: `\$#,0.00;(\$#,0.00);\$#,0.00` (USD)
- Percentage: `0.00%;-0.00%;0.00%`
- Integer/Count: `#,0`
- Keys: `0` (hidden, summarizeBy: none)

## ⚠️ Intentional Inconsistencies

The following issues exist **by design** for workshop demos. Do NOT fix them automatically:

- `Sales_MTD` — underscore naming (should be `Sales MTD`)
- `Order Qty` — abbreviation (should be `Order Quantity`)
- `Avg Order Value` — abbreviation (should be `Average Order Value`)
- `Sum of Transactions` — verbose/non-standard naming
- Missing `///` descriptions on: `Order Qty`, `Gross Profit Margin`, `Sales_MTD`, `Sum of Transactions`, `Sales per Active Day`
- Missing table descriptions on all tables except `Date`
