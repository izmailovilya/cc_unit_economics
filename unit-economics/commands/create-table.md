---
description: Create Google Sheets table with unit economics analysis
allowed-tools:
  - mcp__*
  - Read
  - AskUserQuestion
---

# Create Unit Economics Table

Generate a professional Google Sheets spreadsheet by copying a pre-formatted template and filling it with user's data.

## Template

**Template ID**: `1yYgutbjITe1qR1A6FpX3BlxpIAeURN-MKzqrFryvgpQ`
**Template URL**: https://docs.google.com/spreadsheets/d/1yYgutbjITe1qR1A6FpX3BlxpIAeURN-MKzqrFryvgpQ

The template contains pre-formatted sheets with:
- Professional styling (colors, borders, merged headers)
- Two-level headers (Russian descriptions + English metric names)
- Column groups: Воронка продаж → Доход на платящего → На привлечённого → Финансовые метрики

## Your Task

### Step 0: Check MCP Setup

**Make a TEST CALL** to `list_spreadsheets` tool first:
- Server: `plugin:unit-economics:google-sheets`
- If error about credentials/OAuth → show setup instructions from `${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md`
- If success → proceed!

### Step 1: Gather Business Data

Ask the user:
1. **Company/project name** (for spreadsheet title)
2. **Do they have data?**
   - CSV/Excel file path
   - Or will input manually
   - Or use sample data
3. **Business model**: SaaS, e-commerce, marketplace, mobile app
4. **Key metrics** (if known): pricing, CAC, churn, conversion rates

### Step 2: Create Spreadsheet from Template

**Method: Copy sheets from template to new spreadsheet**

**IMPORTANT:** Use `create_spreadsheet` (NOT `create_sheet`!) to create a new file.

```
1. create_spreadsheet(title: "Unit Economics - {Company Name}")
   → returns spreadsheet_id (save it for next steps!)

2. list_sheets(template_id) → get all sheet names from template
   Template ID: 1yYgutbjITe1qR1A6FpX3BlxpIAeURN-MKzqrFryvgpQ

3. For each sheet in template:
   copy_sheet(
     src_spreadsheet: "1yYgutbjITe1qR1A6FpX3BlxpIAeURN-MKzqrFryvgpQ",
     src_sheet: "{sheet_name}",
     dst_spreadsheet: "{new_spreadsheet_id}",
     dst_sheet: "{sheet_name}"
   )

4. Delete the default "Sheet1" if it exists (optional)
```

**Important**: `copy_sheet` preserves ALL formatting from template:
- Colors, fonts, borders
- Merged cells
- Column widths
- Conditional formatting

### Step 3: Fill Data

Use `batch_update_cells` to populate the copied sheets with user's data:

```json
{
  "spreadsheet_id": "{new_spreadsheet_id}",
  "sheet": "{sheet_name}",
  "ranges": {
    "A5:A10": [["june 2025"], [""], [""], ["july 2025"], [""], [""]],
    "C5:C10": [[1203], [497], [706], [1413], [828], [585]]
  }
}
```

**Data row structure** (starting from row 5, after headers):
- Row for month total (e.g., "june 2025")
- Row for "paid" channel
- Row for "other" channel
- Repeat for each month

### Step 4: Add Formulas

**CRITICAL: Column structure in most sheets:**
```
Column A = Labels (text)
Column B = Values (numbers)
Column C = Units or formulas
Column D = Notes
```

**When writing formulas, reference Column B for values, NOT Column A!**

❌ WRONG: `=A4*Inputs!B7`  (A4 is text "Base generation")
✅ RIGHT: `=B4*Inputs!B7`  (B4 is the number 0.72)

**Key formulas** (reference glossary.md for accuracy):

| Cell | Formula | Description |
|------|---------|-------------|
| Conv. to regs | `=D5/C5` | Regs / Users (values in C, D) |
| C1 | `=H5/F5` | Buyers / Trials |
| AMPPU | `=L5*(1-O5)*P5` | Price × Margin × PaymentCount |
| CAC | `=R5/C1_cell` | CPUser / C1 |
| LTV | `=AMPPU/Churn` | From Inputs sheet |

**Before writing formulas:**
1. Use `get_sheet_data` to see actual column layout
2. Identify which column has VALUES (usually B or numeric columns)
3. Never reference label columns (A) in calculations

### Step 5: Return Results

Provide:
1. **Spreadsheet URL**: `https://docs.google.com/spreadsheets/d/{id}`
2. **What was created**: List of sheets copied
3. **How to use**: Which cells to edit (yellow = inputs)
4. **Key metrics** if data was provided: LTV/CAC ratio, payback period

---

## MCP Tools Reference

**IMPORTANT: Server name is `plugin:unit-economics:google-sheets` (NOT just `google-sheets`)**

When calling MCP tools, use the FULL server name.

| Tool | Use for | DO NOT CONFUSE |
|------|---------|----------------|
| `create_spreadsheet` | Create NEW spreadsheet file | ≠ create_sheet |
| `create_sheet` | Add tab to EXISTING spreadsheet | ≠ create_spreadsheet |
| `list_spreadsheets` | Test MCP connection | |
| `list_sheets` | Get sheet names from template | |
| `copy_sheet` | Copy formatted sheet from template | |
| `batch_update_cells` | Fill data into cells | |
| `get_sheet_data` | Read existing data | |
| `share_spreadsheet` | Share with user's email | |

**Common mistakes:**
- ❌ `google-sheets` → ✅ `plugin:unit-economics:google-sheets`
- ❌ `create_sheet` for new file → ✅ `create_spreadsheet` for new file

---

## Example Flow

**User**: "/create-table for my SaaS startup VideoAI"

**You**:
1. Test MCP: `list_spreadsheets` → success
2. Ask: "Do you have metrics data or should I use sample data?"
3. User: "Use sample data for now"
4. Create: `create_spreadsheet` title="Unit Economics - VideoAI"
5. List template sheets: `list_sheets` on template
6. Copy each sheet: `copy_sheet` from template to new
7. Fill sample data: `batch_update_cells`
8. Return: "Created! URL: https://... The yellow cells are for your inputs."

---

## Troubleshooting

**"Template not accessible"**
- Template must be shared as "Anyone with link can view"
- Or user's Google account must have access

**"copy_sheet failed"**
- Check template ID is correct
- Check sheet names match exactly (case-sensitive)

**Formatting not copied**
- `copy_sheet` DOES copy formatting
- If not working, check if sheets were created fresh vs copied

**MCP credentials error**
- See `${CLAUDE_PLUGIN_ROOT}/GOOGLE_SHEETS_SETUP.md`
