# Regime-Dependent Tail Risk and Regime-Switching Predictive Distributions
### Evidence from Two-State MS-GARCH with Explicit Tail Shift Quantification

---

## Overview

This project studies whether Value-at-Risk (VaR) failures during market stress arise from structural tail shifts rather than volatility clustering alone.

Using U.S. equity data (1996–2025), I document that rolling Student-t GARCH(1,1) models are well calibrated in calm periods but systematically under-cover during high-volatility regimes. I then:

1. Quantify regime-specific tail curvature using POT–GPD methods with bootstrap inference.
2. Estimate a two-state Markov-switching GARCH model.
3. Construct a regime-weighted predictive mixture density that allows tail shape to vary probabilistically with filtered state probabilities.

The results show that stress regimes involve structural tail thickening beyond variance scaling and that regime-switching predictive densities partially restore risk calibration.

---

## 1. Problem Statement

Standard rolling Student-t GARCH(1,1) models produce one-step-ahead VaR forecasts using:

- Persistent volatility dynamics  
- Fixed heavy-tailed innovations  

In calm regimes, coverage is near nominal levels.  
In high-volatility regimes:

- VaR95 violations: **11.84%** (vs 5% nominal)  
- VaR99 violations: **3.67%** (vs 1% nominal)

The baseline model already embeds:

- Strong volatility persistence (α + β ≈ 0.993)  
- Heavy-tailed innovations (ν ≈ 6–8)

Therefore, miscalibration cannot be attributed to volatility clustering alone.

**Central hypothesis:** Stress regimes involve structural tail thickening — a shift in tail curvature beyond simple variance scaling.

---

## 2. Data

Sample: 1 January 1996 – 31 December 2025  
Observations: 6,268 aligned daily data points  

Variables:

- SPY log returns (Yahoo Finance)  
- VIX index (Yahoo Finance)  
- 3-month and 10-year Treasury yields (FRED)  

All models use rolling windows with aligned one-step-ahead out-of-sample forecasts.

Extreme left-tail events occur with unconditional frequency below 1% but cluster strongly in stress states.

---

## 3. Regime Definitions

Three complementary classifications are used.

### (i) Exogenous Volatility Regimes (VIX Quantiles)

- Low / Mid / High partitions  
- 3,528 low-volatility days  
- 1,420 high-volatility days  

Extreme losses are several times more frequent in VIX-high regimes.

---

### (ii) Endogenous Regimes: Two-State MS-GARCH

A two-state Markov-switching GARCH model is estimated with Student-t innovations.

State process:
- First-order Markov chain
- Transition matrix estimated via maximum likelihood

Each regime has:
- Its own GARCH parameters
- Its own degrees-of-freedom parameter

Estimation procedure:
1. Gaussian MS-GARCH warm start
2. Student-t refinement via maximum likelihood
3. Multiple random initializations
4. Post-estimation relabeling (State 2 = high-volatility regime)

Filtered regime probabilities are computed using the Hamilton filter.

These probabilities parameterize regime-dependent predictive densities.

The endogenous MS state provides sharper separation of calm and stress periods than VIX classification alone.

---

### (iii) Monetary Policy Shock Indicator (Supplementary)

Standardized 3-month rate changes classify:
- Tightening episodes (n = 887)
- Easing episodes (n = 797)

Policy shocks are supplementary and not primary volatility states.

---

## 4. Methodology

### 4.1 Baseline Rolling Student-t GARCH

- Fixed rolling estimation window
- One-step-ahead VaR forecasts
- 5,008 aligned out-of-sample observations

Backtesting:

- Kupiec (1995) unconditional coverage test
- Christoffersen (1998) conditional coverage test

VaR failure is concentrated in stress regimes rather than uniform over time.

---

### 4.2 Tail-Shift Quantification (POT–GPD)

Extreme value framework applied to standardized residuals:

- Peaks-over-Threshold (POT) method
- Generalized Pareto Distribution (GPD)
- Regime-specific tail index (ξ) estimation
- Bootstrap confidence intervals
- Bootstrap test of equality of tail indices across regimes

Findings:

- Tail-event probability rises from 0.28% (low) to 1.90% (high)
- High regime tail index: ξ ≈ 0.62 (95% CI [0.51, 0.83])
- Bootstrap rejects equality of ξ across regimes

Stress regimes exhibit structural changes in tail curvature, not merely higher scale.

---

### 4.3 Regime-Weighted Predictive Mixture Density

Following Gray (1996), the predictive density is constructed as:

f(r_{t+1}) = (1 − p_t) f_low + p_t f_high

where:

- p_t = filtered probability of high-volatility state
- f_low, f_high = regime-specific conditional Student-t densities

VaR and Expected Shortfall are computed directly from the mixture distribution.

This specification reshapes the full predictive distribution rather than applying a mechanical volatility multiplier.

---

## 5. Empirical Results

### 5.1 Regime-Conditional VaR Failure

In VIX-high regimes:

- VaR95 violation rate ≈ 10–12%
- VaR99 violation rate ≈ 3%+

Low-volatility regimes remain near nominal.

Backtest rejection is driven by stress-period under-coverage.

---

### 5.2 Structural Tail Thickening

- Extreme losses disproportionately concentrated in stress states
- Monotonic increase in tail frequency across filtered MS probability bins
- Tail curvature shifts statistically significant under bootstrap inference

Stress regimes involve distributional curvature shifts beyond variance amplification.

---

### 5.3 Mixture Model Improvement

High-regime VaR95 violations reduced:

11.84% → 10.68%

Smaller improvement at 99% level (3.67% → 3.25%).

Extreme far-tail calibration remains challenging due to low tail-event frequency (~0.37%).

---

### 5.4 Transition Dynamics (Event Study)

Low → High regime switches show:

- Immediate upward shift in predicted volatility
- Persistent elevation post-transition
- Extreme losses spike at transition date
- Tail index does not shift discretely within short window

Logistic regression:

- VIX-high increases odds of extreme event by ~6.9×
- Policy shocks not statistically significant

Regime entry reflects structural volatility shifts rather than isolated shocks.

---

## 6. Interpretation

A single-state Student-t model imposes constant tail curvature across market conditions.

When the data-generating process exhibits regime-dependent survival-function curvature, no single heavy-tailed distribution can simultaneously match calm and stress periods.

Markov-switching mixture densities partially resolve this structural limitation by allowing predictive shape to vary probabilistically with filtered state probabilities.

---

## 7. Limitations

- Two-state specification may be coarse
- Constant transition probabilities
- Sparse extreme observations in far tail
- Risk-neutral pricing extension not implemented

---

## 8. Reproducibility

### Requirements

pip install numpy pandas scipy statsmodels arch scikit-learn matplotlib seaborn fredapi yfinance

### Workflow

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
