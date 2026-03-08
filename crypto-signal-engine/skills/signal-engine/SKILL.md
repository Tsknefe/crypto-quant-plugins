# Signal Engine (Crypto)

## When to use
Trigger this skill when the user asks for:
- buy/sell/hold style crypto trade decisions
- signal generation for BTC, ETH, or other trading pairs
- RSI, MACD, ATR, trend, or momentum interpretation
- model forecast + confidence evaluation
- "should I trade now?" requests
- explainable signal scoring before execution

## Objective
Produce an explainable and standardized crypto signal evaluation using:
- indicator alignment
- model edge
- confidence
- volatility-aware gating
- signal scoring
- structured directional intent

This skill does not directly execute trades. It produces a decision layer output that can later be consumed by risk and execution systems.

## Core definitions
- spot: current market price
- predicted_price: model forecast for the next horizon
- edge_pct = (predicted_price - spot) / spot
- confidence: model confidence in the range 0..1
- volatility: preferably ATR or ATR%
- cooldown_active: whether a recent trade blocks immediate re-entry

## Default parameters
- CONF_MIN = 0.55
- MIN_EDGE = 0.0010
- EXTRA_EDGE = 0.0025
- ATR_MULT_SL = 2.0
- ATR_MULT_TP = 3.0
- COOLDOWN_BARS = 3

## Gating rules
These rules are strict and must be checked before a directional trade intent is finalized.

- If confidence is provided and confidence < CONF_MIN => HOLD
- If edge_pct is available and abs(edge_pct) < MIN_EDGE => HOLD
- If volatility is extreme => HOLD or reduce conviction / size
- If cooldown_active is true => HOLD

If a hard gate fails, the final intent must be HOLD even if some indicators look favorable.

## Scoring model (0-100)
The signal score should be explainable and auditable.

Suggested logic:
- indicator alignment contribution
- edge contribution
- confidence contribution
- trend confirmation bonus
- volatility penalty

Suggested interpretation bands:
- 0-39: weak / avoid
- 40-59: mixed / low conviction
- 60-74: actionable but moderate
- 75-100: strong

Always explain the main drivers behind the score.

## Workflow
1. Read inputs:
   - symbol
   - timeframe
   - spot
   - predicted_price
   - confidence
   - RSI
   - MACD
   - ATR
   - trend proxy
   - cooldown state
2. Compute edge_pct if possible
3. Build market snapshot
4. Evaluate indicators
5. Apply gating rules
6. Produce signal score with rationale
7. Produce final directional signal:
   - LONG
   - SHORT
   - HOLD
8. If appropriate, indicate readiness for /trade-intent

## Output requirements
For `/signal`, output must contain:
### 1) Inputs
### 2) Market Snapshot
### 3) Indicators
### 4) Model Edge + Confidence
### 5) Gating Rules
### 6) Signal Score (0-100) + Rationale
### 7) Final Signal

For `/trade-intent`, output must:
- first emit a JSON object matching `trade-intent-schema.json`
- then emit a short human-readable explanation

## Guardrails
- Do not pretend confidence exists if it was not provided
- Do not produce LONG / SHORT when a hard gate clearly fails
- Do not hide uncertainty
- Prefer HOLD over forced action when evidence is weak
- Keep the result explainable enough for downstream risk review