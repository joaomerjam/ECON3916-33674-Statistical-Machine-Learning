# Lab 5: The Architecture of Bias

## Objective

Investigated how the **Data Generating Process (DGP)** introduces systematic bias long before a model is trained. This lab moves upstream from algorithms to the data itself, demonstrating that biased sampling mechanisms silently corrupt every downstream inference — and that rigorous diagnostics can detect and correct them.

## Tech Stack

`Python` · `pandas` · `NumPy` · `SciPy (Chi-Square)` · `scikit-learn`

## Methodology

### 1 · Simple Random Sampling & the Variance Problem

Drew repeated simple random samples from the Titanic dataset and measured the variance of key class distributions across draws. This demonstrated empirically that **small SRS samples suffer from high sampling error**: any single draw can produce a class ratio far from the population truth, leading to models that learn artifacts of the sample rather than genuine structure.

### 2 · Stratified Sampling to Eliminate Covariate Shift

Replaced SRS with **stratified sampling** (via `sklearn.model_selection.StratifiedShuffleSplit`) to guarantee that each split preserves the population-level class distribution. By locking the covariate balance between training and test sets, stratification removes one of the most common — and most invisible — sources of **covariate shift**, ensuring that model evaluation reflects real-world performance rather than distributional luck.

### 3 · Sample Ratio Mismatch (SRM) Forensic Audit

Simulated an A/B experiment with a planned 50/50 allocation and an observed 450/550 split across 1,000 users. Applied a **Chi-Square goodness-of-fit test** (`scipy.stats.chisquare`) to formalize the intuition that this deviation is not natural variance:

| Metric | Value |
|---|---|
| χ² Statistic | 10.00 |
| p-value | 0.0016 |
| Verdict | **SRM Detected** (p < 0.01) |

A deviation of ~3.16σ from the expected ratio signals a broken randomization mechanism — typically a load-balancer misconfiguration, differential bot traffic, or an initialization bug that drops users from one arm. **Any estimated treatment effect from a misrandomized experiment is untrustworthy.**

---

## Theoretical Deep-Dive: Survivorship Bias & Ghost Data

### The Problem

Analyzing only successful Unicorn startups (e.g., sourced from TechCrunch funding announcements) produces **Survivorship Bias** — a selection distortion where the dataset conditions on the outcome variable. We observe the winners and implicitly treat their characteristics as causal drivers of success, when in reality thousands of startups shared those same traits and still failed. The failures are invisible precisely *because* they failed; they were never featured.

This is a textbook **selection-on-the-dependent-variable** problem. Any regression run on survivors alone will overestimate the impact of observed features (founding team pedigree, market size, initial traction) because the counterfactual — companies with identical features that died — is missing from the sample.

### The Ghost Data

The specific missing observations are the **failed and stalled startups** that shared the same observable covariates as the Unicorns but never reached a liquidity event. This ghost data includes companies that raised Seed or Series A rounds and subsequently shut down, pivoted into irrelevance, or were quietly acqui-hired. Without them, the data-generating process is truncated: we only see realizations where *Selection = 1*.

### The Fix: Heckman Correction (Two-Stage)

The **Heckman Selection Model** treats survivorship as a formal econometric selection problem:

**Stage 1 — Selection Equation (Probit):** Model the probability of a startup *surviving into the dataset* as a function of instruments — variables that predict selection (survival) but are plausibly excluded from the outcome equation. Examples include founder network density, geographic proximity to VC clusters, or macroeconomic funding-cycle timing. From this probit, compute the **Inverse Mills Ratio (λ)** for each observation — a monotone-decreasing function of the predicted probability of selection that quantifies how truncated the sample is at each point.

**Stage 2 — Outcome Equation (OLS):** Regress the outcome of interest (e.g., valuation at exit) on the substantive covariates **plus λ as an additional regressor**. The coefficient on λ corrects the bias introduced by non-random selection. If it is statistically significant, survivorship bias is confirmed and the corrected coefficients on the substantive variables now approximate what we would have estimated had we observed the full, untruncated population.

### Why It Matters

Without the Heckman correction (or an equivalent selection model), any "driver of Unicorn success" analysis is methodologically equivalent to studying only Medal of Honor recipients and concluding that bravery guarantees survival in combat. The architecture of the bias lives in the sampling mechanism, not the model — which is the central lesson of this entire lab.
