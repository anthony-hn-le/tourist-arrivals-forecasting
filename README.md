# Time Series Analysis of U.S. Monthly Tourist Arrivals and Spending

MATH 422 (Time Series Analysis) mid-semester project analyzing U.S. inbound tourism from 2000–2024: monthly international arrivals and monthly tourist spending ("tourist exports"). The project fits and compares function-of-time regression models and SARIMA models for each series, validates them with residual diagnostics and out-of-sample cross-validation, and forecasts a hypothetical 2020–2021 with no COVID-19 disruption.

## Data

- **Arrivals** — monthly international visitor arrivals to the U.S. by country/region of residence, Jan 2000–Jul 2024, from the ITA's I-94 Arrivals Program (`data/monthly_arrivals_by_country_2000_present.xlsx`, aggregated total used in `data/monthly_tourist_arrivals.csv`).
- **Spending** — monthly seasonally-adjusted travel exports (foreign visitor spending in the U.S.), Jan 1999–Jul 2024, from the ITA's International Travel Receipts and Payments Program (`data/monthly_exports_imports_balance.xlsx`, total used in `data/monthly_exports.csv`).
- Both series exclude data from January 2020 onward for model fitting, isolating pre-pandemic dynamics; full-history data is used only for descriptive plots.

## Method

1. Detrend/decompose each series with function-of-time regressions (linear, quadratic, seasonal dummies, cosine terms), comparing fit via adjusted R², AIC, BIC.
2. Test stationarity of raw series and residuals (ADF, Phillips-Perron, KPSS, ERS) and normality (Shapiro-Wilk, Anderson-Darling, Jarque-Bera, Lilliefors).
3. Fit SARIMA models guided by ACF/PACF behavior after differencing, selecting among candidates by AIC/BIC and residual diagnostics.
4. Cross-validate the leading SARIMA model by training through 2018 and forecasting 2019 against actual data.
5. Forecast 24 months ahead (2020–2021) under a counterfactual no-COVID scenario.

## Key Findings

- **Arrivals:** SARIMA(0,1,1)(0,1,1)[12] was the best-fitting model (lowest AIC/BIC) and validated well out-of-sample — the 2019 holdout forecast achieved R² = 0.97. Residuals are stationary and independent but not normal, so forecast intervals should be treated as indicative rather than exact.
- **Spending:** SARIMA(0,1,0)(0,0,1)[12] with drift was preferred on BIC grounds (a competing model with SAR(2) had marginally lower AIC but implausible 2-year seasonal dependence for a seasonally-adjusted series). Residuals again fail normality.
- Function-of-time models plateaued around adjusted R² ≈ 0.92 (arrivals: linear + seasonal + cosine; spending: quadratic) but consistently failed residual independence/normality/stationarity checks, motivating the move to SARIMA.
- Both series show a clear pre-pandemic upward trend and strong 12-month seasonality (summer peak, winter trough for arrivals).

## Repository Structure

```
analysis.qmd     # Quarto source: full analysis, code, and narrative
data/            # Raw and lightly-cleaned source data
report/          # Final paper (paper.pdf) and presentation slides (slides.pptx)
figures/         # Figures/tables referenced in the paper
```

## Reproducing

Requires [Quarto](https://quarto.org/) and R with the packages listed in `analysis.qmd`'s setup chunk (`astsa`, `forecast`, `tseries`, `urca`, `tidyverse`, `gganimate`, `gifski`, and others for diagnostics/tables). Render with:

```bash
quarto render analysis.qmd
```

## Author

Anthony Le — Denison University, MATH 422, Fall 2024
