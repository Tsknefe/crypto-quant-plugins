# /market-snapshot

Create a concise market snapshot for a crypto symbol.

## Inputs
- symbol (e.g., BTCUSDT)
- timeframe (e.g., 5m, 1h, 1d)

## Output format (must follow)
### 1) Snapshot
- Last price
- Directional context (up/down/flat)
- Volatility note (ATR% or proxy)

### 2) Context
- Trend note (e.g., above/below a moving average if available)
- Key levels (only if the user provides levels)

### 3) Suggested next action
Recommend one:
- /signal (to generate trade intent)
- /risk-check (to gate execution)