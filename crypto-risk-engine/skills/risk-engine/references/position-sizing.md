# Position Sizing Model (volatility-aware, confidence-weighted)

This sizing model converts signal confidence + volatility into a suggested position size.

## Inputs
- risk_budget (R): max % of equity risked per trade (default: 0.50%)
- confidence (C): 0..1 (default: 0.55 if unknown)
- ATR% (V): volatility proxy (ATR / spot). If unknown assume 1.5%.
- stop_distance_pct (S): stop distance as % of spot (preferred).
  If unknown: use S = 2 * ATR%.

## 1) Risk per trade
R = 0.50% of equity (default)
Low risk mode: 0.25%
High conviction mode: up to 1.00% (only when C is high and V is low)

## 2) Volatility factor
We reduce size when volatility rises:

vol_factor = clamp( 1.5% / V , 0.25, 1.25 )

Examples:
- V = 1.5% => 1.00
- V = 3.0% => 0.50
- V = 0.75% => 1.25 (cap)

## 3) Confidence factor
We scale exposure with confidence:

conf_factor = clamp( (C - 0.50) / 0.50 , 0.0, 1.0 )

Examples:
- C = 0.55 => 0.10
- C = 0.70 => 0.40
- C = 0.90 => 0.80
- C <= 0.50 => 0.0

## 4) Position size (as % equity)
Position size is constrained by risk and stop distance:

pos_size_pct = (R / S) * vol_factor * (0.50 + conf_factor)

Where:
- (R / S) ensures risk budget is respected given the stop distance
- vol_factor reduces size in high volatility
- (0.50 + conf_factor) ensures a baseline but rewards confidence

Clamp:
- min position: 0.25% equity
- max position: 5.00% equity (unless user overrides)

## Practical defaults
If S is unknown:
S = 2 * ATR%

If ATR% unknown:
V = 1.5%
S = 3.0%

## Output requirements
Always output:
- R, C, V, S
- pos_size_pct
- brief rationale (1-2 sentences)