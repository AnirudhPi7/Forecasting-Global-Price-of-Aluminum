# Forecasting Global Price of Aluminum

---

## Introduction
This project predicts monthly aluminum prices using an ARIMAX framework, explicitly modeling major external shocks such as COVID-19 and the Russia–Ukraine war via event dummies. The objective is to produce accurate, business-ready forecasts that support hedging, budgeting, and procurement decisions while remaining transparent and easy to maintain.

---

## Background
Aluminum is deeply tied to macro cycles and energy input costs: smelting is electricity-intensive, construction/transport demand drives usage, trade policy shifts alter flows, and USD strength affects affordability. We combine classical time-series modeling with exogenous “shock” indicators to reconcile trend behavior with punctuated regime shifts observed around 2020–2022. This balances statistical rigor (parsimonious ARIMA) with economic narrative (pandemic, stimulus rebound, and war).

---

## Problem Statement
The objective is to build a robust monthly forecaster for global aluminum prices that stays accurate and interpretable across regime shifts. We compare a parsimonious ARIMA baseline with an ARIMAX that adds short event dummies (COVID phases, Russia–Ukraine war) to capture structural shocks. Success means lower AIC/BIC, equal or better MAE/RMSE/MAPE, white-noise residuals, and economically sensible coefficients that stakeholders can trust for hedging, budgeting, and procurement.

---

## Dataset
Monthly global aluminum price series PALUMUSDM from FRED is used (Data Source: FRED). The training window spans Jan 2010 – Nov 2024, with the last five months held out for validation. The series shows a clear trend and structural breaks around COVID-19 (2020) and Russia–Ukraine (2022)—necessitating differencing and shock modeling.

---

## Data Preparation
Exploratory plots indicated no strong seasonality and no significant outliers, but pronounced non-stationarity. The initial ADF test fails to reject a unit root (ADF ≈ −2.35, p ≈ 0.16). After first differencing, stationarity is achieved (ADF ≈ −9.16, p ≪ 0.01). ACF shows slow decay pre-difference; post-difference diagnostics point to a compact ARMA structure with PACF behavior consistent with AR(1) and ACF consistent with MA(1).

---

## Model Selection
We specify an ARIMAX on the differenced series, which includes dummy variables for: COVID dip (Mar–Jun 2020), COVID surge/recovery (Aug 2020–Dec 2021), and Russia–Ukraine war (Jan–Apr 2022). We tune (p,d,q) via information criteria and forecast errors. Grid search favored ARIMA(1,1,1) as the base, with competitive AIC/forecast errors relative to neighboring orders. With dummies, the war-only specification emerged as the most parsimonious winner: lowest AIC/BIC and top-tier MAE/MAPE, marginally outperforming models with multiple COVID dummies. Representative results (AIC/MAE/RMSE/MAPE) showed ARIMA(1,1,1) ≈ 2127.8 / 78.35 / 103.67 / 3.14%, and the final war-dummy model around AIC 2126.6; MAE 78.22; RMSE 103.78; MAPE 3.13%. Notably, COVID dummy p-values were often not significant, whereas the war dummy provided consistent incremental value with minimal complexity.

---

## Diagnostics
Post-fit diagnostics confirm that first-differencing addressed non-stationarity and that the AR(1) + MA(1) structure adequately captured short-term dependence. Residual checks for autocorrelation, homoskedasticity, and normality were conducted; no systematic serial correlation remained, supporting the adequacy of the ARIMAX specification. Where dummy variables were included, residual behavior was at least as well-behaved as the baseline, with the war dummy specification offering the best information-criteria balance without overfitting.

---

## Results & Discussion
Economically, the results align with industry intuition: the pandemic’s initial dip and stimulus surge were largely absorbed by the differenced ARIMA dynamics, whereas the 2022 supply disruption left a clearer, short, high-magnitude signature that improves fit when modeled explicitly. Technically, ARIMA(1,1,1) + war dummy is an interpretable, maintainable choice that generalizes across evaluation modes.

---

## Conclusion & Future Scope
A parsimonious ARIMAX(1,1,1) with a single war dummy delivers strong, interpretable aluminum price forecasts on recent history. Future improvements include stress-testing alternative shock windows, adding energy price or USD index covariates, exploring structural breaks and regime-switching models, and benchmarking against state-space trend-seasonality baselines. These extensions can further enhance resilience to evolving macro conditions while preserving clarity for decision-makers.

---
