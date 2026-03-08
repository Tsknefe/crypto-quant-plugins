# /trade-intent

Generate a standardized crypto trade-intent object with direction, levels, controls, and short explanation.

## Purpose
Use this command after signal evaluation to produce a structured trade decision object that can be passed to downstream risk and execution layers.

## Inputs
- symbol (required, e.g. BTCUSDT)
- timeframe (required, e.g. 5m, 15m, 1h)
- optional: spot
- optional: predicted_price
- optional: confidence
- optional: signal_score
- optional: RSI
- optional: MACD
- optional: ATR
- optional: cooldown_active
- optional: source_model
- optional: market_regime
- optional: constraints
  - CONF_MIN (default 0.55)
  - MIN_EDGE (default 0.0010)
  - EXTRA_EDGE (default 0.0025)
  - ATR_MULT_SL (default 2.0)
  - ATR_MULT_TP (default 3.0)
  - COOLDOWN_BARS (default 3)
  - max_position_pct
  - risk_per_trade_pct

## Required behavior
- Output MUST begin with a JSON block matching trade-intent-schema.json
- Then output a short human-readable explanation
- If spot and predicted_price are available, compute:
  - edge_pct = (predicted_price - spot) / spot
- Determine intent:
  - LONG if bullish conditions pass
  - SHORT if bearish conditions pass
  - HOLD if gating fails or evidence is insufficient
- Use ATR-based levels when ATR and spot are available
- If intent is HOLD, levels may be null
- Position sizing should be confidence-weighted and volatility-aware
- Explanation must state why the intent passed or failed gating

## Intent rules
- If confidence is available and confidence < CONF_MIN => HOLD
- If edge_pct is available and abs(edge_pct) < MIN_EDGE => HOLD
- If volatility is extreme => HOLD or smaller size
- If cooldown_active is true => HOLD
- LONG typically requires positive edge and bullish indicator alignment
- SHORT typically requires negative edge and bearish indicator alignment

## Output
1. JSON block matching trade-intent-schema.json
2. Short explanation paragraph

## Example behavior notes
- Prefer LONG / SHORT / HOLD terminology in the JSON
- BUY / SELL wording may be used only in the human explanation if needed
- Keep rationale concise but auditable