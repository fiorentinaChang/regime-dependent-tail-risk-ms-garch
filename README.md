# Regime-Dependent Tail Risk and Regime-Switching Predictive Distributions  
### Evidence from MS-GARCH with Explicit Tail Shift Quantification

---

## Overview

This project investigates whether Value-at-Risk (VaR) failures during market stress arise from structural tail shifts rather than volatility clustering alone.

Using U.S. equity data (1996–2025), I show that standard rolling Student-t GARCH(1,1) models exhibit severe under-coverage in high-volatility regimes. I then quantify regime-dependent tail thickening using extreme value methods and incorporate state dependence directly into a regime-weighted predictive mixture density.

---

## Research Question

Do stress regimes exhibit structural tail thickening beyond variance scaling?

---

## Empirical Motivation

Standard rolling Student-t GARCH(1,1) models generate well-calibrated VaR in calm periods.  
However, in high-volatility regimes:

- **VaR95 violations:** 11.84% (vs 5% nominal)  
- **VaR99 violations:** 3.67% (vs 1% nominal)  

Low-volatility regimes remain close to nominal coverage.

The baseline model already embeds:

- Strong volatility persistence (α + β ≈ 0.993)  
- Heavy-tailed innovations (ν ≈ 6–8)

Therefore, stress-period miscalibration cannot be explained by volatility clustering alone.

**Hypothesis:** Stress regimes involve structural changes in tail curvature, not merely higher conditional variance.

---

## Data

**Sample:** 1 January 1996 – 31 December 2025  
**Observations:** 6,268 aligned daily data points  

**Sources:**

- SPY adjusted close prices (Yahoo Finance)  
- VIX index (Yahoo Finance)  
- 3-month and 10-year Treasury yields (FRED)

Extreme left-tail events occur with unconditional frequency below 1% but cluster strongly in stress states.

---

## Regime Definitions

Three complementary classifications are used:

### 1. Exogenous Volatility Regimes (VIX Quantiles)
- Low / Mid / High partitions  
- 3,528 low-volatility days  
- 1,420 high-volatility days  

Extreme losses are several times more frequent in VIX-high regimes.

### 2. Endogenous Regimes (Two-State MS-GARCH)
- Latent state inferred via maximum likelihood  
- Hamilton filtering  
- State 2 corresponds to higher unconditional volatility  

Provides sharper separation of calm and stress periods than VIX classification.

### 3. Monetary Policy Shock Indicator (Supplementary)
- Tightening / easing episodes defined using standardized rate changes  
- Used as auxiliary regime information

---

## Methodology

### 1. Baseline Model

Rolling Student-t GARCH(1,1):

- One-step-ahead VaR forecasts  
- 5,008 aligned out-of-sample observations  
- Kupiec (1995) unconditional coverage test  
- Christoffersen (1998) conditional coverage test  

---

### 2. Tail Shift Quantification (POT–GPD)

Extreme value framework applied to standardized residuals:

- Peaks-over-Threshold (POT) method  
- Generalized Pareto Distribution (GPD)  
- Regime-specific tail index (ξ) estimation  
- Bootstrap inference for cross-regime differences  

This isolates structural tail curvature beyond scale effects.

---

### 3. Regime-Weighted Predictive Density

Predictive mixture representation:

f(r_{t+1}) = (1 − p_t) * f_low + p_t * f_high

Filtered regime probabilities reshape the full predictive distribution rather than mechanically scaling volatility.

VaR and Expected Shortfall are computed directly from the mixture density.

---

## Key Results

### Regime-Dependent VaR Failure

- Severe under-coverage concentrated in high-volatility regimes  
- Backtest rejection driven by stress states rather than uniform miscalibration  

---

### Evidence of Structural Tail Thickening

- Tail-event frequency increases from 0.28% (low) to 1.90% (high)  
- Bootstrap rejects equality of GPD tail indices across regimes  
- Tail index in high regime: ξ ≈ 0.62 (95% CI [0.51, 0.83])  

Stress states exhibit curvature shifts beyond variance amplification.

---

### Regime Mixture Improvement

High-regime VaR95 violations reduced:

11.84% → 10.68%

Improvement is stronger at the 95% level than at 99%, where tail sparsity limits precision (~0.37% extreme frequency).

---

## Event Study Evidence

Low → High regime transitions show:

- Immediate upward shift in predicted volatility  
- Persistent elevation after transition  
- Extreme losses spike at transition date  
- Logistic regression: VIX-high increases odds of extreme event by ~6.9×  

Regime entry reflects structural volatility change rather than isolated shock.

---

## Interpretation

A single-state Student-t specification imposes constant tail curvature across market conditions.

When the data-generating process exhibits regime-dependent survival-function curvature, no single heavy-tailed distribution can match both calm and stress periods.

Regime-weighted mixture densities partially resolve this structural limitation.

---

## Limitations

- Two-state specification  
- Constant transition probabilities  
- Sparse extreme observations in far tail  
- No risk-neutral pricing extension  

---

## Reproducibility

### Requirements

pip install numpy pandas scipy statsmodels arch scikit-learn matplotlib seaborn fredapi yfinance

### Notebook Workflow

1. Data download and alignment  
2. Baseline GARCH backtest  
3. MS-GARCH estimation  
4. POT tail estimation  
5. Regime mixture forecasting  
6. Event study analysis  

All models use rolling windows with aligned one-step-ahead forecasts.

---

## Author

Xinyi Chang  
MSc Application Portfolio Project
