# /risk-check

Assess risk conditions for a crypto symbol and produce execution guardrails.

## Inputs
- symbol (e.g., BTCUSDT)
- timeframe (e.g., 5m, 1h, 1d)
- optional: constraints (max risk per trade %, max drawdown %, max position %)

## Output format (must follow)
### 1) Inputs
### 2) Volatility + Regime Summary
### 3) Key Risk Flags
### 4) Controls
- Position sizing guidance
- Stop placement guidance (ATR-based)
- No-trade / cooldown conditions
### 5) Final Risk Verdict
LOW / MEDIUM / HIGH