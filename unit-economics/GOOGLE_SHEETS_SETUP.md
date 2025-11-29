# Google Sheets Setup Guide

Set up Google Sheets integration to create unit economics tables directly in your Google Drive.

## Why Setup is Needed

The plugin needs permission to create and edit Google Sheets in your Drive. You'll create OAuth credentials (like an API key) that allow the plugin to work with your Google account.

---

## Quick Setup (5 minutes)

### Step 1: Install uvx

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart your terminal after installation.

### Step 2: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **"Select a project"** → **"New Project"**
3. Name it anything (e.g., "Unit Economics Plugin")
4. Click **"Create"**

### Step 3: Enable APIs

1. In your new project, go to **"APIs & Services"** → **"Enable APIs and Services"**
2. Search for and enable:
   - **Google Sheets API**
   - **Google Drive API**

### Step 4: Create OAuth Credentials

1. Go to **"APIs & Services"** → **"Credentials"**
2. Click **"Configure OAuth Consent Screen"**
   - User Type: **External** (or Internal if you have Google Workspace)
   - Click **"Create"**
3. Fill in required fields:
   - App name: "Unit Economics Plugin" (or anything)
   - User support email: your email
   - Developer contact: your email
   - Click **"Save and Continue"** through all screens
4. Back to **"Credentials"** → **"Create Credentials"** → **"OAuth client ID"**
5. Application type: **"Desktop app"**
6. Name: "Claude Code Plugin"
7. Click **"Create"**
8. **Download JSON** file (click the download icon)

### Step 5: Save Credentials File

```bash
# Create directory
mkdir -p ~/.claude/unit-economics

# Copy your downloaded file to the right location
# Replace "YOUR_DOWNLOADED_FILE.json" with actual filename
cp ~/Downloads/client_secret_*.json ~/.claude/unit-economics/credentials.json
```

### Step 6: Restart Claude Code

Close and reopen Claude Code.

### Step 7: Test It!

```shell
/create-table
```

On first run:
- Browser will open automatically
- Sign in with your Google account
- Click **"Allow"** to grant permissions
- Token will be saved (no need to authorize again)

---

## How It Works

1. **OAuth credentials** (credentials.json) - Your API "key" to access Google
2. **First authorization** - Browser opens, you grant permissions
3. **Token saved** (token.json) - Saved locally for future use
4. **Create spreadsheets** - Plugin can now create sheets in your Drive!

---

## File Locations

- **Credentials**: `~/.claude/unit-economics/credentials.json` (you create this)
- **Token**: `~/.claude/unit-economics/token.json` (auto-generated on first use)

Both files are:
- ✅ Stored locally (not in git)
- ✅ Protected by .gitignore
- ✅ Only accessible to you

---

## Troubleshooting

**"uvx not found"**
```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Restart terminal
exec $SHELL
```

**"API not enabled"**
- Enable [Google Sheets API](https://console.cloud.google.com/apis/library/sheets.googleapis.com)
- Enable [Google Drive API](https://console.cloud.google.com/apis/library/drive.googleapis.com)

**"credentials.json not found"**
```bash
# Check if file exists
ls -la ~/.claude/unit-economics/credentials.json

# If not, make sure you copied it to the right place
cp ~/Downloads/client_secret_*.json ~/.claude/unit-economics/credentials.json
```

**"Invalid credentials" or "Unauthorized"**
- Delete old token: `rm ~/.claude/unit-economics/token.json`
- Run `/create-table` again
- Browser will open to re-authorize

**Browser doesn't open**
- Look for the authorization URL in the terminal
- Copy and paste it into your browser manually

---

## Alternative: Use Environment Variable

If you prefer to store credentials elsewhere:

```bash
# Add to ~/.zshrc or ~/.bashrc
export GOOGLE_OAUTH_CREDENTIALS="/path/to/your/credentials.json"
```

Then restart Claude Code.

---

## Security Notes

- ✅ **Safe to create**: OAuth credentials are designed for desktop apps
- ✅ **Your account only**: Each user authorizes with their own Google account
- ✅ **Stored locally**: credentials.json and token.json stay on your machine
- ✅ **Revocable**: You can revoke access anytime in Google Account settings
- ⚠️ **Don't share**: Keep your credentials.json private (like a password)
- 🔒 **Never commit**: Already protected by .gitignore

To revoke access:
1. Go to [Google Account Permissions](https://myaccount.google.com/permissions)
2. Find "Unit Economics Plugin" (or your app name)
3. Click **"Remove Access"**

---

## Why This Approach?

**Alternative approaches:**
- ❌ **Service Account** - More complex, requires sharing Drive folders
- ❌ **Shared credentials** - Security risk, quota limits
- ✅ **Your own OAuth** - Simple, secure, unlimited quota

**Benefits:**
- ✨ Simple 5-minute setup
- 🔒 Your data, your account
- ⚡ No quota limits
- 🎯 Works immediately after setup

---

## Next Steps

After setup, try these commands:

```shell
/create-table
```

Claude will ask about your business model and create a professional Google Sheets with:
- 📊 Unit economics dashboard
- 🧮 Automated CAC, LTV, AMPPU calculations
- 📈 Cohort analysis tables
- 🎯 Funnel visualization
- 💰 Channel comparison

The spreadsheet will appear in your Google Drive!

---

## Sources

- [MCP Google Sheets](https://github.com/xing5/mcp-google-sheets)
- [OAuth 2.0 for Desktop Apps](https://developers.google.com/identity/protocols/oauth2/native-app)
- [Google Sheets API](https://developers.google.com/sheets/api)
