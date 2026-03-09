# MCP Trading Agent

**Turn your IDE into a perpetual futures trading terminal. Ready-to-use configurations for Claude Code, Cursor, and Claude Desktop.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![MCP Protocol](https://img.shields.io/badge/MCP-Protocol-blue.svg)](https://modelcontextprotocol.io)

---

## Overview

This repository provides pre-configured MCP (Model Context Protocol) setups and trading strategies for AI-powered perpetual futures trading via the [Eterna MCP Gateway](https://github.com/eterna-exchange/eterna-mcp). Pick your IDE, copy the config, and start trading from your editor.

## Supported IDEs

| IDE | Directory | Description |
|-----|-----------|-------------|
| [Cursor](cursor/) | `cursor/` | MCP server config and `.cursorrules` for trading |
| [Claude Code](claude-code/) | `claude-code/` | MCP config with skills for trade, scalp, and portfolio management |
| [Claude Desktop](claude-desktop/) | `claude-desktop/` | MCP server config for the desktop app |

## Included Strategies

| Strategy | File | Description |
|----------|------|-------------|
| [Momentum Scalping](strategies/momentum-scalping.md) | `strategies/momentum-scalping.md` | Scan for momentum, confirm with orderbook, scalp with tight TP/SL |
| [Grid Trading](strategies/grid-trading.md) | `strategies/grid-trading.md` | Place limit orders across a price range for range-bound markets |
| [DCA Accumulation](strategies/dca-accumulation.md) | `strategies/dca-accumulation.md` | Dollar-cost average into positions over time |

## Getting Started

1. **Pick your IDE** -- navigate to the corresponding directory above.
2. **Copy the config** -- follow the README in that directory to set up the MCP server.
3. **Get an API key** -- see below.
4. **Start trading** -- ask your AI assistant to place trades, scalp, or manage your portfolio.

## How to Get an API Key

The Eterna MCP Gateway issues API keys through the MCP protocol itself:

1. **Connect without authentication** -- set up the MCP server config without an API key (leave the `Authorization` header empty or omit it).
2. **Call `register_agent`** -- once connected, ask your AI assistant to register. It will call the `register_agent` tool and receive an API key in the response.
3. **Save the key** -- update your MCP config with the returned key in the `Authorization` header as `Bearer eterna_mcp_<your_key>`.
4. **Reconnect** -- restart your IDE or reload the MCP server. You are now authenticated.

## Available Tools

The Eterna MCP Gateway exposes the following tools:

| Tool | Description |
|------|-------------|
| `register_agent` | Register and receive an API key |
| `get_tickers` | Current prices and 24h statistics |
| `get_instruments` | Contract specs: tick size, lot size, leverage limits |
| `get_orderbook` | Order book depth for a symbol |
| `get_balance` | Account balance and available margin |
| `get_positions` | All open positions with PnL and leverage |
| `get_orders` | Active and recent order history |
| `place_order` | Place a market or limit order with optional TP/SL |
| `close_position` | Close an entire position at market price |
| `get_deposit_address` | Get deposit address for funding |
| `get_deposit_records` | View deposit history |
| `transfer_to_trading` | Move funds from Funding to Trading wallet |

---

Powered by [Eterna MCP Gateway](https://github.com/eterna-exchange/eterna-mcp)

## Ecosystem

- [eterna-mcp](https://github.com/eterna-exchange/eterna-mcp) -- Canonical docs for the Eterna MCP Gateway
- [bybit-mcp-server](https://github.com/eterna-exchange/bybit-mcp-server) -- Bybit-focused managed MCP server
- [mcp-trading-agent](https://github.com/eterna-exchange/mcp-trading-agent) -- IDE configurations and strategies (this repo)
- [awesome-mcp-trading](https://github.com/eterna-exchange/awesome-mcp-trading) -- Curated list of MCP trading resources

## Contact

For questions, support, or partnership inquiries: **hello@eterna.exchange**
