# Google Sheets Integration Setup

This plugin includes MCP integration for creating unit economics tables directly in Google Sheets.

## Prerequisites

- Google Cloud account
- Python with `uvx` installed (or `uv`)

## Setup Steps

### 1. Install uvx (if not already installed)

```bash
# Install uv (includes uvx)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or use existing)
3. Enable APIs:
   - Google Sheets API
   - Google Drive API

### 3. Set up Service Account (Recommended)

1. Go to **APIs & Services → Credentials**
2. Click **Create Credentials → Service Account**
3. Fill in details and click **Create**
4. Click on created service account
5. Go to **Keys** tab → **Add Key → Create new key**
6. Choose **JSON** format and download

### 4. Create Google Drive Folder

1. Create a folder in Google Drive where spreadsheets will be stored
2. Right-click folder → **Share**
3. Add service account email (found in JSON file as `client_email`)
4. Give **Editor** permissions
5. Copy folder ID from URL: `https://drive.google.com/drive/folders/{FOLDER_ID}`

### 5. Configure Environment Variables

Add to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
export GOOGLE_SERVICE_ACCOUNT_PATH="/path/to/your/service-account-key.json"
export GOOGLE_DRIVE_FOLDER_ID="your_folder_id_here"
```

Reload your shell:
```bash
source ~/.zshrc  # or source ~/.bashrc
```

### 6. Test the Integration

Restart Claude Code and try:

```shell
/create-table
```

## Alternative: OAuth 2.0 Setup

If you prefer user-based authentication:

1. In Google Cloud Console: **APIs & Services → Credentials**
2. **Create Credentials → OAuth 2.0 Client ID**
3. Choose **Desktop app**
4. Download credentials JSON
5. Set environment variable:
   ```bash
   export GOOGLE_CREDENTIALS_PATH="/path/to/oauth-credentials.json"
   ```

On first use, you'll authorize via browser and token will be saved.

## Available MCP Tools

The plugin can use these Google Sheets operations:

- `create_spreadsheet` - Create new spreadsheet
- `create_sheet` - Add tabs to spreadsheet
- `update_cells` - Write data to cells
- `batch_update_cells` - Bulk data updates
- `get_sheet_data` - Read spreadsheet data
- `list_spreadsheets` - List all spreadsheets in folder
- `share_spreadsheet` - Share with specific users

## Troubleshooting

**Error: "uvx not found"**
- Install uv: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Restart terminal

**Error: "Permission denied"**
- Check service account has Editor access to Drive folder
- Verify `GOOGLE_DRIVE_FOLDER_ID` is correct

**Error: "API not enabled"**
- Enable Google Sheets API and Google Drive API in Cloud Console

## Security Notes

- **Never commit** service account JSON to git
- Store credentials securely
- Use service accounts for automation
- Use OAuth for personal use
- Restrict service account permissions to specific folder only

## Sources

- [MCP Google Sheets GitHub](https://github.com/xing5/mcp-google-sheets)
- [Google Sheets API Documentation](https://developers.google.com/sheets/api)
