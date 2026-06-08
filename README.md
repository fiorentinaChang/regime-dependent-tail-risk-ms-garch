# Regime-Dependent Tail Risk and Regime-Switching Predictive Distributions

Evidence from two-state MS-GARCH, POT–GPD tail estimation, and regime-weighted predictive densities.

---

## 1. Executive Summary

This project studies whether Value-at-Risk (VaR) failures during market stress are caused by structural tail shifts rather than volatility clustering alone.

Using U.S. equity market data from 1996 to 2025, the analysis shows that rolling Student-t GARCH(1,1) models are reasonably calibrated in calm periods but systematically under-cover during high-volatility regimes. This is important because the baseline model already includes persistent volatility dynamics and heavy-tailed innovations.

The project then tests whether stress periods involve a change in tail shape, not only a change in volatility level. It combines three approaches: regime-conditional VaR backtesting, Peaks-over-Threshold Generalized Pareto tail estimation, and a two-state Markov-switching GARCH framework.

The main finding is that high-volatility regimes exhibit structural tail thickening. Extreme losses are more frequent and the estimated tail index is materially higher in stress states. A regime-weighted predictive mixture density partially improves VaR calibration, although far-tail risk remains difficult to model due to sparse extreme observations.

The project is intended as a market-risk and model-risk case study. It focuses on VaR failure diagnostics, stress-regime identification, tail-risk measurement, and predictive distribution design rather than return forecasting.

---

## 2. Research Question

Standard rolling Student-t GARCH(1,1) models produce one-step-ahead VaR forecasts using:

* Persistent volatility dynamics.
* Fixed heavy-tailed innovations.
* Rolling parameter estimation.
* Conditional volatility forecasts.

In calm regimes, VaR coverage is close to nominal levels. In high-volatility regimes, however, the baseline model materially under-covers:

| Regime / Metric   | Observed Violation Rate | Nominal Level | Interpretation                              |
| ----------------- | ----------------------: | ------------: | ------------------------------------------- |
| High-regime VaR95 |                  11.84% |         5.00% | Significant under-coverage                  |
| High-regime VaR99 |                   3.67% |         1.00% | Far-tail under-coverage                     |
| Low-regime VaR    |            Near nominal |  Target level | Baseline model works better in calm periods |

The baseline model already embeds:

* Strong volatility persistence, with α + β approximately 0.993.
* Heavy-tailed Student-t innovations, with degrees of freedom approximately 6–8.

This means the miscalibration is unlikely to be explained by volatility clustering alone.

**Central hypothesis:** market stress regimes involve structural tail thickening, meaning a change in tail curvature beyond simple variance scaling.

---

## 3. Risk Management Relevance

This project is relevant to market risk, model risk, and stress testing because it investigates when a standard VaR model fails, why it fails, and how regime-dependent predictive distributions can partially improve risk calibration.

### Market Risk

The project evaluates market risk through:

* One-step-ahead VaR forecasting.
* VaR95 and VaR99 violation analysis.
* Regime-conditional VaR backtesting.
* Expected Shortfall estimation from predictive distributions.
* Stress-period tail-event frequency.
* Event-study analysis around regime transitions.

The key market-risk result is that VaR breaches are not uniformly distributed through time. They are concentrated in high-volatility regimes, where the baseline model materially underestimates tail risk.

### Model Risk

The project treats the baseline GARCH model as a risk model requiring validation. Model risk is assessed through:

* Out-of-sample VaR backtesting.
* Kupiec unconditional coverage tests.
* Christoffersen conditional coverage tests.
* Regime-conditional failure rates.
* Tail-index estimation by regime.
* Comparison between single-state and regime-switching predictive distributions.

The analysis shows that a single-state Student-t model can be misspecified even when it includes heavy-tailed innovations, because it imposes constant tail curvature across calm and stress regimes.

### Stress Testing

Stress behaviour is analysed using:

* VIX-based exogenous volatility regimes.
* Endogenous Markov-switching GARCH regimes.
* Extreme left-tail event frequencies.
* Tail-index estimates under stress.
* Event studies around low-to-high regime switches.
* Logistic regression of extreme-event indicators.

The stress-testing focus is not only whether losses increase during stress, but whether the shape of the loss distribution changes.

---

## 4. Data

The sample covers U.S. market data from **1 January 1996 to 31 December 2025**.

The aligned dataset contains approximately **6,268 daily observations**.

### Variables

| Variable                          | Source        | Use                                        |
| --------------------------------- | ------------- | ------------------------------------------ |
| SPY adjusted prices / log returns | Yahoo Finance | Main equity return series                  |
| VIX index                         | Yahoo Finance | Exogenous volatility-regime classification |
| 3-month Treasury yield            | FRED          | Monetary policy shock proxy                |
| 10-year Treasury yield            | FRED          | Yield-curve and macro-financial context    |

### Data Notes

* SPY log returns are the main risk series.
* VIX is used to classify exogenous low, mid, and high volatility regimes.
* Treasury yield data are used for supplementary policy-shock analysis.
* Extreme left-tail events have unconditional frequency below 1%, but they cluster strongly in stress states.

### FRED API Requirement

The notebook uses FRED Treasury yield data. To reproduce the full analysis, users need a free FRED API key.

When the notebook prompts for a FRED key, enter the key securely. A production version should include a cached CSV fallback so the notebook can run without requiring an API key.

---

## 5. Key Results

### 5.1 Summary of Main Findings

| Result Area                      |                                     Finding | Interpretation                                             |
| -------------------------------- | ------------------------------------------: | ---------------------------------------------------------- |
| High-regime VaR95 violation rate |                        11.84% vs 5% nominal | Baseline Student-t GARCH materially under-covers in stress |
| High-regime VaR99 violation rate |                         3.67% vs 1% nominal | Far-tail under-coverage remains severe                     |
| Tail-event probability           | 0.28% in low regime to 1.90% in high regime | Extreme losses cluster in stress states                    |
| High-regime tail index           |                                    ξ ≈ 0.62 | Stress regimes have materially thicker tails               |
| High-regime tail-index CI        |           95% CI approximately [0.51, 0.83] | Bootstrap evidence supports tail thickening                |
| Mixture VaR95 improvement        |                            11.84% to 10.68% | Regime mixture partially improves calibration              |
| Mixture VaR99 improvement        |                              3.67% to 3.25% | Far-tail improvement is smaller                            |
| Main limitation                  |        Extreme tail observations are sparse | Far-tail calibration remains difficult                     |

The key result is that volatility scaling alone is not sufficient. Stress regimes appear to involve changes in the survival-function shape of returns, not only higher conditional variance.

---

## 6. Regime Definitions

The project uses three complementary regime classifications.

### 6.1 Exogenous Volatility Regimes: VIX Quantiles

VIX-based regimes are used as an observable market-stress classification.

| Regime   | Description                              |
| -------- | ---------------------------------------- |
| Low VIX  | Calm market conditions                   |
| Mid VIX  | Normal/intermediate conditions           |
| High VIX | Stress or elevated-volatility conditions |

The high-VIX regime contains approximately **1,420 observations**, while the low-volatility regime contains approximately **3,528 observations**.

Extreme losses are several times more frequent in high-volatility regimes than in low-volatility regimes.

### 6.2 Endogenous Regimes: Two-State MS-GARCH

A two-state Markov-switching GARCH model is estimated with Student-t innovations.

The state process follows a first-order Markov chain. Each regime has its own volatility dynamics and tail parameter.

Each state has:

* Regime-specific GARCH parameters.
* Regime-specific Student-t degrees of freedom.
* Estimated transition probabilities.
* Filtered state probabilities from the Hamilton filter.

The high-volatility state is identified through post-estimation relabelling based on estimated conditional volatility.

The endogenous MS-GARCH state provides a sharper separation between calm and stress periods than VIX classification alone.

### 6.3 Monetary Policy Shock Indicator

A supplementary monetary policy shock indicator is constructed using standardized changes in short-term rates.

The indicator classifies:

* Tightening episodes.
* Easing episodes.

The policy-shock variable is used as a supplementary explanatory variable rather than as the main volatility-regime definition.

---

## 7. Methodology

### 7.1 Baseline Rolling Student-t GARCH

The baseline model is a rolling Student-t GARCH(1,1) model.

The model produces aligned one-step-ahead VaR forecasts over approximately **5,008 out-of-sample observations**.

The baseline model is evaluated using:

* VaR95 and VaR99 violation rates.
* Kupiec unconditional coverage test.
* Christoffersen conditional coverage test.
* Regime-conditional failure rates.
* Comparison across calm and stress states.

The main diagnostic is whether VaR failures are distributed evenly through time or concentrated in stress regimes.

### 7.2 POT–GPD Tail-Shift Quantification

The project uses an Extreme Value Theory framework to measure whether tail shape changes by regime.

The tail analysis uses:

* Peaks-over-Threshold method.
* Generalized Pareto Distribution.
* Regime-specific tail-index estimation.
* Bootstrap confidence intervals.
* Bootstrap test of equality of tail indices across regimes.

The key finding is that the estimated high-regime tail index is materially larger than the low-regime tail index. This supports the interpretation that stress regimes involve structural tail thickening.

### 7.3 Two-State MS-GARCH

The two-state MS-GARCH model is used to estimate latent volatility regimes.

The model is estimated in stages:

1. Gaussian MS-GARCH warm start.
2. Student-t likelihood refinement.
3. Post-estimation relabelling so that State 2 is the high-volatility regime.
4. Hamilton filtering to estimate regime probabilities.

The filtered probability of the high-volatility state is then used to construct regime-weighted predictive densities.

### 7.4 Regime-Weighted Predictive Mixture Density

Following the logic of regime-switching predictive distributions, the one-step-ahead return density is modelled as a probability-weighted mixture:

```text
f(r_{t+1}) = (1 - p_t) f_low(r_{t+1}) + p_t f_high(r_{t+1})
```

where:

* `p_t` is the filtered probability of the high-volatility state.
* `f_low` is the low-regime conditional Student-t density.
* `f_high` is the high-regime conditional Student-t density.

VaR and Expected Shortfall are computed directly from the predictive mixture distribution.

This approach changes the full predictive distribution rather than applying a simple volatility multiplier.

### 7.5 Event Study and Logistic Regression

The project also studies low-to-high regime transitions.

The event study examines:

* Predicted volatility around regime switches.
* Extreme losses around transition dates.
* Tail-index behaviour around transitions.
* Persistence of the high-volatility state after entry.

A supplementary logistic regression is used to test whether VIX-high states and monetary policy shocks are associated with extreme left-tail events.

---

## 8. Empirical Results

### 8.1 Regime-Conditional VaR Failure

In high-volatility regimes:

* VaR95 violation rate is approximately 10–12%.
* VaR99 violation rate is approximately 3% or higher.
* Low-volatility regimes remain closer to nominal coverage.

This shows that baseline GARCH failure is concentrated in market stress rather than evenly distributed across all periods.

### 8.2 Structural Tail Thickening

The tail analysis shows:

* Extreme losses are disproportionately concentrated in stress states.
* Tail-event frequency rises from approximately **0.28%** in the low regime to **1.90%** in the high regime.
* The high-regime tail index is approximately **ξ = 0.62**.
* Bootstrap inference rejects equality of tail indices across regimes.

This supports the conclusion that stress periods involve a change in tail curvature, not merely an increase in conditional volatility.

### 8.3 Mixture Model Improvement

The regime-weighted predictive mixture improves high-regime VaR calibration, but only partially.

| Metric                           | Baseline Student-t GARCH | Regime Mixture | Interpretation               |
| -------------------------------- | -----------------------: | -------------: | ---------------------------- |
| High-regime VaR95 violation rate |                   11.84% |         10.68% | Partial improvement          |
| High-regime VaR99 violation rate |                    3.67% |          3.25% | Smaller far-tail improvement |

The improvement is meaningful but incomplete. Far-tail calibration remains difficult because extreme observations are sparse.

### 8.4 Transition Dynamics

Low-to-high regime switches show:

* Immediate upward shift in predicted volatility.
* Persistent elevation after transition.
* Spike in extreme losses around transition dates.
* No clear discrete jump in the tail index within a short event window.

A supplementary logistic regression indicates that VIX-high regimes substantially increase the odds of an extreme event, while monetary policy shock indicators are not statistically significant in the same way.

This suggests that regime entry reflects broader volatility-state shifts rather than isolated policy shocks alone.

---

## 9. Interpretation

A single-state Student-t GARCH model assumes constant tail curvature across all market environments.

This is restrictive. If the data-generating process has regime-dependent tail shape, then one fixed heavy-tailed innovation distribution cannot simultaneously fit calm-period and stress-period losses.

The empirical evidence suggests that high-volatility regimes involve:

* Higher conditional variance.
* Higher extreme-loss frequency.
* Thicker left-tail curvature.
* Greater VaR under-coverage.

A regime-switching mixture density partially addresses this by allowing the predictive return distribution to change shape as filtered regime probabilities change.

The project therefore treats VaR failure as a model-specification issue, not only a volatility-forecasting issue.

---

## 10. Model Governance and Limitations

This section summarises the main modelling and implementation limitations.

### 10.1 Two-State Specification

The model uses two regimes: low-volatility and high-volatility.

This is interpretable, but it may be too coarse. A three-state model could distinguish calm, elevated-risk, and crisis regimes.

### 10.2 Constant Transition Probabilities

The MS-GARCH model uses constant transition probabilities.

In practice, transition probabilities may depend on macro-financial variables, liquidity conditions, VIX levels, or policy shocks.

### 10.3 Transition-Probability Regularisation

The MS-GARCH estimation uses numerical stabilisation to avoid degenerate transition estimates.

This improves estimation stability, but it may influence estimated regime persistence. A production version should report sensitivity to this regularisation and compare constrained and unconstrained estimates.

### 10.4 Local Optima in MS-GARCH Estimation

MS-GARCH likelihoods can be non-convex and sensitive to starting values.

The current implementation uses staged optimisation, including a Gaussian warm start followed by Student-t refinement. A more robust production implementation should use multiple random initialisations and compare likelihood values across candidate solutions.

### 10.5 Sparse Far-Tail Observations

The far left tail contains very few observations.

This limits the precision of VaR99, Expected Shortfall, and GPD tail-index estimates. Bootstrap confidence intervals help quantify uncertainty, but far-tail inference remains sample-limited.

### 10.6 Single Equity Proxy

The project uses SPY as the main equity risk series.

This improves interpretability but limits cross-asset generality. A production extension should test the method on equity indices, rates, FX, credit spreads, and commodities.

### 10.7 FRED API Dependency

The notebook requires a FRED API key for Treasury yield data.

This is acceptable for research, but a more reproducible public version should include a cached data option or a clear fallback path.

### 10.8 Computational Cost

Rolling GARCH and MS-GARCH estimation can be computationally expensive.

For demonstration runs, a larger step size can be used to reduce runtime. Final reported results should use aligned one-step-ahead forecasts.

### 10.9 Partial Calibration Improvement

The regime-mixture model improves high-regime VaR calibration but does not fully solve far-tail under-coverage.

This is an important result rather than a weakness to hide: regime-dependent predictive densities help, but extreme stress losses remain difficult to model.

### 10.10 Risk-Neutral Pricing Not Implemented

The project focuses on physical-measure predictive risk distributions.

It does not implement a risk-neutral pricing extension for derivatives or option-implied tail risk.

---

## 11. Reproducibility

## 11. Reproducibility

### 11.1 Repository Structure

```text
regime-dependent-tail-risk-ms-garch/
│
├── README.md
├── requirements.txt
├── report.pdf
├── ms_garch_tail_risk_analysis.ipynb
│
├── figures/
│   ├── var_violations_by_regime.png
│   ├── tail_xi_ci_by_vix_regime.png
│   ├── tail_xi_ci_by_ms_p2_bin.png
│   ├── regime_probability_timeseries.png
│   ├── mixture_vs_baseline_var.png
│   ├── event_study_transition_volatility.png
│   └── event_study_extreme_loss_rate.png
│
└── tables/
    ├── key_results_summary.csv
    ├── var_backtest_summary.csv
    ├── regime_conditional_var_failures.csv
    ├── tail_index_by_regime.csv
    ├── mixture_vs_baseline_var.csv
    ├── event_study_summary.csv
    ├── logistic_regression_summary.csv
    └── ms_garch_multistart_summary_stage2_only.csv
```

### 11.2 Install Requirements

Install the required packages using:

```bash
pip install -r requirements.txt
```

The `requirements.txt` file should contain one package per line:

```text
numpy
pandas
scipy
matplotlib
seaborn
statsmodels
arch
yfinance
fredapi
scikit-learn
xgboost
jupyter
ipykernel
```

### 11.3 Run the Notebook

Open and run:

```text
ms_garch_tail_risk_analysis.ipynb
```

Workflow:

1. Download and align SPY, VIX, and Treasury yield data.
2. Estimate rolling Student-t GARCH forecasts.
3. Backtest VaR95 and VaR99 forecasts.
4. Classify exogenous volatility regimes using VIX.
5. Estimate two-state MS-GARCH regimes.
6. Estimate regime-specific POT–GPD tail indices.
7. Construct regime-weighted predictive mixture densities.
8. Compare baseline and mixture-model VaR calibration.
9. Run event-study and logistic-regression diagnostics.

### 11.4 Runtime Note

The final reported results should use aligned one-step-ahead forecasts.

For faster demonstration runs, the notebook may use a larger rolling-estimation step size. If this is done, the README and notebook should clearly label the run as a computational shortcut rather than the final reported specification.

### 11.5 Data Source Note

This project uses public financial data from Yahoo Finance and FRED. Results may vary slightly if data are revised, adjusted, or unavailable at the time of download.

This project is for research and educational purposes only. It is not investment advice and is not a production market-risk model.

---

## 12. Future Improvements

Future extensions could make the project more robust and closer to a production-style market-risk framework.

Potential improvements include:

* Add cached data files for reproducibility without API keys.
* Run multiple random initialisations for MS-GARCH estimation.
* Compare two-state and three-state regime specifications.
* Test sensitivity to rolling-window length.
* Add rolling VaR and Expected Shortfall backtesting dashboards.
* Compare MS-GARCH against EGARCH, GJR-GARCH, and GAS models.
* Extend the analysis to multiple asset classes.
* Include liquidity stress indicators.
* Add regime-dependent transition probabilities using macro-financial covariates.
* Compare physical-measure tail forecasts against option-implied tail risk.
* Refactor the notebook into reusable Python modules.


