# regime-dependent-tail-risk-ms-garch
Markov-switching GARCH framework with explicit tail-shift quantification and regime-weighted predictive densities for stress-period VaR evaluation.
Regime-Dependent Tail Risk and Regime-Switching Predictive Distributions
Evidence from MS-GARCH with Explicit Tail Shift Quantification
Overview

Markov-switching GARCH framework with explicit tail-shift quantification and regime-weighted predictive densities for stress-period VaR evaluation.

Research Motivation

Standard rolling Student-t GARCH(1,1) models generate one-step-ahead VaR forecasts using persistent volatility dynamics and fixed heavy-tailed innovations.

In calm periods, coverage is close to nominal levels.
In high-volatility regimes, systematic under-coverage emerges:

VaR95 violations: 11.84% (vs 5% nominal)

VaR99 violations: 3.67% (vs 1% nominal)

Low-volatility regimes remain approximately well calibrated.

The baseline model already embeds:

Strong volatility persistence (α + β ≈ 0.993)

Heavy-tailed innovations (ν ≈ 6–8)

Therefore, miscalibration cannot be attributed to volatility clustering alone.

Central hypothesis:
Stress regimes involve structural tail thickening — a shift in tail curvature beyond variance scaling.

Contributions

This project develops a three-stage empirical framework:

1. Regime-Conditional Backtesting

Out-of-sample VaR forecasts evaluated conditional on volatility states.

2. Tail-Shift Quantification (POT–GPD)

Peaks-over-Threshold estimation

Regime-specific GPD tail index (ξ)

Bootstrap inference for cross-regime differences

3. Regime-Weighted Predictive Density
𝑓
(
𝑟
𝑡
+
1
)
=
(
1
−
𝑝
𝑡
)
𝑓
𝑙
𝑜
𝑤
+
𝑝
𝑡
𝑓
ℎ
𝑖
𝑔
ℎ
f(r
t+1
	​

)=(1−p
t
	​

)f
low
	​

+p
t
	​

f
high
	​


Filtered regime probabilities reshape the entire predictive distribution.

Key Empirical Results
Regime-Dependent VaR Failure

Severe under-coverage concentrated in stress regimes

VaR95 reduced from 11.84% → 10.68% under mixture specification

Extreme 99% tails remain difficult to calibrate

Evidence of Structural Tail Thickening

Tail-event frequency increases multiple-fold in stress states

Bootstrap rejects equality of tail indices across regimes

Monotonic gradient across filtered MS probabilities

Data

SPY returns (Yahoo Finance)

VIX index

3M and 10Y Treasury yields (FRED)

Sample: 1996–2025

6,268 daily observations

Reproducibility
Requirements
pip install numpy pandas scipy statsmodels arch scikit-learn matplotlib seaborn fredapi yfinance
Notebook Order

Data download

Baseline GARCH backtest

MS-GARCH estimation

POT tail estimation

Regime-mixture forecast

Event study

Limitations

Two-state structure

Constant transition probabilities

Low extreme-event frequency (~0.37%)

No risk-neutral extension
