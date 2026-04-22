# AdventureWorks Workshop Sample

This is the hands-on semantic model for the **ALTYCA "AI-Driven Power BI Developer"** workshop. It demonstrates how to use an MCP server to analyze, refactor, and document Power BI semantic models stored in PBIP format (TMDL).

## Setup

### Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (latest version with PBIP support)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Git](https://git-scm.com/)

### Clone the repository

```bash
git clone <repo-url>
cd "AI Driven Power BI Developer"
```

### Open in VS Code

1. Open VS Code.
2. **File → Open Workspace from File…** and select `AdventureWorks.code-workspace`.
3. Install the recommended extensions when prompted.

### Required VS Code extensions

| Extension | Marketplace ID |
|---|---|
| TMDL Language Support | `analysis-services.tmdl` |
| Power BI Modeling MCP Server | `analysis-services.powerbi-modeling-mcp` |
| Git Graph | `mhutchie.git-graph` |

### Open in Power BI Desktop

1. Open `Adventure Works DW 2020.pbip` directly in Power BI Desktop.
2. You will be prompted to configure the SQL Server parameters — replace them with your own AdventureWorksDW instance, or use the provided Azure SQL endpoint if you have credentials.

## Workshop note

> **This model contains intentional inconsistencies for demo purposes. Do not "fix" them before the workshop.**
>
> During the workshop we will use the Power BI Modeling MCP Server to detect and resolve these issues as live demos.
