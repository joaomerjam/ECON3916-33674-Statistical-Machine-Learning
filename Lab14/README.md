# AI Capex Diagnostic Modeling

## Objective

Stress-test an OLS revenue model against structural violations — heteroscedasticity and multicollinearity — using 2026 Nvidia AI capital expenditure and deployment data, and restore inferential validity through HC3-robust standard error correction.

---

## Methodology

- **Data Preparation:** Ingested and structured 2026 Nvidia AI capex and deployment metrics using `pandas`, engineering features across capital expenditure tiers to support regression diagnostics.

- **Baseline OLS Estimation:** Fit a naive OLS model via `statsmodels` predicting AI software revenue from deployment and capex variables, establishing a pre-correction inferential benchmark.

- **Heteroscedasticity Diagnosis:** Applied the Breusch-Pagan and White tests to formally evaluate residual variance constancy; confirmed severe heteroscedasticity expanding monotonically across high-capex deployment tiers, visualised through residuals-vs-fitted plots constructed in `matplotlib` and `seaborn`.

- **Multicollinearity Assessment:** Computed Variance Inflation Factors (VIF) across all regressors to identify collinear predictor pairs within the deployment metric set.

- **HC3 Robust Correction:** Re-estimated the model using MacKinnon-White HC3 heteroscedasticity-consistent covariance estimators, replacing naive standard errors with leverage-penalised equivalents robust to non-constant residual variance.

- **Comparative Inference Analysis:** Benchmarked naive vs. robust coefficient tables side-by-side, isolating variables where inflated t-statistics had produced false confidence under the uncorrected model.

---

## Key Findings

The naive OLS model exhibited **severe heteroscedasticity concentrated in high capital expenditure tiers**, where residual variance expanded systematically with fitted values — a structural pattern consistent with scale-dependent noise in large-capex AI deployment environments. This violation caused the model to report **artificially compressed standard errors**, producing t-statistics and p-values that overstated the precision of several deployment metric coefficients.

Correcting via **HC3 robust estimators** appropriately widened standard errors for the affected regressors, recalibrating statistical significance to reflect the true uncertainty in the data-generating process. Coefficients that appeared significant under naive OLS did not uniformly survive this correction — underscoring the practical cost of ignoring heteroscedasticity in capital-intensive, high-variance technology datasets.

The diagnostic workflow demonstrates that **inferential credibility in AI investment modeling requires variance structure validation as a precondition**, not an afterthought, before drawing policy or allocation conclusions from OLS output.

---

*Tools: Python · pandas · statsmodels · matplotlib · seaborn*
