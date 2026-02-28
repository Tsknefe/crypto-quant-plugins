# Signal Scoring Model

The signal engine produces a normalized score between 0 and 100.

The score represents the strength and reliability of a trading signal.

## Components

The score combines four elements:

Indicator alignment  
Model edge  
Prediction confidence  
Risk penalty (volatility)

### 1 Indicator Alignment

Indicators considered:

RSI  
MACD  
Trend proxy (MA / EMA)

Scoring:

Strong alignment → +40  
Partial alignment → +20  
Neutral → +10  
Conflict → 0

### 2 Model Edge

Edge is defined as:

edge_pct = (predicted_price - spot) / spot

Edge contribution:

0–0.1% → +5  
0.1–0.25% → +10  
0.25–0.5% → +20  
>0.5% → +30

### 3 Confidence

Confidence scales the edge contribution.

confidence_scaled_edge = edge_score * confidence

Confidence values:

0.50–0.60 → weak  
0.60–0.75 → medium  
0.75–1.00 → strong

### 4 Risk Penalty

Volatility measured using ATR percentage.

ATR% penalty:

ATR < 1% → 0  
ATR 1–2% → −5  
ATR 2–3% → −10  
ATR > 3% → −20

### Final Score

signal_score =

indicator_score  
+ confidence_scaled_edge  
− risk_penalty

Clamp result between 0 and 100.