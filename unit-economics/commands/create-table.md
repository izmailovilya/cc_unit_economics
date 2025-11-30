---
description: Create Google Sheets table with unit economics analysis
allowed-tools:
  - mcp__*
  - Read
  - AskUserQuestion
---

# Create Unit Economics Table

Generate a professional Google Sheets spreadsheet with comprehensive unit economics analysis and calculations.

## How It Works

When you run this command:

1. **First time**: Browser opens for Google OAuth authorization (one-time setup)
2. **MCP connects**: Uses `mcp-google-sheets` to access Google Sheets API
3. **Gathers info**: Asks about your business model and metrics
4. **Creates spreadsheet**: Multiple tabs with formulas and formatting
5. **Returns URL**: Link to your new spreadsheet in Google Drive

## Your Task

### Step 0: CRITICAL - Check MCP Setup (Do This FIRST!)

**Before doing anything else**, actively verify Google Sheets MCP is WORKING (not just visible):

1. **Make a TEST CALL**: Call `list_spreadsheets` tool (it's read-only and safe)
   - Look for tool containing `google-sheets` and `list_spreadsheets` in the name
   - Server name format: `plugin:unit-economics:google-sheets` (with colons)
   - Just seeing MCP tools in the list is NOT enough - they might fail without credentials!

2. **Analyze the result**:
   - **Success** (returns list, even empty) → MCP IS working, proceed!
   - **Error about credentials, OAuth, token, file not found** → MCP NOT configured

3. **If MCP is NOT configured**, tell the user:

```
⚠️ Google Sheets MCP is not set up yet!

To create spreadsheets, you need to configure Google Sheets integration (~10 minutes):

1. **Install uvx** (if not installed):
   curl -LsSf https://astral.sh/uv/install.sh | sh

2. **Follow setup guide**: Read detailed instructions at:
   ${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md

   Or run: cat ${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md

3. **Quick summary**:
   - Create Google Cloud project
   - Enable Google Sheets API + Drive API
   - Create OAuth Client ID (Desktop app)
   - Download credentials JSON
   - Save to: ~/.claude/unit-economics/credentials.json
   - Restart Claude Code

4. **Test**: Run /create-table again

Would you like me to show you the detailed setup instructions?
```

4. **If user says yes**, use Read tool to show the full GOOGLE_SHEETS_SETUP.md content

5. **Stop here** - do not attempt to create spreadsheet without working MCP

6. **If MCP IS working**, proceed with creating the spreadsheet!

---

### 1. Gather Business Data (Only if MCP is available)

Ask the user about:
- **Business model type**: e-commerce, SaaS, marketplace, mobile app, etc.
- **Key metrics**: CAC, LTV, AMPPU, conversion, retention, payback period
- **Pricing plans**: if applicable (Starter, Business, Enterprise)
- **Data source**: CSV file path, or will input manually

### 2. Design Spreadsheet Structure

Create these tabs using MCP tools:

**Tab 1: Inputs**
- All input parameters (pricing, COGS, CAC, churn rates)
- Color-coded for easy editing
- Notes column explaining each parameter

**Tab 2: Unit Economics**
- AMPPU calculation: `Price - COGS`
- CAC calculation: `CPUser / C1`
- LTV calculation: `AMPPU / Churn`
- LTV/CAC ratio
- Payback period
- Blended metrics (if multiple plans)

**Tab 3: Growth Projections**
- 12-month forecast
- New customers, MRR, COGS, Gross Profit
- Marketing spend, Net Profit
- Cumulative profit tracking

**Tab 4: Cohort Analysis**
- Retention table (M0-M12)
- Cohort MRR over time
- Key retention metrics with targets

### 3. Use MCP Google Sheets Tools

This plugin uses [xing5/mcp-google-sheets](https://github.com/xing5/mcp-google-sheets). Available tools:

**Reading:**
- `list_spreadsheets` - List accessible spreadsheets
- `list_sheets` - List all sheets (tabs) in a spreadsheet
- `get_sheet_data` - Read data from a range
- `get_sheet_formulas` - Read formulas from a range

**Creating:**
- `create_spreadsheet` - Create new spreadsheet (returns spreadsheet_id)
- `create_sheet` - Add new sheet (tab) to spreadsheet

**Writing:**
- `update_cells` - Write data to a specific range
- `batch_update_cells` - Update multiple ranges at once
- `add_rows` - Append rows to end of sheet
- `add_columns` - Add columns to a sheet

**Managing:**
- `share_spreadsheet` - Share with users/emails
- `copy_sheet` - Duplicate a sheet
- `rename_sheet` - Rename a sheet

**Note**: Server name is `plugin:unit-economics:google-sheets`. Tool naming varies by client — check your available tools list.

### 4. Populate with Correct Formulas

**IMPORTANT**: Use formulas from `${CLAUDE_PLUGIN_ROOT}/materials/glossary.md`:

```
CAC = Acquisition Costs / buyers
AMPPU = (Av.Price - COGS) × AvPaymentCount
LTV = AMPPU / Churn
C1 = buyers / users
Payback Period = CAC / AMPU
```

Reference cells from the Inputs tab (e.g., `=Inputs!B4*Inputs!B5`)

### 5. Apply Professional Formatting

- **Headers**: Bold, colored background, centered
- **Numbers**: Currency format for $, percentage for %
- **Conditional formatting**: Red for negative profit, green for positive
- **Freeze rows**: Top row on each tab
- **Column widths**: Auto-adjust or set manually
- **Borders**: Around tables for clarity

### 6. Return Results

Provide to the user:
- **Spreadsheet URL**: Direct link to open in browser
- **Brief explanation**: What each tab contains
- **How to use**: Which cells to edit (Inputs tab)
- **Key insights**: Current LTV/CAC ratio, payback period status

## Example Interaction

**User**: "Create a unit economics table for my SaaS business"

**You**:
1. Ask: "What pricing plans do you have? (e.g., Starter $49, Business $149)"
2. Ask: "What's your estimated CAC and monthly churn?"
3. Read `materials/glossary.md` for formula reference
4. Create spreadsheet with MCP tools
5. Populate Inputs tab with their data
6. Add formulas in Unit Economics tab
7. Return: "Here's your spreadsheet: [URL]. Key finding: LTV/CAC = 3.2 (healthy!)"

## Tips

- Start simple with Inputs + Unit Economics tabs, add others if requested
- Use glossary.md for accurate formulas (not assumptions!)
- Add comments in cells to explain complex formulas
- Include sample data if user doesn't have real numbers yet
- Color-code input cells vs calculated cells (blue vs white)

## Troubleshooting

**MCP tools visible but test call fails:**
- This means credentials.json is missing or invalid
- User needs to complete Google Cloud setup (see GOOGLE_SHEETS_SETUP.md)
- Check: `ls ~/.claude/unit-economics/credentials.json`

**If MCP tools not visible at all:**
- User needs to restart Claude Code after installing plugin
- Check `${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md` for setup guide

**If browser doesn't open for OAuth:**
- Check terminal output for authorization URL
- Copy URL and paste in browser manually
- Token will be saved after successful authorization

**Common error messages:**
- "credentials not found" → Need to download credentials.json from Google Cloud Console
- "invalid_grant" → Token expired, delete token.json and re-authorize
- "access_denied" → OAuth consent screen not configured properly
