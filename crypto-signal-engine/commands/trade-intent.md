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


## Output (MUST FOLLOW STRICT FORMAT)

First output a JSON block matching trade-intent-schema.json.

Then output a short human-readable explanation.

### JSON Example

```json
{
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "intent": "LONG",
  "confidence": 0.72,
  "signal_score": 68,
  "levels": {
    "entry": 52100,
    "stop_loss": 50950,
    "take_profit": 54600
  },
  "controls": {
    "position_size_pct": 2.1,
    "cooldown_bars": 3,
    "gating": {
      "conf_min": 0.55,
      "min_edge": 0.001,
      "volatility_block": false
    }
  },
  "explanation": "Bullish RSI + MACD alignment, sufficient edge and confidence. Volatility moderate."
}