NY Fed Yield Curve Recession Model Replication
Objective
Replicated the Federal Reserve Bank of New York's yield curve recession probability model by fitting a logistic regression on FRED macroeconomic data, producing a historically validated monthly time series of recession risk 12 months ahead.
Methodology

Data acquisition: Pulled two FRED series via fredapi — T10Y3M (10-Year minus 3-Month Treasury yield spread, daily, resampled to month-end) and USREC (NBER binary recession indicator) — spanning 1970 to present
Feature engineering: Applied a 12-month lag to the yield spread, matching the NY Fed's published forecasting convention; dropped leading observations with no lag available
LPM failure demonstration: Fit an OLS Linear Probability Model as a baseline and documented out-of-bounds predictions (below 0 and above 1) on real FRED data, establishing the empirical motivation for logistic regression
Logistic regression: Fit an sklearn LogisticRegression model and a statsmodels Logit for inference; extracted the yield spread coefficient, odds ratio, and 95% confidence interval
Probability time series: Applied .predict_proba()[:, 1] to the full sample to generate the fitted recession probability curve with NBER recession shading
Model validation: Evaluated out-of-sample performance using TimeSeriesSplit 5-fold cross-validation, benchmarked against a naive always-predict-zero baseline
Extension: Fit a two-predictor logistic regression adding the 12-month change in UNRATE (unemployment rate); compared odds ratios across specifications to assess variable independence

Key Findings
The yield spread odds ratio of approximately 0.45 indicates that each additional percentage point of spread reduces recession odds by roughly 55%, consistent with the NY Fed's published model. The LPM produced logically incoherent predictions on actual data, confirming that probability outcomes require a bounded model. The 2022–2024 inversion — the deepest in the sample — drove predicted recession probabilities to ~40%, elevated but below the 50% decision threshold; no NBER recession materialized, illustrating that a well-calibrated probabilistic forecast is not invalidated by a single non-event at sub-majority probability. Adding unemployment change as a second predictor left the yield spread odds ratio nearly unchanged (0.454 → 0.460), suggesting the two variables capture orthogonal recession channels and the spread retains its full independent signal.
