# Signal Engine (Crypto)

## When to use
Trigger when the user asks for:
- buy/sell decision, signal, entry/exit
- RSI/MACD/ATR interpretation
- model prediction + confidence
- "should I trade BTC/ETH now?" style requests

## Objective
Produce an execution-ready signal summary with explicit gating rules:
- indicators + model edge + confidence
- risk penalty from volatility (ATR)
- standardized output sections
- clear BUY/SELL/HOLD intent plus controls

## Core definitions
- spot: current price
- predicted_price: model forecast (if provided)
- edge_pct = (predicted_price - spot) / spot
- confidence: 0..1 (distance-based or provided)
- volatility: prefer ATR% if available

## Default parameters (overrideable)
- CONF_MIN = 0.55
- MIN_EDGE = 0.10% (0.0010)
- EXTRA_EDGE = 0.25% (0.0025)
- ATR_MULT_SL = 2.0
- ATR_MULT_TP = 3.0
- COOLDOWN_BARS = 3

## Gating rules (strict)
- If confidence < CONF_MIN => HOLD (no-trade)
- If abs(edge_pct) < MIN_EDGE => HOLD (no-trade)
- If volatility is extreme => HOLD or reduce sizing
- If cooldown is active (user indicates recent trade) => HOLD

## Scoring (0-100, explainable)
Build the score from:
- Indicator alignment (RSI + MACD + trend proxy)
- Edge contribution scaled by confidence
- Risk penalty from volatility (ATR%)

Always explain the major contributors and keep it auditable.

## Workflow
1) Confirm inputs: symbol, timeframe, and any custom params.
2) Market snapshot: spot, brief momentum context, volatility proxy (ATR%).
3) Indicator state: RSI, MACD, trend proxy, ATR context.
4) Model block (if available): compute edge_pct and confidence.
5) Apply gating rules.
6) Score (0-100) with rationale.
7) Trade intent + controls (sizing hint + ATR-based stops).

## Output format (must follow)
### 1) Inputs
### 2) Market Snapshot
### 3) Indicators
### 4) Model Edge + Confidence (if available)
### 5) Gating Rules
### 6) Signal Score (0-100) + Rationale
### 7) Trade Intent (BUY/SELL/HOLD) + Controls (Sizing + Stops)