# /trade-intent

Generate a full crypto trading decision with signal evaluation, risk controls, and position sizing.

## Inputs

- symbol (BTCUSDT etc)
- timeframe (5m, 15m, 1h)
- optional market context
- optional prediction + confidence

## Workflow

1. Generate signal context
   - RSI state
   - MACD direction
   - trend context

2. Evaluate model edge
   edge_pct = (predicted_price - spot) / spot

3. Apply gating rules

CONF_MIN = 0.55  
MIN_EDGE = 0.10%

Rules:

If confidence < CONF_MIN → HOLD  
If abs(edge_pct) < MIN_EDGE → HOLD  

4. Risk evaluation

Use volatility proxy (ATR or similar).

Determine:

- stop distance
- acceptable risk

5. Position sizing

Sizing must consider:

- confidence
- volatility
- risk budget

Higher confidence → larger size  
Higher volatility → smaller size

6. Produce execution intent

## Output format

### Trade Intent

Symbol  
Direction (LONG / SHORT / HOLD)

Entry price

Stop Loss
Take Profit

Confidence

Signal Score (0-100)

Suggested Position Size (% capital)

### Explanation

Explain briefly:

- why signal triggered
- why risk level acceptable