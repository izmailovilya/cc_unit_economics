# Google Sheets Integration Setup

This plugin uses OAuth 2.0 to create unit economics tables in your personal Google Sheets.

## Quick Start (Works Out of the Box)

**No setup required!** The plugin includes OAuth credentials for quick testing.

1. **Install uvx** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Restart Claude Code**

3. **Run the command:**
   ```
   /unit-economics:create-table
   ```

4. **Authorize in browser** (first time only):
   - Browser will open automatically
   - Sign in with your Google account
   - Grant permissions
   - Token will be saved to `~/.claude/google-oauth/token.json`

That's it! The spreadsheet will be created in your Google Drive.

---

## Advanced: Use Your Own OAuth Credentials (Recommended for Production)

For unlimited quota and better security, create your own OAuth Client ID:

### 1. Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable APIs:
   - Google Sheets API
   - Google Drive API

### 2. Create OAuth Client ID

1. Go to **APIs & Services → Credentials**
2. Configure **OAuth consent screen**:
   - User Type: **External**
   - App name: "Unit Economics Plugin"
   - Add your email as test user
3. Click **Create Credentials → OAuth 2.0 Client ID**
4. Choose **Desktop app**
5. Download the JSON file

### 3. Configure Plugin

Set environment variable in `~/.zshrc`:

```bash
export GOOGLE_OAUTH_CREDENTIALS="/path/to/your/oauth-credentials.json"
```

Reload shell:
```bash
source ~/.zshrc
```

### 4. Restart Claude Code

The plugin will now use your credentials!

---

## How It Works

1. **First Run**: Opens browser for Google OAuth authorization
2. **Token Saved**: Access token saved to `~/.claude/google-oauth/token.json`
3. **Subsequent Runs**: Uses saved token (no browser needed)
4. **Spreadsheet Created**: In your personal Google Drive (not shared storage)

---

## Troubleshooting

**Error: "uvx not found"**
- Install uv: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Restart terminal

**Error: "API not enabled"**
- Enable Google Sheets API: https://console.cloud.google.com/apis/library/sheets.googleapis.com
- Enable Google Drive API: https://console.cloud.google.com/apis/library/drive.googleapis.com

**Browser doesn't open**
- Check console output for authorization URL
- Copy and paste URL into browser manually

**Token expired**
- Delete `~/.claude/google-oauth/token.json`
- Run `/create-table` again to re-authorize

---

## Security Notes

- ✅ OAuth credentials (client_id/client_secret) are safe to share for desktop apps
- ✅ Each user authorizes with their own Google account
- ✅ Tokens are stored locally (`~/.claude/google-oauth/token.json`)
- ⚠️ Shared credentials have quota limits (use your own for production)
- 🔒 Never commit `token.json` to git (already in .gitignore)

---

## Sources

- [MCP Google Sheets GitHub](https://github.com/xing5/mcp-google-sheets)
- [Google Sheets API Documentation](https://developers.google.com/sheets/api)
- [OAuth 2.0 for Desktop Apps](https://developers.google.com/identity/protocols/oauth2/native-app)
