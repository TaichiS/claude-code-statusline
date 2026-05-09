# Claude Code Statusline

Personal backup of Claude Code custom statusline script.

## Credits

**Original script:** [claude-statusline](https://github.com/nilbuild/claude-statusline) by [Kamran Ahmed](https://github.com/kamranahmedse) (MIT License).

This repository is a fork with added Windows compatibility (Git Bash/MSYS2), Linux Secret Service (`secret-tool`) support, and documentation improvements. The original does not include Windows support.

## Features

- ✅ Context usage tracking with visual progress bar
- ✅ Git branch and dirty status display
- ✅ Session duration tracking
- ✅ Thinking mode indicator
- ✅ API rate limit tracking (5h / 7d / extra usage)
- ✅ OAuth token auto-resolution (macOS Keychain / Linux Secret Service)
- ✅ Cross-platform caching (60s cache TTL)

## Requirements

- Bash shell (including Git Bash/MSYS2 on Windows)
- `jq` - JSON processor
- `git` - for branch/status detection
- `curl` - for API requests
- macOS, Linux, or Windows (with Bash shell)

### Windows Support

This script runs on Windows via Bash (Git Bash/MSYS2) but requires `jq` to be installed and accessible from within the bash environment.

**Step 1 — Install jq via winget (run in PowerShell or CMD):**
```powershell
winget install jqlang.jq
```

**Step 2 — Make jq accessible in Git Bash:**

winget installs `jq.exe` to a long path under `AppData` that Git Bash cannot find automatically. You need to copy it to a directory on your bash `PATH`:

```bash
# Run in Git Bash
mkdir -p ~/bin
cp "/c/Users/$USERNAME/AppData/Local/Microsoft/WinGet/Packages/jqlang.jq_Microsoft.Winget.Source_8wekyb3d8bbwe/jq.exe" ~/bin/jq.exe
```

Verify it works:
```bash
jq --version  # should print jq-1.x.x
```

> **Why this extra step?** winget updates the Windows system PATH, but Git Bash maps paths differently and `~/bin` (`C:\Users\<you>\bin`) is already on its PATH by default — the directory just needs to exist.

## Installation

### macOS / Linux

1. Clone this repository:
```bash
git clone https://github.com/YOUR_USERNAME/claude-code-statusline.git
cd claude-code-statusline
```

2. Copy to Claude Code config directory:
```bash
cp statusline.sh ~/.claude/statusline.sh
chmod +x ~/.claude/statusline.sh
```

3. Configure Claude Code to use the custom statusline:
```bash
cat > ~/.claude/settings.json << 'EOF'
{
  "statusLine": {
    "type": "command",
    "command": "bash \"$HOME/.claude/statusline.sh\""
  }
}
EOF
```

4. Restart Claude Code

### Windows (Git Bash/MSYS2)

**Prerequisites:**
- Git for Windows (includes Git Bash)
- Claude Code installed
- `jq` installed (see Requirements)

1. Clone this repository:
```bash
git clone https://github.com/YOUR_USERNAME/claude-code-statusline.git
cd claude-code-statusline
```

2. Copy to Claude Code config directory:
```bash
cp statusline.sh ~/.claude/statusline.sh
chmod +x ~/.claude/statusline.sh
```

3. Configure Claude Code to use the custom statusline:
```bash
cat > ~/.claude/settings.json << 'EOF'
{
  "statusLine": {
    "type": "command",
    "command": "bash \"$HOME/.claude/statusline.sh\""
  }
}
EOF
```

4. Restart Claude Code

## Configuration

Edit `~/.claude/settings.json` to customize display options.

### Cache Location

Default cache location: `/tmp/claude/statusline-usage-cache.json`

To change cache location, edit the `cache_file` variable in the script.

### Colors

Color definitions at the top of the script:
- Blue: Model name
- Cyan: Project directory
- Green/Yellow/Orange/Red: Usage percentage thresholds
- Magenta: Thinking mode indicator

## API Integration

This script calls the Anthropic usage API:
- Endpoint: `https://api.anthropic.com/api/oauth/usage`
- Auth: OAuth Bearer token
- Data: 5-hour, 7-day, and monthly usage limits

Tokens are automatically resolved from:
1. Environment variable: `CLAUDE_CODE_OAUTH_TOKEN`
2. macOS Keychain: `Claude Code-credentials`
3. Credentials file: `~/.claude/.credentials.json` (works on all platforms including Windows)
4. Linux Secret Service (GNOME Keyring)

## License

MIT - See [LICENSE](LICENSE) file

## Related Projects

- [claude-statusline](https://github.com/nilbuild/claude-statusline) — original source (also installable via `npx @kamranahmedse/claude-statusline`)
- [Claude HUD](https://github.com/jarrodwatts/claude-hud) — TypeScript/Node.js statusline plugin with similar features
