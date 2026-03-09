---
name: portfolio
description: Review and manage the full trading portfolio -- positions, PnL, balance, and margin health.
---

# Portfolio Management

A systematic approach to reviewing and managing all open positions, balance, and overall portfolio health.

## Workflow

### 1. Check All Open Positions

Call `get_positions` to retrieve every open position. For each position, note:
- Symbol
- Side (long/short)
- Size
- Entry price
- Current mark price
- Unrealised PnL
- Leverage
- Take-profit and stop-loss levels

### 2. Review Unrealised PnL

Assess each position:
- **Winning positions**: those with positive unrealised PnL
- **Losing positions**: those with negative unrealised PnL
- **At-risk positions**: those approaching their stop-loss

Calculate total unrealised PnL across all positions.

### 3. Check Balance and Margin Usage

Call `get_balance` to get:
- Total equity
- Available balance (free margin)
- Used margin
- Unrealised PnL

Calculate margin utilization:

```
margin_utilization = used_margin / total_equity * 100
```

Flag if margin utilization exceeds 70% -- this indicates high risk.

### 4. Identify Positions to Adjust

Review positions that may need action:
- **Close**: positions with unrealised loss exceeding 2% of total equity
- **Tighten SL**: winning positions where SL can be moved to breakeven
- **Take partial profit**: positions with unrealised gain exceeding 3% of equity
- **No TP/SL set**: positions missing take-profit or stop-loss (set them immediately)

For any adjustment, use `set_tp_sl` to update levels or `place_order` to close.

### 5. Portfolio Health Summary

Present a summary table:

```
Portfolio Summary
-----------------
Total Equity:        $X,XXX.XX
Available Margin:    $X,XXX.XX
Used Margin:         $X,XXX.XX
Margin Utilization:  XX.X%
Open Positions:      X
Total Unrealised PnL: +/- $X,XXX.XX
Winning / Losing:    X / X
```

Followed by a per-position breakdown and any recommended actions.
