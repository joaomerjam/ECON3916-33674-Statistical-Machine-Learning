# The Architecture of Dimensionality: Hedonic Pricing & the FWL Theorem

## Objective

Construct and interrogate a multivariate hedonic pricing model on 2026 California real estate data, using the Frisch-Waugh-Lovell (FWL) theorem as both a diagnostic instrument and a formal proof of algorithmic *ceteris paribus* — the principle that a well-specified OLS estimator isolates each regressor's marginal effect by construction.

---

## Methodology

**Data & Environment**

- **Dataset:** 2026 California real estate metrics (Zillow synthetic) — `Sale_Price`, `Property_Age`, `Distance_to_Tech_Hub`
- **Stack:** Python 3.10+, `pandas`, `statsmodels.formula.api`, `matplotlib`

**Econometric Pipeline**

- **Bivariate baseline (OLS₁):** Regressed `Sale_Price` on `Property_Age` alone to establish a naïve coefficient estimate; this single-regressor model served as the omitted variable bias (OVB) benchmark.

- **Multivariate specification (OLS₂):** Extended the model to include `Distance_to_Tech_Hub` as a control, recovering the joint coefficient vector via the normal equations. The shift in the `Property_Age` coefficient between OLS₁ and OLS₂ quantified the magnitude of OVB induced by omitting proximity.

- **Manual FWL residualization (Stage 1 — partialling out X₂):** Regressed `Property_Age` on `Distance_to_Tech_Hub` and retained the residuals `ẽ₁` — the component of property age orthogonal to tech hub proximity, representing variation in age that cannot be explained by location.

- **Manual FWL residualization (Stage 2 — partialling out X₂ from Y):** Regressed `Sale_Price` on `Distance_to_Tech_Hub` and retained the residuals `ẽ_y` — the component of price orthogonal to proximity.

- **FWL auxiliary regression (OLS₃):** Regressed `ẽ_y` on `ẽ₁`. By the FWL theorem, the slope of this partialled regression must equal β̂₁ from the full multivariate OLS₂ exactly — a proposition verified numerically to machine precision.

- **Visualization:** Plotted coefficient trajectories, residual distributions, and the 3D regression hyperplane (via `plotly.graph_objects`) to render the geometry of dimensionality reduction tangible.

---

## Key Findings

**Omitted Variable Bias — Direction and Magnitude**

The bivariate model produced a severely distorted `Property_Age` coefficient: because newer construction is disproportionately concentrated near tech employment centers, age and proximity are negatively correlated in the sample. Omitting `Distance_to_Tech_Hub` caused the model to misattribute a substantial portion of the *location premium* to the physical recency of the structure — inflating the age coefficient in the direction of the omitted variable's influence.

**FWL as Algorithmic Ceteris Paribus**

The three-stage residualization confirmed the FWL theorem's central claim: after stripping the shared covariance between `Property_Age` and `Distance_to_Tech_Hub` from both the regressor and the outcome, the auxiliary regression slope converged to the multivariate OLS estimate with exact numerical precision. This is not a statistical approximation — it is an algebraic identity, and the manual proof demonstrates that OLS does not merely *control for* confounders conceptually; it mechanically orthogonalizes each regressor against the remaining covariate space before estimating its marginal effect.

**Interpretive Implication**

The fitted hyperplane makes this geometry legible in three dimensions: the coefficient on `Property_Age` represents the slope of the plane *along the age axis while holding distance constant* — a ceteris paribus slice through covariate space that the regression recovers automatically. Omitting any correlated regressor rotates the hyperplane, corrupting every remaining coefficient by an amount proportional to the omitted variable's explanatory power and its correlation with the included regressors.

---

*Lab completed under ECON 3916 — Financial Econometrics, Northeastern University. Manual FWL proof executed without computational shortcuts; all residual extraction performed step-by-step to demonstrate the theorem's algebraic foundations.*
