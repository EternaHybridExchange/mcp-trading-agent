# DCA Accumulation Strategy

A long-only strategy that builds a position gradually over time through regular, fixed-size purchases, reducing the impact of short-term price volatility.

## Overview

| Parameter | Value |
|-----------|-------|
| Timeframe | Long-term (days to weeks) |
| Direction | Long only |
| Purchase frequency | Daily, every few days, or weekly |
| Position sizing | Fixed USDT amount or percentage of balance |
| Default leverage | 1-3x |

## How It Works

Dollar-cost averaging (DCA) involves buying a fixed amount of an asset at regular intervals, regardless of the current price. Over time, this smooths out the average entry price and reduces the risk of entering at a local top.

## Step 1: Define DCA Parameters

Before starting, decide on:

- **Symbol**: which asset to accumulate (e.g., BTCUSDT, ETHUSDT)
- **Purchase amount**: fixed USDT value per buy (e.g., $100) or a percentage of available balance (e.g., 5%)
- **Frequency**: how often to buy (the agent executes each time you ask it to)
- **Target position size**: optional upper limit on total accumulated size
- **Leverage**: keep low (1-3x) since this is a long-term hold strategy

## Step 2: Pre-Purchase Checks

Before each purchase:

1. Call `get_balance` to confirm sufficient available margin for the purchase.
2. Call `get_positions` to check the current accumulated position size and unrealised PnL.
3. Call `get_tickers` to get the current price.
4. Call `get_instruments` to get the minimum order quantity and lot size.

If the balance is too low for the planned purchase, reduce the amount or skip this interval.

## Step 3: Calculate Order Size

### Fixed Amount Method

```
order_size = purchase_amount_usdt / current_price
```

Example: $100 purchase at BTC = $100,000 gives order_size = 0.001 BTC.

### Percentage of Balance Method

```
purchase_amount = available_balance * percentage
order_size = purchase_amount / current_price
```

Example: 5% of $10,000 balance = $500, at BTC = $100,000 gives order_size = 0.005 BTC.

Round down to the nearest valid lot size from `get_instruments`.

## Step 4: Execute the Purchase

1. Call `set_leverage` to set the desired leverage (keep it low -- 1x to 3x).
2. Call `place_order` with:
   - `side` = "Buy"
   - `orderType` = "Market"
   - `qty` = calculated order size
   - No take-profit (this is an accumulation strategy)
   - Optional: set a wide stop-loss (e.g., 10-15% below entry) as a safety net

## Step 5: Track Average Entry Price

After each purchase, call `get_positions` to see the updated position. The position's `avgPrice` field shows the blended average entry price across all DCA purchases.

Keep a mental or written log:

| Purchase | Price | Size | Running Total | Avg Entry |
|----------|-------|------|---------------|-----------|
| 1 | $98,000 | 0.001 | 0.001 | $98,000 |
| 2 | $95,000 | 0.001 | 0.002 | $96,500 |
| 3 | $101,000 | 0.001 | 0.003 | $98,000 |

## Step 6: When to Take Profit

DCA is an accumulation strategy, but profits should eventually be taken. Consider taking profit when:

- **Price target reached**: the asset reaches a pre-defined price target (e.g., 20% above average entry)
- **Position size target**: the accumulated position has reached the desired size
- **Market conditions change**: fundamentals shift or the trend clearly reverses

To take profit:
- Close a portion of the position with `place_order` (Sell side)
- Or set a take-profit level with `set_tp_sl` on the accumulated position

### Partial Profit Taking

Consider scaling out rather than closing the entire position:
- Take 25% off at +10% from average entry
- Take another 25% at +20%
- Let the remaining 50% ride with a trailing stop (manually adjust SL upward as price rises using `set_tp_sl`)

## Risk Management

- **Low leverage**: 1-3x maximum. DCA is a patient strategy and high leverage defeats the purpose.
- **Stop-loss**: set a wide stop-loss (10-15% below average entry) as catastrophic protection, not as an active trading tool.
- **Maximum allocation**: do not allocate more than 20-30% of total portfolio to a single DCA accumulation.
- **Skip on low balance**: if available balance drops below 2x the planned purchase amount, skip the interval.

## When to Use DCA

- When you have conviction in an asset's long-term direction but are uncertain about short-term price
- During extended periods of consolidation or mild downtrends (accumulating at lower prices)
- When you want a systematic, emotion-free approach to position building

## When to Avoid DCA

- During confirmed bear markets with no clear support levels
- If the asset has poor liquidity or extremely wide spreads
- When your portfolio is already heavily concentrated in the same asset
