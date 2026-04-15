## The Sovereign Risk Engine — Regularization, Classification, and Model Evaluation for Macroeconomic Early Warning Systems

**Objective:** Built a next-generation IMF Early Warning System (EWS) for
identifying emerging-market economies at elevated sovereign crisis risk,
diagnosing OLS overfitting in high-dimensional WDI data, rebuilding the
forecasting engine with Ridge and Lasso regularization, constructing a
logistic regression crisis classifier, and evaluating it under real IMF
operational constraints using the full classification metrics toolkit.

---

### Methodology

**Data Pipeline**
- Downloaded 33 World Development Indicators via `wbgapi` for ~150 countries
  (2013–2019 averages), spanning macroeconomics, trade, finance, education,
  health, infrastructure, demographics, governance, and natural resources
- Applied 40% missingness thresholds for both countries and indicators;
  median-imputed remaining gaps following standard IMF/World Bank practice
- Constructed a binary crisis indicator (`crisis = 1` if average GDP per
  capita growth < 0) and performed a 70/30 train-test split with
  `StandardScaler` fit exclusively on training data to prevent leakage

**Phase 1 — Regularization**
- Demonstrated OLS overfitting via the Train–Test R² gap and p/n ratio
  analysis; confirmed variance explosion at high predictor-to-observation ratios
- Applied `RidgeCV` and `LassoCV` with 5-fold cross-validation and log-spaced
  alpha grids; produced a three-model comparison table (λ*, non-zero
  predictors, Training R², Test R², Test RMSE)
- Computed the full Lasso coefficient path with `lasso_path`; visualized
  all coefficient trajectories as a function of log(λ), colored by selection
  status at CV-optimal λ*; identified the first-entering predictor as the
  strongest unconditional growth signal in the WDI feature set

**Phase 2 — Classification**
- Exposed the Linear Probability Model's fundamental failure: predicted
  probabilities outside [0, 1] on the test set, with out-of-bounds
  predictions quantified and visualized
- Fitted `LogisticRegression` on Lasso-selected features; reported
  coefficients, odds ratios sorted by magnitude, and verified sigmoid
  boundedness via `.predict_proba()`
- Produced side-by-side LPM vs. logistic plots with impossible-region
  shading and marginal effect curves along the strongest predictor

**Phase 3 — Operational Evaluation**
- Demonstrated the accuracy paradox: naïve "no crisis" baseline achieves
  high accuracy with zero recall, exposing accuracy as a misleading metric
  under class imbalance
- Generated confusion matrix with `ConfusionMatrixDisplay` and full
  `classification_report`; extracted TP, FP, FN, TN individually
- Plotted ROC and Precision-Recall curves side by side with AUC annotations;
  explained why PR-AUC is the more informative metric for rare-event detection
- Swept thresholds from 0.01 to 0.99; identified the capacity-constrained
  operating point (≤5 missions/quarter) and F1-optimal point; plotted
  Precision, Recall, and F1 as functions of τ with both thresholds annotated

**Phase 4 — AI Context Engineering (P.R.I.M.E. Framework)**
- Designed and executed a P.R.I.M.E. prompt for bootstrap Lasso stability
  analysis: 200 resamples, selection frequency per predictor, horizontal
  bar chart colored by stability tier (stable >80%, fragile <30%)
- Designed and executed a P.R.I.M.E. prompt for cost-sensitive threshold
  optimization: swept expected cost (FN×$50B + FP×$2M) across all thresholds,
  identified cost-minimizing τ, and produced a three-way comparison table
  against the F1-optimal and capacity-constrained operating points

---

### Key Findings

OLS achieved near-perfect training fit (R² ≈ 1.0) but collapsed on the test
set, confirming variance explosion at the dataset's elevated p/n ratio.
RidgeCV and LassoCV both closed the generalization gap materially; Lasso
additionally performed automatic variable selection, retaining a sparse subset
of the strongest cross-country growth predictors.

The Lasso Path revealed that the first-entering predictor — the one surviving
the most aggressive penalty — carries the dominant unconditional growth signal
in the WDI feature space. Bootstrap stability analysis (200 resamples) further
partitioned predictors into robust anchors and correlation-induced substitutes,
confirming that low selection frequency reflects shared signal among correlated
development proxies rather than economic irrelevance.

The logistic classifier resolved the LPM's out-of-bounds failure by construction.
Under the IMF's asymmetric cost structure (FN:FP cost ratio of 25,000:1), the
cost-minimizing threshold sat well below the F1-optimal threshold — because F1
implicitly treats missed crises and false alarms as equally costly, an assumption
the IMF's operational reality invalidates entirely. The recommended deployment
threshold prioritizes Recall to minimize expected systemic loss, paired with a
probability-rank triage protocol to respect the five-mission staffing constraint.

---

### Tech Stack

`Python` · `wbgapi` · `scikit-learn` · `statsmodels` · `pandas` · `numpy` ·
`matplotlib` · `seaborn` · `Google Colab`
