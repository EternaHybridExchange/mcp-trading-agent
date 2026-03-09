# Cursor Setup

Configure Cursor to trade perpetual futures through the Eterna MCP Gateway.

## Files

| File | Purpose |
|------|---------|
| `.cursor/mcp.json` | MCP server configuration for Cursor |
| `.cursorrules` | Trading-focused rules and guidelines for the AI agent |

## Setup

### 1. Add the MCP Server

Copy the `.cursor/mcp.json` file into your project root so the directory structure looks like:

```
your-project/
  .cursor/
    mcp.json
  .cursorrules
```

Alternatively, open Cursor Settings and add the MCP server manually under the MCP section.

### 2. Configure Your API Key

Open `.cursor/mcp.json` and replace `eterna_mcp_your_key_here` with your actual API key. If you do not have one yet, see the root README for instructions on how to register.

### 3. Add Cursor Rules

Copy `.cursorrules` into your project root. This file tells Cursor how to behave as a trading agent -- what tools are available, risk management rules, and position sizing guidelines.

### 4. Reload Cursor

Restart Cursor or reload the window so it picks up the new MCP server and rules.

## What `.cursorrules` Does

The `.cursorrules` file provides Cursor with context about:

- The Eterna MCP Gateway and available trading tools
- Risk management rules (always use stop-loss and take-profit)
- Position sizing formulas
- The list of all 12 available MCP tools and what they do

This ensures that when you ask Cursor to place a trade or manage positions, it follows safe trading practices by default.

## Usage

Once configured, you can ask Cursor things like:

- "What is my current balance?"
- "Open a 0.1 BTC long with 10x leverage, 1% TP, 0.5% SL"
- "Show me all open positions"
- "What are the top movers in the last 24 hours?"
- "Close my ETH position"
