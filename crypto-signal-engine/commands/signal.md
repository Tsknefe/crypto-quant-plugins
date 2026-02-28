# /signal

Generate a standardized crypto trading signal with explicit gating, risk notes, and execution-ready intent.

## Inputs
- symbol (e.g., BTCUSDT)
- timeframe (e.g., 5m, 15m, 1h)
- optional: params
  - CONF_MIN (default 0.55)
  - MIN_EDGE (default 0.10%)
  - EXTRA_EDGE (default 0.25%)
  - ATR_MULT_SL (default 2.0)
  - ATR_MULT_TP (default 3.0)
  - COOLDOWN_BARS (default 3)

## Output format (must follow)
### 1) Inputs
### 2) Market Snapshot
- price (spot)
- recent momentum (brief)
- volatility proxy (ATR% or proxy)

### 3) Indicators
- RSI (level + interpretation)
- MACD (direction)
- Trend proxy (MA/EMA context)
- ATR context (risk)

### 4) Model Edge + Confidence (if available)
- predicted_price
- edge_pct = (predicted_price - spot) / spot
- confidence (0-1)
- notes on reliability

### 5) Gating Rules
- If confidence < CONF_MIN => HOLD (no-trade)
- If abs(edge_pct) < MIN_EDGE => HOLD (no-trade)
- If volatility is extreme => HOLD or reduce sizing
- If in cooldown => HOLD

### 6) Signal Score (0-100) + Rationale
Explain how indicators + edge + confidence + risk penalty combine.

### 7) Trade Intent
BUY / SELL / HOLD
Include:
- entry rationale
- suggested position sizing hint (confidence-weighted, volatility-aware)
- stops: ATR-based SL/TP guidance
- trailing-stop suggestion (optional)