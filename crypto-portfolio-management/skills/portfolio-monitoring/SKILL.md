# Portfolio Monitoring (Crypto)

## When to use
Trigger when user asks for portfolio status, allocation, performance, rebalancing, exposure, or drawdown.

## Objective
Turn holdings into a professional portfolio snapshot:
- equity, cash, exposure
- PnL summary
- concentration/drawdown risk flags
- clear next actions (only if user provides constraints)

## Workflow
1) Parse holdings and compute totals in base currency.
2) Summarize exposures and concentration.
3) Compute unrealized/realized PnL if inputs allow.
4) Flag risks: concentration, volatility, drawdown (if provided).
5) Actions: propose rebalancing only when targets/constraints exist.

## Output format (must follow)
### 1) Portfolio Summary
### 2) PnL
### 3) Risk Snapshot
### 4) Actions