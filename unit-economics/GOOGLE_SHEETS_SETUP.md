# Google Sheets Setup Guide

Set up Google Sheets integration to create unit economics tables directly in your Google Drive.

## Why Setup is Needed

The plugin needs permission to create and edit Google Sheets in your Drive. You'll create OAuth credentials (like an API key) that allow the plugin to work with your Google account.

---

## Setup Guide (~10 minutes)

### Step 1: Install uvx

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart your terminal after installation.

### Step 2: Create Google Cloud Project

1. Open: **[Create New Project](https://console.cloud.google.com/projectcreate)**
2. Project name: `unit-economics` (or anything you like)
3. Organization: leave as is (or select yours)
4. Click **"Create"**
5. Wait 10-20 seconds for project to be created

> **Tip**: If you already have a project, you can use it — just make sure to enable the APIs in Step 3.

### Step 3: Enable APIs

Enable both APIs (click each link and press "Enable"):

1. **[Enable Google Sheets API](https://console.cloud.google.com/apis/library/sheets.googleapis.com)** — click blue **"Enable"** button
2. **[Enable Google Drive API](https://console.cloud.google.com/apis/library/drive.googleapis.com)** — click blue **"Enable"** button

> **Note**: Make sure you're in the correct project (check top-left dropdown). If you just created a new project, it should be selected automatically.

### Step 4: Configure OAuth Consent Screen

Before creating credentials, you need to configure the consent screen (what users see when authorizing):

1. Open: **[OAuth Consent Screen](https://console.cloud.google.com/apis/credentials/consent)**
2. User Type: Select **"External"** → Click **"Create"**
   - (Choose "Internal" only if you have Google Workspace and want to restrict to your org)
3. Fill in the form:
   - **App name**: `Unit Economics Plugin` (or anything)
   - **User support email**: select your email from dropdown
   - **App logo**: skip (optional)
   - **App domain, Authorized domains**: skip (optional)
   - **Developer contact email**: enter your email
4. Click **"Save and Continue"**
5. **Scopes page**: Click **"Save and Continue"** (no changes needed)
6. **Test users page**: Click **"Add Users"** → enter your Gmail → **"Save and Continue"**
7. **Summary page**: Click **"Back to Dashboard"**

> **Important**: Your app will be in "Testing" mode. This is fine — it means only test users (you) can use it. No need to publish.

### Step 5: Create OAuth Credentials

1. Open: **[Create Credentials](https://console.cloud.google.com/apis/credentials)**
2. Click **"+ Create Credentials"** → **"OAuth client ID"**
3. Application type: **"Desktop app"**
4. Name: `Claude Code Plugin` (or anything)
5. Click **"Create"**
6. **Important!** Click **"Download JSON"** (or the download icon ⬇️)
7. Save the file — it will be named like `client_secret_XXXXX.json`

> **Security**: This JSON file is like a password. Keep it private, never commit to git.

### Step 6: Save Credentials File

```bash
# Create directory
mkdir -p ~/.claude/unit-economics

# Copy your downloaded file to the right location
cp ~/Downloads/client_secret_*.json ~/.claude/unit-economics/credentials.json
```

> **Verify**: `ls ~/.claude/unit-economics/credentials.json` should show the file.

### Step 7: Restart Claude Code

Close and reopen Claude Code (or restart terminal if using CLI).

### Step 8: Authorize Google Account

Run this command to start OAuth authorization:

```bash
CREDENTIALS_PATH=~/.claude/unit-economics/credentials.json \
TOKEN_PATH=~/.claude/unit-economics/token.json \
uvx mcp-google-sheets@latest
```

This will:
1. **Open browser** with Google sign-in page
2. **Sign in** with your Google account
3. **Click "Allow"** to grant permissions
4. **Token saved** to `~/.claude/unit-economics/token.json`

> **Note**: The terminal will show "Installed X packages" and wait. Once you complete authorization in browser, press `Ctrl+C` to stop.

### Step 9: Test It!

```shell
/create-table
```

Now MCP can access Google Sheets!

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
