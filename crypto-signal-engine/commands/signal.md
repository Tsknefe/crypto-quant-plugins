# /signal

Generate an explainable crypto trading signal summary using indicators, model edge, confidence, and volatility-aware gating rules.

## Purpose
Use this command to evaluate the current market state for a crypto symbol and produce a structured signal analysis before execution intent is finalized.

## Inputs
- symbol (required, e.g. BTCUSDT)
- timeframe (required, e.g. 5m, 15m, 1h)
- optional: spot
- optional: predicted_price
- optional: confidence
- optional: RSI
- optional: MACD
- optional: trend proxy (EMA/MA context)
- optional: ATR
- optional: cooldown_active
- optional: params
  - CONF_MIN (default 0.55)
  - MIN_EDGE (default 0.0010)
  - EXTRA_EDGE (default 0.0025)
  - ATR_MULT_SL (default 2.0)
  - ATR_MULT_TP (default 3.0)
  - COOLDOWN_BARS (default 3)

## Required behavior
- Compute edge_pct if spot and predicted_price are available:
  - edge_pct = (predicted_price - spot) / spot
- Evaluate indicator alignment:
  - RSI
  - MACD
  - trend proxy
- Evaluate volatility context:
  - prefer ATR% when ATR and spot are available
- Apply gating rules before producing any directional conclusion
- If gating fails, final signal must degrade to HOLD

## Gating rules
- If confidence is available and confidence < CONF_MIN => HOLD
- If edge_pct is available and abs(edge_pct) < MIN_EDGE => HOLD
- If volatility is extreme => HOLD or reduced conviction
- If cooldown_active is true => HOLD

## Output format (must follow)

### 1) Inputs
- symbol
- timeframe
- provided values
- active parameter values

### 2) Market Snapshot
- spot price
- short momentum summary
- volatility summary (ATR or ATR%)

### 3) Indicators
- RSI: value + interpretation
- MACD: direction + interpretation
- Trend proxy: bullish / bearish / neutral
- ATR context: normal / elevated / extreme

### 4) Model Edge + Confidence
- predicted_price
- edge_pct
- confidence
- reliability notes

### 5) Gating Rules
- confidence gate: passed / failed / not available
- edge gate: passed / failed / not available
- volatility gate: passed / warning / blocked
- cooldown gate: passed / failed

### 6) Signal Score (0-100) + Rationale
Explain how the score is formed from:
- indicator alignment
- edge contribution
- confidence contribution
- volatility penalty

### 7) Final Signal
- LONG / SHORT / HOLD
- summary rationale
- risk notes
- whether it should proceed to /trade-intent