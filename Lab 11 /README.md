# Data Wrangling & Engineering Pipeline

## Objective
Engineer a robust, analysis-ready dataset from a structurally compromised HR economics file by systematically resolving missingness, eliminating multicollinearity, and compressing high-cardinality features — enabling downstream econometric modeling with statistical integrity.

---

## Methodology

- **Missingness Diagnostics (MAR Analysis)**
  Leveraged `missingno` to visualize and classify the missingness structure across all features. Identified patterns consistent with **Missing At Random (MAR)** — where the probability of a missing value is conditionally dependent on observed covariates — ruling out MCAR and MNAR mechanisms and informing an appropriate imputation strategy.

- **Structural Imputation**
  Applied targeted imputation routines via `pandas` and `statsmodels`, anchoring fill logic to the identified MAR structure. Preserved distributional properties of the original data to avoid introducing systematic bias into model estimates.

- **Dummy Variable Trap Mitigation**
  One-hot encoded categorical variables while explicitly dropping a reference class for each feature to eliminate perfect multicollinearity. This ensured full-rank design matrices and numerically stable OLS estimation — a prerequisite for invertible **XᵀX** in linear regression contexts.

- **Target Encoding for High-Cardinality Geographies**
  Applied `category_encoders.TargetEncoder` to compress high-cardinality geographic variables (e.g., region/city-level identifiers) into a single continuous feature representing the conditional mean of the target. This avoided dimensionality explosion while retaining the predictive signal embedded in geographic variation.

---

## Key Findings

The pipeline successfully transformed a structurally chaotic HR dataset into a clean, model-ready design matrix. Three core econometric hazards were identified and resolved:

1. **Missingness was non-random but recoverable** — MAR classification confirmed that imputation conditioned on observed covariates was both valid and sufficient, avoiding the need for full multiple imputation frameworks.

2. **Multicollinearity was structural, not incidental** — The Dummy Variable Trap was preempted by systematic reference-class dropping, ensuring the resulting feature matrix was full-rank and estimation-ready for linear models.

3. **Geographic dimensionality was compressed without information loss** — Target Encoding reduced a high-cardinality nominal variable to a single interpretable continuous feature, balancing model parsimony with predictive coverage.

---

**Stack:** Python · pandas · statsmodels · missingno · category_encoders  
**Data:** `messy_hr_economics.csv`
