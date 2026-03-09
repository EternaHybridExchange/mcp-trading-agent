---
name: trade
description: Place a perpetual futures trade with proper risk management, position sizing, and TP/SL.
---

# Trade Placement

Follow these steps to place a trade on Bybit perpetual futures via the Eterna MCP Gateway.

## Steps

### 1. Check Balance

Call `get_balance` to determine the available USDT balance and margin. Confirm there is sufficient free margin for the intended trade.

### 2. Check Existing Positions

Call `get_positions` to see all open positions. Verify you are not already exposed to the same symbol. If a position exists, decide whether to add to it or skip.

### 3. Get Instrument Details

Call `get_instruments` for the target symbol to retrieve:
- Minimum order quantity
- Lot size (quantity step)
- Tick size (price step)
- Maximum leverage

### 4. Get Current Price

Call `get_tickers` for the target symbol to get the current mark price and last traded price.

### 5. Calculate Position Size

Use the following formula:

```
risk_amount = available_balance * risk_percentage
position_size = risk_amount / (entry_price * stop_loss_distance)
```

Default values:
- `risk_percentage` = 0.02 (2% of balance)
- `stop_loss_distance` = 0.006 (0.6% from entry)

Round down to the nearest valid lot size from step 3.

### 6. Set Leverage

Call `set_leverage` with the desired leverage for the symbol. Default to 10x if not specified.

### 7. Place the Order

Call `place_order` with:
- `symbol` -- the trading pair (e.g., BTCUSDT)
- `side` -- Buy (long) or Sell (short)
- `orderType` -- Market or Limit
- `qty` -- the calculated position size
- `takeProfit` -- target price (default 1.0% from entry)
- `stopLoss` -- stop price (default 0.6% from entry)

### 8. Verify the Position

Call `get_positions` to confirm the position was opened with the correct size, leverage, take-profit, and stop-loss.

Report the trade details: symbol, side, size, entry price, leverage, TP, and SL.
