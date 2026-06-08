# Regime-Dependent Tail Risk and Regime-Switching Predictive Distributions

A quantitative market-risk project testing whether standard Student-t GARCH VaR models fail in stress periods because tail risk changes by regime, not only because volatility rises.

The project combines rolling GARCH backtesting, regime-conditional VaR diagnostics, Peaks-over-Threshold tail estimation, and Markov-switching regime probabilities to analyse stress-period Value-at-Risk failures in U.S. equity returns.

---

## 1. Executive Summary

This project studies a practical model-risk question:

> When a standard Student-t GARCH VaR model underestimates losses during market stress, is the failure explained only by higher volatility, or does the shape of the return distribution also change?

Using daily U.S. equity market data, the analysis finds that VaR breaches are concentrated in high-volatility regimes. This is important because the benchmark model already includes both volatility clustering and heavy-tailed Student-t innovations. The evidence therefore points to a regime-dependent tail-risk problem rather than a simple failure to forecast conditional variance.

The project evaluates this hypothesis through four linked components:

1. Rolling one-step-ahead Student-t GARCH VaR forecasts.
2. Regime-conditional VaR backtesting across calm and stress states.
3. Peaks-over-Threshold Generalized Pareto tail estimation by regime.
4. Regime-weighted predictive distributions using Markov-switching state probabilities.

The main empirical conclusion is that stress regimes exhibit materially higher downside tail risk. Extreme losses are more frequent in high-volatility states, and tail estimates indicate thicker left-tail behaviour during stress. A regime-weighted predictive distribution can improve the modelling framework by allowing the predictive density to change with the estimated market state, although far-tail calibration remains difficult because extreme observations are sparse.

This is a market-risk and model-validation case study. It is designed to demonstrate applied knowledge of VaR backtesting, volatility modelling, tail estimation, regime detection, and model-governance limitations.

---

## 2. Research Question

Standard GARCH models are widely used for conditional market-risk forecasting. A Student-t GARCH(1,1) model improves on Gaussian GARCH by allowing heavy-tailed shocks, but it still assumes that the innovation distribution has a fixed shape through time.

This project tests whether that assumption is too restrictive.

The central research question is:

> Does stress-period VaR under-coverage reflect regime-dependent tail thickening beyond volatility clustering?

The benchmark model uses:

* Rolling parameter estimation.
* One-step-ahead volatility forecasts.
* Student-t innovations.
* VaR95 and VaR99 backtesting.
* Out-of-sample evaluation.

The key diagnostic is not only whether the model fails, but where it fails. If violations are concentrated in high-volatility states, then a single unconditional tail shape may be inadequate for market-risk measurement.

---

## 3. Why This Matters for Market Risk

This project is relevant to front-office risk, market-risk methodology, and model validation because it focuses on the failure mode of a standard VaR model.

### Market-Risk Relevance

The project evaluates:

* One-step-ahead VaR forecasting.
* VaR95 and VaR99 breach behaviour.
* Regime-conditional failure rates.
* Stress-state tail-event concentration.
* Expected Shortfall from predictive distributions.
* Event behaviour around transitions into high-volatility states.

The main risk-management insight is that VaR breaches are not evenly distributed through time. They cluster in stress states, where the return distribution becomes more difficult to represent with a single-state model.

### Model-Risk Relevance

The benchmark GARCH model is treated as a model requiring validation rather than as a final answer.

The project uses:

* Kupiec unconditional coverage tests.
* Christoffersen conditional coverage tests.
* Regime-conditional VaR diagnostics.
* Tail-index estimation by state.
* Comparison between single-state and regime-sensitive predictive distributions.

The model-risk issue is that a Student-t GARCH model can appear sophisticated while still imposing constant tail curvature across calm and stressed markets.

### Stress-Testing Relevance

The project studies stress behaviour using both observable and latent regime definitions:

* VIX-based volatility regimes.
* Markov-switching volatility-state probabilities.
* Extreme left-tail event indicators.
* Tail-index estimates by regime.
* Event-study diagnostics around low-to-high regime transitions.

The stress-testing focus is distributional: the project asks whether the left tail changes shape in stress, not simply whether volatility rises.

---

## 4. Data

The analysis uses daily U.S. market data.

### Core Variables

| Variable                         | Source        | Role                                    |
| -------------------------------- | ------------- | --------------------------------------- |
| SPY adjusted close / log returns | Yahoo Finance | Main equity return series               |
| VIX index                        | Yahoo Finance | Observable volatility-regime proxy      |
| 3-month Treasury yield           | FRED          | Short-rate and policy-shock proxy       |
| 10-year Treasury yield           | FRED          | Yield-curve and macro-financial context |

### Notes

* SPY log returns are the main risk series.
* VIX is used to classify observable volatility regimes.
* Treasury yield changes are used for supplementary macro-policy diagnostics.
* Data are downloaded directly in the notebook.
* A FRED API key is required for the Treasury-yield components.

A more production-ready version should include cached data files or a fallback data-loading path so the notebook can be reproduced without live API access.

---

## 5. Methodology

### 5.1 Rolling Student-t GARCH Benchmark

The benchmark model is a rolling Student-t GARCH(1,1) model estimated on historical return windows.

For each out-of-sample date, the model produces one-step-ahead VaR forecasts at the 95% and 99% confidence levels.

The benchmark is evaluated using:

* Violation rates.
* Kupiec unconditional coverage tests.
* Christoffersen conditional coverage tests.
* Regime-conditional VaR failure rates.
* Comparison of calm and stress-state calibration.

The purpose of the benchmark is to provide a realistic but interpretable market-risk model against which regime-dependent methods can be assessed.

### 5.2 Observable Regimes Using VIX

The first regime classification uses VIX as an observable stress proxy.

The notebook separates market states into low, mid, and high volatility regimes. These regimes are then used to evaluate whether VaR breaches and extreme losses are concentrated in stress periods.

This provides a transparent, economically interpretable stress classification.

### 5.3 Tail-Risk Estimation Using POT-GPD

The project uses Peaks-over-Threshold Extreme Value Theory to estimate downside tail behaviour by regime.

The tail-risk workflow is:

1. Define extreme left-tail losses.
2. Estimate Generalized Pareto tail parameters above a loss threshold.
3. Compare tail estimates across regimes.
4. Use bootstrap confidence intervals to assess estimation uncertainty.

The aim is to test whether stress states are associated with thicker downside tails, not only larger conditional variance.

### 5.4 Markov-Switching Regime Probabilities

The project also estimates latent volatility-state probabilities using a two-state Markov-switching framework.

The states are interpreted as:

* State 1: lower-volatility / calmer market state.
* State 2: higher-volatility / stress market state.

The estimated probability of the high-volatility state is used as an input to the predictive distribution.

The current notebook implementation should be treated as a research implementation rather than a production-grade MS-GARCH library. It includes numerical stabilisation and approximation steps, which are useful for experimentation but should be documented carefully in model validation.

### 5.5 Regime-Weighted Predictive Distribution

The predictive density is represented as a regime-weighted mixture:

```text
f(r_{t+1}) = (1 - p_t) f_low(r_{t+1}) + p_t f_high(r_{t+1})
```

where:

* `p_t` is the estimated probability of the high-volatility state.
* `f_low` is the predictive density under the low-volatility state.
* `f_high` is the predictive density under the high-volatility state.

This approach changes the full predictive distribution as regime probabilities change. It is more flexible than applying a simple volatility multiplier because it allows the stress-state distribution to contribute directly to tail-risk forecasts.

---

## 6. Key Findings

The results support three main conclusions.

### 6.1 VaR Failures Are Regime-Dependent

The benchmark Student-t GARCH model performs better in calm periods than in stress periods. VaR violations are disproportionately concentrated in high-volatility regimes.

This suggests that the model’s average out-of-sample performance can hide important conditional failure patterns.

### 6.2 Stress Regimes Exhibit Thicker Downside Tails

Extreme left-tail events are more frequent in high-volatility regimes. POT-GPD tail estimation provides evidence that stress-period losses have heavier downside-tail behaviour than calm-period losses.

The interpretation is that market stress changes the shape of the loss distribution, not only its scale.

### 6.3 Regime Mixtures Are Useful but Not Sufficient

A regime-weighted predictive density is a more flexible risk model than a single-state Student-t GARCH benchmark. It can incorporate changing state probabilities and produce a predictive distribution that reacts to stress conditions.

However, the improvement should be interpreted cautiously. Far-tail calibration remains difficult because VaR99 and Expected Shortfall depend on very few extreme observations. Any reported improvement should be checked against the final exported backtesting table produced by the notebook.

---

## 7. Technical Stack

The project uses Python and standard quantitative-finance libraries:

* `numpy`
* `pandas`
* `scipy`
* `matplotlib`
* `statsmodels`
* `arch`
* `yfinance`
* `fredapi`
* `scikit-learn`
* `xgboost`

The live `requirements.txt` should be treated as the source of truth for the current repository environment.

---

## 8. Repository Structure

Current repository structure:

```text
regime-dependent-tail-risk-ms-garch/
│
├── README.md
├── requirements.txt
└── ms_garch_tail_risk_analysis.ipynb
```

The notebook includes the main analysis workflow and can generate additional outputs such as figures and tables when run locally.

If figures, tables, or reports are exported in a local run, they should either be committed to the repository or clearly described as generated artifacts rather than existing repository files.

---

## 9. How to Run

### 9.1 Install Dependencies

```bash
pip install -r requirements.txt
```

### 9.2 Run the Notebook

Open:

```text
ms_garch_tail_risk_analysis.ipynb
```

Then run the notebook cells in order.

The notebook workflow covers:

1. Downloading and aligning market and macro-financial data.
2. Computing SPY log returns.
3. Estimating rolling Student-t GARCH VaR forecasts.
4. Backtesting VaR95 and VaR99.
5. Defining VIX-based volatility regimes.
6. Estimating tail behaviour by regime using POT-GPD.
7. Estimating latent volatility-state probabilities.
8. Building regime-weighted predictive VaR estimates.
9. Running event-study and classification diagnostics.
10. Exporting summary tables where configured.

### 9.3 FRED API Note

Treasury-yield data require a FRED API key. If the key is not provided, the Treasury-yield and policy-shock parts of the analysis may not run.

For full reproducibility, a future version should add cached CSV data or a fallback loader.

---

## 10. Model Governance and Limitations

### 10.1 Two-State Regime Design

The project uses a two-state regime structure. This is interpretable but may be too coarse for real market-risk systems. A three-state framework could separate calm, elevated-risk, and crisis regimes.

### 10.2 Constant Transition Probabilities

The Markov-switching component uses constant transition probabilities. In production, transition probabilities may depend on VIX, liquidity indicators, macro variables, or market microstructure conditions.

### 10.3 Numerical Stability and Approximation

MS-GARCH-type models are numerically challenging. The notebook uses stabilisation and approximation steps to obtain usable regime probabilities. This is acceptable for a research project, but a production model would require stronger estimation controls, convergence diagnostics, and sensitivity tests.

### 10.4 Local Optima

Regime-switching likelihoods can be sensitive to starting values. A production-grade implementation should use multiple random initialisations, compare likelihood values, and report parameter stability.

### 10.5 Sparse Far-Tail Data

The far left tail contains very few observations. This limits the precision of VaR99, Expected Shortfall, and GPD tail-index estimates. Bootstrap confidence intervals help, but they do not remove the sample-size problem.

### 10.6 Single-Asset Scope

SPY is used as the main equity proxy. This improves interpretability but limits cross-asset generality. A stronger market-risk framework would test the method across equity indices, rates, FX, credit, and commodities.

### 10.7 Reproducibility

Live data downloads can create small changes in results because financial data may be revised, adjusted, or temporarily unavailable. For a fully reproducible public repository, the project should include cached data or versioned output tables.

### 10.8 Not a Production Risk Model

This project is for research and educational purposes. It is not investment advice and should not be treated as a production VaR model without additional validation.

---

## 11. Future Improvements

Possible extensions:

* Commit generated figures and tables to the repository.
* Add cached data files for deterministic reproducibility.
* Refactor the notebook into reusable Python modules.
* Add unit tests for VaR backtesting and tail-estimation functions.
* Compare against EGARCH, GJR-GARCH, and GAS volatility models.
* Estimate three-state regime specifications.
* Add time-varying transition probabilities.
* Extend the analysis to rates, FX, credit spreads, and commodities.
* Compare physical-measure tail forecasts with option-implied tail-risk measures.
* Add Basel-style traffic-light VaR backtesting.
* Add Expected Shortfall backtesting and stress-period ES diagnostics.

---

## 12. Disclaimer

This repository is a research project in quantitative market risk. It is intended to demonstrate empirical modelling, tail-risk diagnostics, and model-validation reasoning. It is not investment advice and is not a production trading or risk-management system.



