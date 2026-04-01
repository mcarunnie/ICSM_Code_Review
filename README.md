# ICSM_Code_Review

AI-powered ABAP code review tool using Claude Code + SAP ADT MCP server.

## Prerequisites

- [Claude Code](https://claude.ai/code) installed and licensed
- Access to your SAP system (EWM/S4 with ADT enabled)
- `mcp-abap-adt` server installed locally (see [mcp-abap-adt](https://github.com/mcarunnie/mcp-abap-adt))

## Setup

1. Clone the repository:
   ```bash
   git clone <repo-url>
   cd ICSM_Code_Review
   ```

2. Create your `.claude.json` (not committed — contains credentials):
   ```json
   {
     "mcpServers": {
       "abap-adt": {
         "command": "node",
         "args": ["C:/Users/I348349/mcp-abap-adt/dist/index.js"],
         "env": {
           "SAP_URL": "http://<your-sap-host>:<port>",
           "SAP_USERNAME": "YOUR_USERNAME",
           "SAP_PASSWORD": "YOUR_PASSWORD",
           "SAP_CLIENT": "<your-client>"
         }
       }
     }
   }
   ```

3. Open Claude Code in this folder:
   ```bash
   claude
   ```

## Running a Code Review

Just tell Claude Code what to review:

```
Review class ZCL_MY_CLASS
Review report Z_MY_REPORT
Review function module Z_MY_FM in function group Z_MY_FG
Review include Z_MY_INCLUDE
```

Claude will:
1. Fetch the source code from SAP via ADT
2. Review it against all checks defined in `CLAUDE.md`
3. Return a structured report grouped by category

## Adding Custom Checks

Edit `CLAUDE.md` to add your ICSM-specific naming conventions or additional review rules.
