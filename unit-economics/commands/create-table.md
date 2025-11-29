---
description: Create Google Sheets table with unit economics analysis
allowed-tools:
  - mcp__google-sheets__*
  - Read
  - AskUserQuestion
  - Bash
---

# Create Unit Economics Table

Generate a professional Google Sheets spreadsheet with comprehensive unit economics analysis and calculations.

## IMPORTANT: Check MCP Setup First

Before proceeding, **check if Google Sheets MCP is available**:

1. Try to list available MCP tools by looking for `mcp__google-sheets__*` tools
2. If MCP tools are NOT available, provide setup instructions to the user:

```
Google Sheets MCP is not configured yet. Here's how to set it up:

1. Install uvx (if not installed):
   curl -LsSf https://astral.sh/uv/install.sh | sh

2. Follow the setup guide at:
   ${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md

   Or read it with:
   cat ${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md

3. Key steps:
   - Create Google Cloud project
   - Enable Google Sheets API + Drive API
   - Create Service Account
   - Download JSON key
   - Create Drive folder and share with service account
   - Set environment variables:
     export GOOGLE_SERVICE_ACCOUNT_PATH="/path/to/key.json"
     export GOOGLE_DRIVE_FOLDER_ID="folder_id"

4. Restart Claude Code to load MCP server

Would you like me to show the detailed setup instructions?
```

3. If user confirms they want detailed instructions, use Read tool to show `${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md`

## Your Task (if MCP is available)

1. **Gather business data** from the user:
   - Business model type (e-commerce, SaaS, marketplace, etc.)
   - Key metrics they want to track (CAC, LTV, AMPPU, conversion, retention, etc.)
   - Existing data source (CSV file path, or manual input)

2. **Design the spreadsheet structure**:
   - **Summary Dashboard** tab - key metrics, charts, KPIs
   - **Raw Data** tab - input data (users, conversions, revenue)
   - **Calculations** tab - formulas for CAC, LTV, AMPPU, payback period
   - **Cohort Analysis** tab - retention curves, cohort LTV
   - **Channel Analysis** tab - compare marketing channels effectiveness
   - **Funnel** tab - conversion funnel visualization

3. **Create the spreadsheet** using MCP Google Sheets tools:
   - Use `create_spreadsheet` to create new spreadsheet
   - Use `create_sheet` to add multiple tabs
   - Use `update_cells` or `batch_update_cells` to populate data
   - Use formatting tools for headers, colors, number formats

4. **Populate with formulas**:
   - CAC = Marketing Spend / New Customers
   - LTV = AMPPU / Churn Rate
   - AMPPU = Avg Price × Margin × Avg Payment Count
   - Payback Period = CAC / AMPU
   - Add data validation and conditional formatting

5. **Share the result**:
   - Use `share_spreadsheet` if needed
   - Provide the spreadsheet URL to the user
   - Explain the structure and how to use it

## Tips

- Use course materials from `${CLAUDE_PLUGIN_ROOT}/materials/glossary.md` for correct formulas
- If user has CSV data, read it first with Read tool
- Add helpful comments in cells to explain formulas
- Use professional formatting: freeze header rows, add borders, color-code sections
- Include examples or sample data if user starts with empty sheet

## Example Usage

User: "Create a unit economics table for my SaaS business"
You: Ask about their metrics → Read relevant materials → Create structured spreadsheet → Populate with formulas → Share URL
