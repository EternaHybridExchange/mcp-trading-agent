# Claude Code Setup

Configure Claude Code to trade perpetual futures through the Eterna MCP Gateway.

## Files

| File | Purpose |
|------|---------|
| `.mcp.json` | MCP server configuration |
| `.claude/skills/trade/SKILL.md` | General trade placement skill |
| `.claude/skills/scalp/SKILL.md` | Momentum scalping strategy skill |
| `.claude/skills/portfolio/SKILL.md` | Portfolio management skill |

## Setup

### 1. Copy MCP Config

Copy `.mcp.json` to your project root:

```
your-project/
  .mcp.json
  .claude/
    skills/
      trade/
        SKILL.md
      scalp/
        SKILL.md
      portfolio/
        SKILL.md
```

### 2. Configure Your API Key

Open `.mcp.json` and replace `eterna_mcp_your_key_here` with your actual API key. If you do not have one yet, see the root README for instructions on how to register.

### 3. Copy Skills

Copy the `.claude/` directory into your project root. These skills teach Claude Code specific trading workflows:

- **trade** -- Step-by-step trade placement with proper risk management.
- **scalp** -- Momentum scalping strategy with automated scanning and tight TP/SL.
- **portfolio** -- Portfolio health checks, PnL review, and position adjustments.

### 4. Start Claude Code

Launch Claude Code in your project directory. It will automatically detect the MCP server and available skills.

## Usage

Once configured, you can ask Claude Code to:

- "Place a long on ETH with 5x leverage" -- triggers the trade skill
- "Scalp the top movers right now" -- triggers the scalp skill
- "How is my portfolio doing?" -- triggers the portfolio skill
- "Show me the orderbook for BTC"
- "What is my balance?"
- "Close all losing positions"

Claude Code will use the skills as contextual guides and call the appropriate MCP tools to execute.
