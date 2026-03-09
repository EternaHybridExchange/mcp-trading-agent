# Momentum Scalping Strategy

A short-term trading strategy that identifies assets with strong price momentum, confirms direction via the orderbook, and captures quick profits with tight risk management.

## Overview

| Parameter | Value |
|-----------|-------|
| Timeframe | Intraday (minutes to hours) |
| Take-Profit | 1.0% from entry |
| Stop-Loss | 0.6% from entry |
| Risk per trade | 1-2% of available balance |
| Max concurrent positions | 3 |
| Default leverage | 10x |

## Step 1: Market Scan

Call `get_tickers` without a symbol to retrieve all perpetual futures tickers.

Filter for momentum candidates:

- **Long candidates**: `price24hPcnt` greater than 0.03 (3% gain in 24h)
- **Short candidates**: `price24hPcnt` less than -0.03 (3% decline in 24h)

Sort by absolute `price24hPcnt` descending to prioritize the strongest movers.

Additionally, filter out low-volume symbols. Compare each symbol's `volume24h` against the median volume across all tickers and discard those below the median.

## Step 2: Orderbook Confirmation

For each candidate from the scan, call `get_orderbook` to analyze supply and demand.

### For Long Entries

Sum the bid volume across the top 5 price levels and compare to the ask volume across the top 5 levels:

```
bid_volume_ratio = total_bid_volume / total_ask_volume
```

Proceed only if `bid_volume_ratio` >= 1.5 (bids are 50% larger than asks, indicating buying pressure).

### For Short Entries

```
ask_volume_ratio = total_ask_volume / total_bid_volume
```

Proceed only if `ask_volume_ratio` >= 1.5 (asks dominate, indicating selling pressure).

If the orderbook does not confirm the direction, skip the symbol and move to the next candidate.

## Step 3: Entry

Once a candidate passes both the momentum scan and orderbook confirmation:

1. Call `get_instruments` for the symbol to get lot size, tick size, and minimum order quantity.
2. Call `get_balance` to check available margin.
3. Calculate position size:

```
risk_amount = available_balance * 0.02
stop_loss_distance = entry_price * 0.006
position_size = risk_amount / stop_loss_distance
```

Round down to the nearest valid lot size.

4. Call `set_leverage` to set 10x leverage (or your preferred level).
5. Call `place_order` as a market order with:
   - **Take-profit**: entry price * 1.01 (long) or entry price * 0.99 (short)
   - **Stop-loss**: entry price * 0.994 (long) or entry price * 1.006 (short)

## Step 4: Position Limits

Before entering, call `get_positions` to count open positions. If 3 positions are already open, do one of the following:

- Wait for a position to close at TP or SL
- Close the weakest-performing position to free up a slot

Never exceed 3 concurrent scalp positions.

## Step 5: Cycle Workflow

After placing a trade:

1. Call `get_positions` to verify the entry
2. Report trade details (symbol, side, size, entry, TP, SL)
3. If fewer than 3 positions are open, return to Step 1 and scan again
4. Repeat

## Example Scenario

1. `get_tickers` returns SOLUSDT with `price24hPcnt` = 0.054 (5.4% up)
2. `get_orderbook` for SOLUSDT shows bid volume of 45,000 SOL vs ask volume of 22,000 SOL (ratio = 2.04) -- confirmed
3. Current price is $180.50
4. Balance is $5,000, risk 2% = $100
5. Stop-loss distance = $180.50 * 0.006 = $1.083
6. Position size = $100 / $1.083 = 92.3 SOL, rounded to 92 SOL
7. Place market long: TP = $182.31, SL = $179.42
8. Verify position is open with correct parameters

## Risk Notes

- Avoid re-entering a symbol that just hit stop-loss in the same session
- Reduce position sizes during periods of unusually high volatility
- This strategy works best during active market hours with high volume
- Always confirm the orderbook before entry -- momentum without volume confirmation often leads to false breakouts
