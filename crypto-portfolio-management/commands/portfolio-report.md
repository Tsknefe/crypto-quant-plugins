# /portfolio-report

Generate a clean portfolio summary and performance snapshot.

## Inputs
- holdings (positions with size, avg price, current price) OR JSON pasted by user
- optional: base currency (default USDT)
- optional: start capital

## Output format (must follow)
### 1) Portfolio Summary
- Total equity
- Cash %
- Net exposure
- Top positions

### 2) PnL
- Realized PnL (if provided)
- Unrealized PnL
- Total PnL (%)

### 3) Risk Snapshot
- Concentration flags
- Volatility note
- Drawdown note (if provided)

### 4) Actions
- Rebalance suggestions (only if constraints/targets provided)
  - Risk reductions (if needed)