# Architecting the Prediction Engine

## Objective
Transition the OLS framework from a tool of causal explanation into a disciplined prediction engine — engineering a multivariate regression model on cross-sectional real estate data to minimize out-of-sample loss and quantify algorithmic financial risk in production-relevant terms.

---

## Methodology

- **Data Sourcing & Scoping** — Ingested the Zillow ZHVI 2026 Micro Dataset, a modern cross-sectional snapshot of U.S. real estate market valuations, establishing a high-signal empirical foundation for predictive modeling.

- **Feature Engineering & Formula Specification** — Leveraged the `statsmodels` Patsy Formula API to declaratively specify the multivariate model structure, translating domain intuition about real estate valuation drivers into a reproducible, auditable regression formula.

- **OLS Model Estimation** — Fit a multivariate Ordinary Least Squares model via `statsmodels`, extracting coefficient estimates, standard errors, and model diagnostics to validate the integrity of the prediction engine prior to evaluation.

- **Train/Test Split & Out-of-Sample Evaluation** — Partitioned the dataset to enforce a strict separation between model training and performance evaluation, ensuring that reported metrics reflect genuine generalization rather than in-sample fit.

- **Loss Quantification via RMSE (USD)** — Computed the Root Mean Squared Error denominated in actual U.S. Dollars — converting an abstract statistical loss metric into a direct, interpretable measure of the model's average financial prediction error.

---

## Key Findings

The lab successfully reframed OLS not as an explanatory instrument but as a **predictive decision system**. By computing RMSE in dollar terms rather than abstract units, the model's error margin became directly legible as a measure of **algorithmic business risk** — the financial cost, per prediction, of deploying this engine in a real valuation workflow.

This shift in framing is consequential: a model with strong in-sample R² but an RMSE of $80,000 carries materially different operational implications than one with a modest R² and an RMSE of $22,000. The exercise established that model selection in applied econometrics must be governed by **loss function design and business context**, not statistical significance alone.

---

*Stack: Python · pandas · NumPy · statsmodels (Patsy Formula API) · Zillow ZHVI 2026 Micro Dataset*
