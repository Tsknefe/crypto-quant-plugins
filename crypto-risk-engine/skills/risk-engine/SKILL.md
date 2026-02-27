# Risk Engine (Crypto)

## When to use
Trigger when user asks about risk, sizing, stops, volatility, drawdown limits, or wants guardrails before entering a trade.

## Objective
Produce consistent risk controls and an explicit risk verdict to gate execution:
- volatility-aware sizing guidance
- ATR-based stop guidance
- drawdown guard reminders
- no-trade conditions

## Workflow
1) Confirm: symbol, timeframe, and any constraints (risk budget, max position, max drawdown).
2) Volatility: prefer ATR% if available; otherwise use any provided proxy.
3) Regime: simple trend + volatility classification (risk-on / risk-off).
4) Controls:
   - sizing guidance scaled by volatility/regime
   - stop guidance (ATR multiple)
   - cooldown/no-trade conditions for extreme volatility or invalid edge
5) Verdict: LOW/MEDIUM/HIGH with rationale.

## Output format (must follow)
### 1) Inputs
### 2) Volatility + Regime Summary
### 3) Key Risk Flags
### 4) Controls (Sizing + Stops + No-Trade)
### 5) Final Risk Verdict