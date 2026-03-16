# Assignment 3: The Causal Architecture
### ECON 3916 — Financial Econometrics | Northeastern University

---

## Overview

In this assignment I took on the role of a Senior Data Economist at **SwiftCart Logistics**, a multinational on-demand delivery platform. The executive board had three open questions — driver compensation equity, whether a new routing algorithm actually works, and the real ROI of their premium subscription service. The goal was to throw out fragile parametric assumptions and use heavy computation to build real empirical evidence and isolate causality from noise.

---

## Phases

### Phase 1: Bootstrapping Non-Parametric Uncertainty

The tip distribution at SwiftCart is zero-inflated (40% of drivers got $0) and heavily right-skewed — the kind of data where the CLT breaks down and a standard confidence interval is useless. I simulated an audit sample of 250 driver tips and built a manual bootstrap engine from scratch, resampling 10,000 times to construct an empirical 95% CI for the median.

**Key Finding:** The bootstrap CI came out asymmetric — $0.49 below the median vs. $0.61 above it. A standard parametric CI would force equal tails, which would misrepresent the actual distribution. The wider upper tail correctly reflects the right-skew in the data.

---

### Phase 2: Falsification in Logistics A/B Testing

The engineering team claimed their new Batch Routing algorithm significantly cut delivery times. The problem: the treatment group had extreme upper-bound outliers from software crash loops, completely violating the homoscedasticity assumption of a standard t-test. I ran a manual permutation test instead — shuffling all 1,000 deliveries 5,000 times and computing an empirical p-value with no distributional assumptions required.

**Key Finding:** p-value = 0.0000. Not a single one of the 5,000 random permutations matched the observed difference of 2.265 minutes. The algorithm genuinely works — and we proved it without relying on any parametric assumptions the data clearly violates.

---

### Phase 3: Causal Control & Selection Bias Mitigation

The marketing team wanted to double the acquisition budget for SwiftPass based on a naive comparison showing subscribers spend **$17.57/month more** than non-subscribers. Classic selection bias — high-volume power users naturally self-select into the program to save on delivery fees, meaning they would've spent more regardless.

I built a full PSM pipeline to isolate the counterfactual:

1. **Propensity Score Estimation** — Logistic regression on pre-treatment covariates (pre-spend, account age, support tickets) to model the probability of subscribing
2. **Nearest Neighbor Matching** — Each subscriber matched 1:1 to the non-subscriber with the closest propensity score
3. **ATT Calculation** — Mean difference in post-spend between treated units and their matched controls

**Key Finding:**

| Estimate | Value |
|---|---|
| Naive SDO (Biased) | $17.57 |
| PSM ATT (Causal) | $10.02 |
| Bias Removed | $7.55 |

The real causal lift from SwiftPass is **$10.02/month** — not $17.57. Doubling acquisition spend based on the inflated figure would've anchored the entire ROI model to a number driven by selection bias, not the program itself.

---

### Bonus: Love Plot — Covariate Balance Diagnostics

Generated a Love Plot (Standardized Mean Differences) to visually confirm that PSM successfully eliminated selection bias across all covariates. All four features landed inside the |SMD| < 0.1 balance threshold after matching, with the propensity score itself collapsing to 0.000 — confirming the matched groups are structurally identical on all pre-treatment dimensions.

---

## Tools & Methods

- **Language:** Python (Google Colab)
- **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-Learn
- **Methods:** Bootstrap Resampling, Permutation Testing, Logistic Regression, Nearest Neighbor Matching, Propensity Score Matching (PSM), Standardized Mean Differences (SMD)

---

## Course

ECON 3916 — Financial Econometrics
Northeastern University
