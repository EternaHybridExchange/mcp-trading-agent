# Grid Trading Strategy

A range-bound strategy that places a series of limit orders across a defined price range, profiting from price oscillations within the grid.

## Overview

| Parameter | Value |
|-----------|-------|
| Timeframe | Medium-term (hours to days) |
| Market condition | Range-bound / sideways |
| Grid levels | 5-10 orders per side |
| Position size per level | Equal split of total allocation |
| Default leverage | 3-5x |

## How It Works

Grid trading divides a price range into evenly spaced levels. Buy orders are placed below the current price and sell orders above it. As price oscillates within the range, orders are filled and profits are captured from the spread between grid levels.

## Step 1: Define the Price Range

Analyze the recent price action to identify a trading range:

1. Call `get_kline` with an appropriate interval (e.g., 60-minute candles, 48 periods) to see the recent highs and lows.
2. Identify the support level (range bottom) and resistance level (range top).
3. Alternatively, use `get_tickers` to get the current price and define the range as +/- a percentage (e.g., +/- 3% from current price).

Example for ETHUSDT at $3,500:
- Range bottom: $3,395 (-3%)
- Range top: $3,605 (+3%)

## Step 2: Calculate Grid Levels

Divide the range into evenly spaced levels:

```
grid_spacing = (range_top - range_bottom) / (number_of_levels + 1)
```

For 8 levels across the $3,395 - $3,605 range:

```
grid_spacing = ($3,605 - $3,395) / 9 = ~$23.33
```

Levels: $3,418, $3,442, $3,465, $3,488, $3,512, $3,535, $3,558, $3,582

## Step 3: Spread Analysis

Call `get_orderbook` for the target symbol to analyze the current spread:

- A tight spread (small difference between best bid and ask) confirms good liquidity for limit orders.
- If the spread is wide relative to your grid spacing, consider using fewer grid levels or a wider range.
- Check that sufficient volume exists at the price levels where you plan to place orders.

## Step 4: Position Sizing Per Level

1. Call `get_balance` to determine available margin.
2. Decide the total allocation for the grid (e.g., 30% of available balance).
3. Divide equally across grid levels:

```
size_per_level = total_allocation / number_of_levels / current_price
```

4. Call `get_instruments` to get the lot size and round each order quantity to a valid amount.

## Step 5: Place Grid Orders

For each level:

- **Below current price**: place a Buy Limit order (these are the "buy the dip" orders)
- **Above current price**: place a Sell Limit order (these are the "sell the rip" orders)

Use `place_order` with `orderType` = "Limit" for each level:

```
For buy levels below current price:
  place_order(symbol, side="Buy", orderType="Limit", qty=size_per_level, price=grid_level)

For sell levels above current price:
  place_order(symbol, side="Sell", orderType="Limit", qty=size_per_level, price=grid_level)
```

## Step 6: Risk Management

Set overall risk boundaries for the grid:

- **Upper stop-loss**: if price breaks above the range top by 1-2%, close all short positions and cancel remaining sell orders
- **Lower stop-loss**: if price breaks below the range bottom by 1-2%, close all long positions and cancel remaining buy orders
- **Maximum exposure**: total grid exposure should not exceed 30% of account equity

Use `set_tp_sl` on any filled positions to add protective stops.

## Step 7: Monitoring

Grid trading requires periodic monitoring because the MCP agent does not run continuously:

1. Call `get_positions` to see which grid orders have been filled
2. Call `get_order_history` to check for recent fills
3. Call `get_tickers` to see if price is still within the grid range
4. If price has drifted significantly, consider:
   - Cancelling out-of-range orders with `cancel_order`
   - Placing new orders centered around the current price
   - Closing the grid entirely if the market has broken out of the range

## Example Setup

Symbol: BTCUSDT at $100,000

| Level | Price | Side | Size |
|-------|-------|------|------|
| 1 | $97,000 | Buy | 0.005 BTC |
| 2 | $98,000 | Buy | 0.005 BTC |
| 3 | $99,000 | Buy | 0.005 BTC |
| 4 | $101,000 | Sell | 0.005 BTC |
| 5 | $102,000 | Sell | 0.005 BTC |
| 6 | $103,000 | Sell | 0.005 BTC |

Total allocation: 0.03 BTC ($3,000 notional)

## When to Avoid Grid Trading

- During strong trending markets (price will blow through one side of the grid)
- Around major news events or scheduled announcements
- When volatility is extremely high (grid spacing may be too tight)
- When funding rates are heavily skewed (indicates strong directional bias)
