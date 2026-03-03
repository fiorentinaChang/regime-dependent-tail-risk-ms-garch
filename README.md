# regime-dependent-tail-risk-ms-garch
Markov-switching GARCH framework with explicit tail-shift quantification and regime-weighted predictive densities for stress-period VaR evaluation.
1. Research Motivation

Standard rolling Student-t GARCH(1,1) models generate one-step-ahead VaR forecasts using persistent volatility dynamics and fixed heavy-tailed innovations.

In calm periods, coverage is close to nominal levels.
However, during high-volatility regimes, systematic under-coverage emerges:

VaR95 violations: 11.84% (vs 5% nominal)

VaR99 violations: 3.67% (vs 1% nominal)

Low-volatility regimes remain approximately well calibrated.

The baseline model already embeds:

Strong volatility persistence (α + β ≈ 0.993)

Heavy-tailed innovations (ν ≈ 6–8)

Therefore, miscalibration cannot be attributed to volatility clustering alone.

Central hypothesis:
Stress regimes involve structural tail thickening — a shift in tail curvature beyond simple variance scaling.


2. Contributions

This project develops a three-stage empirical framework:

(1) Regime-Conditional Backtesting

Out-of-sample VaR forecasts are evaluated conditional on volatility regimes, documenting state-dependent failure.

(2) Explicit Tail Shift Quantification

Using a Peaks-over-Threshold (POT) framework:

Generalized Pareto Distribution (GPD) estimation

Regime-specific tail index (ξ) estimation

Bootstrap inference for cross-regime tail index differences

This isolates structural tail changes beyond volatility scale effects.

(3) Regime-Weighted Predictive Density

A probabilistic Markov-switching mixture density is constructed:

f(rₜ₊₁) = (1 − pₜ) f_low + pₜ f_high

Filtered regime probabilities reshape the full predictive distribution, allowing tail curvature to vary endogenously.




3. Data

Sample: 1 January 1996 – 31 December 2025
6,268 aligned daily observations

Variables

SPY log returns (Yahoo Finance)

VIX index levels (Yahoo Finance)

3-month and 10-year Treasury yields (FRED)

Extreme left-tail events occur with unconditional frequency below 1%, but cluster strongly in stress regimes.




4. Regime Definitions

Three complementary classifications are used:

Exogenous Regimes (VIX Quantiles)

Low: 3,528 days

Mid: 1,320 days

High: 1,420 days

Extreme losses are several times more frequent in VIX-high states.

Endogenous Regimes (Two-State MS-GARCH)

Latent state inferred via maximum likelihood filtering (Hamilton filter).
Provides sharper separation of calm and stress periods than VIX classification.

Policy-Shock Indicator (Supplementary)

Monetary tightening / easing episodes defined via standardized rate changes.





5. Methodology
Baseline Model

Rolling Student-t GARCH(1,1):

One-step-ahead VaR forecasts

5,008 aligned out-of-sample days

Kupiec (UC) and Christoffersen (CC) backtests

Tail Diagnostics

Standardized residuals constructed from conditional volatility forecasts.
POT estimation applied to left-tail exceedances.

MS-GARCH

Two-state Markov-switching GARCH with regime-specific parameters and degrees of freedom.
Multiple random initializations used to mitigate local maxima.
State relabeling ensures state 2 corresponds to higher unconditional volatility.

Predictive Mixture

Regime-weighted Student-t density.
VaR and ES computed from the full mixture distribution via numerical inversion.




6. Empirical Results
6.1 Regime-Conditional VaR Failure

Under VIX-high regimes:

VaR95 ≈ 10–12% violation rate

VaR99 ≈ 3%+ violation rate

Low regimes remain near nominal.

Backtest rejection is driven by stress-period under-coverage, not uniform model failure.



6.2 Structural Tail Shift (POT–GPD)

Tail-event frequency (z < −3):

0.28% in VIX-low

1.90% in VIX-high (~6.7× increase)

GPD tail index estimates:

High regime: ξ ≈ 0.62 (95% CI [0.51, 0.83])

Bootstrap rejects equality of ξ across regimes

Stress states exhibit structural tail thickening beyond variance scaling.



6.3 Regime-Mixture Calibration

Mixture density reduces high-regime VaR95 violations:

11.84% → 10.68%

Improvement is stronger at 95% than 99%.
Extreme far tails remain statistically difficult due to data sparsity (~0.37% OOS tail frequency).




6.4 Transition Dynamics (Event Study)

Low→High regime switches show:

Immediate volatility level shift (persistent elevation)

Extreme losses spike sharply at transition date

Tail index does not shift discretely within short window

Logistic regression:

VIX-high increases odds of extreme event by ~6.9×

Policy shocks not significant

Regime entry reflects structural volatility change rather than isolated shock.





7. Interpretation

A single-state Student-t specification imposes constant tail curvature.
When the data-generating process exhibits regime-dependent tail behaviour, no single heavy-tailed distribution can simultaneously match calm and stress periods.

Regime-weighted mixture densities partially resolve this by allowing predictive shape to vary probabilistically.

This supports a structural distributional explanation of VaR breakdown.





8. Limitations

Two-state structure may be coarse

Constant transition probabilities

Extreme quantile estimation limited by low tail frequency

Risk-neutral pricing extension not implemented




9. Reproducibility
Requirements
pip install numpy pandas scipy statsmodels arch scikit-learn matplotlib seaborn fredapi yfinance
Notebook Order

Data download and alignment

Baseline GARCH backtest

MS-GARCH estimation

POT tail estimation

Regime-mixture forecast

Event study

All models use rolling windows with aligned one-step-ahead forecasts.
