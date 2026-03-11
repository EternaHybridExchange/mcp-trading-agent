# MCP Trading Agent

**Turn your IDE into a perpetual futures trading terminal. Ready-to-use configs and strategies for Claude Code, Cursor, and Claude Desktop.**

**0.014% maker fees on futures. Sub-account isolation. Copy config and trade.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![MCP Protocol](https://img.shields.io/badge/MCP-Protocol-blue.svg)](https://modelcontextprotocol.io)

---

## Pick Your IDE, Start Trading

| IDE | Config | Setup Time |
|---|---|---|
| [Claude Code](claude-code/) | `.mcp.json` + trading skills | 30 seconds |
| [Cursor](cursor/) | `.cursor/mcp.json` + rules | 30 seconds |
| [Claude Desktop](claude-desktop/) | `config.json` | 30 seconds |

## Getting Started

1. Navigate to your IDE's directory above
2. Copy the config file into your project
3. Ask your AI to call `register_agent` -- it gets an API key instantly
4. Add the key to the config, restart, trade

No Bybit account needed. No API keys to manage. No server to run.

---

## Included Strategies

| Strategy | Description | Style |
|---|---|---|
| [Momentum Scalping](strategies/momentum-scalping.md) | Scan for momentum, confirm with orderbook, scalp with tight TP/SL | Active, short timeframe |
| [Grid Trading](strategies/grid-trading.md) | Place limit orders across a price range | Passive, range-bound markets |
| [DCA Accumulation](strategies/dca-accumulation.md) | Dollar-cost average into positions over time | Conservative, long-term |

---

## Works With Any Framework

This repo focuses on IDE configs, but the same MCP server works with LangChain, AutoGen, CrewAI, and any MCP-compatible client. See the [eterna-mcp examples](https://github.com/eterna-exchange/eterna-mcp/tree/main/examples) for framework integrations.

---

## Available Tools

| Tool | Description |
|---|---|
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

## Coming Soon

- 130+ additional Bybit API endpoints (order management, position controls, kline data)
- Code execution sandbox -- submit TypeScript strategies for isolated execution
- Strategy runtime -- deploy strategies on cron, zero LLM at runtime
- Backtesting engine with historical data replay

See the full [roadmap](https://github.com/eterna-exchange/eterna-mcp/blob/main/ROADMAP.md).

---

## Ecosystem

| Repository | Description |
|---|---|
| [eterna-mcp](https://github.com/eterna-exchange/eterna-mcp) | Full gateway docs, benchmarks, and API reference |
| [bybit-mcp-server](https://github.com/eterna-exchange/bybit-mcp-server) | Bybit-focused managed MCP server |
| [mcp-trading-agent](https://github.com/eterna-exchange/mcp-trading-agent) | IDE configs and strategies (this repo) |
| [awesome-mcp-trading](https://github.com/eterna-exchange/awesome-mcp-trading) | Curated list of MCP trading resources |

---

Powered by [Eterna MCP Gateway](https://github.com/eterna-exchange/eterna-mcp)

## Contact

Questions or support: **hello@eterna.exchange**
