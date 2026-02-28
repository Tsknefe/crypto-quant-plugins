# /trade-intent

Generate a full crypto trading decision with signal evaluation, risk controls, and position sizing.

## Inputs
- symbol (e.g., BTCUSDT)
- timeframe (e.g., 5m, 15m, 1h)
- optional: spot, predicted_price, confidence, ATR, RSI, MACD (if user provides)
- optional: constraints (risk per trade %, max position %, cooldown)

## Output format (must follow)
### Trade Intent
- Symbol
- Direction (LONG / SHORT / HOLD)
- Entry (spot or provided level)
- Stop Loss (ATR-based)
- Take Profit (ATR-based)
- Confidence (0-1)
- Signal Score (0-100)
- Suggested Position Size (% capital)

### Explanation
- Why this direction
- Which gating rules passed/failed
- Risk notes (volatility/cooldown/drawdown if provided)