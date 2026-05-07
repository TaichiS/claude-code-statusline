# Claude Code Statusline

Personal backup of Claude Code custom statusline script.

## Credits

**Original author unknown.**

This script is based on statusline concepts and API patterns from [Claude HUD](https://github.com/jarrodwatts/claude-hud) by Jarrod Watts (MIT License).

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

This script runs on Windows via Bash (Git Bash/MSYS2) but requires `jq` to be installed.

**Recommended installation (one-line):**
```powershell
winget install jqlang.jq
```
After installation, restart your terminal for PATH changes to take effect.

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
3. Credentials file: `~/.claude/.credentials.json`
4. Linux Secret Service (GNOME Keyring)
5. Windows: Not supported (script skips token resolution on Windows)

## License

MIT - See [LICENSE](LICENSE) file

## Disclaimer

This is a personal backup of a script whose original author is unknown. Use at your own risk. The script may contain bugs or security vulnerabilities.

## Related Projects

- [Claude HUD](https://github.com/jarrodwatts/claude-hud) - TypeScript/Node.js statusline plugin with similar features
