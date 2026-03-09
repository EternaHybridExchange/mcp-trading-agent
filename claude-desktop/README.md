# Claude Desktop Setup

Configure Claude Desktop to trade perpetual futures through the Eterna MCP Gateway.

## Files

| File | Purpose |
|------|---------|
| `config.json` | MCP server configuration for Claude Desktop |

## Setup

### 1. Locate Your Config File

Claude Desktop stores its configuration in a platform-specific location:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

### 2. Add the MCP Server

Open the config file and merge the contents of `config.json` from this directory. If the file already exists and has other MCP servers, add the `eterna-trading` entry to the existing `mcpServers` object.

If the file does not exist, copy `config.json` directly to the appropriate location.

### 3. Configure Your API Key

Replace `eterna_mcp_your_key_here` with your actual API key. If you do not have one yet, see the root README for instructions on how to register.

### 4. Restart Claude Desktop

Close and reopen Claude Desktop for the new MCP server to be detected.

## Limitations vs Claude Code

Claude Desktop supports MCP tool calls but does not support:

- **Skills** -- there is no `.claude/skills/` equivalent in Claude Desktop. The agent will not have pre-loaded trading strategies.
- **Project-scoped config** -- the MCP config is global, not per-project.
- **File system access** -- Claude Desktop cannot read or write local files the way Claude Code can.

Despite these limitations, you can still use all 12 Eterna MCP tools directly. Simply describe what you want to do and Claude Desktop will call the appropriate tools.

## Usage

Once configured, you can ask Claude Desktop things like:

- "What is my trading balance?"
- "Show me all my open positions"
- "Buy 0.05 BTC at market with 10x leverage"
- "Set a stop-loss at 95000 on my BTC position"
- "What are the biggest movers today?"
