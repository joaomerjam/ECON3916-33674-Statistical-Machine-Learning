# Hypothesis Testing & Causal Evidence Architecture

> *"The goal is not to find a signal. The goal is to determine whether a signal deserves to exist."*

---

## Objective

Most analytical workflows stop at estimation — computing a mean, fitting a model, reporting a coefficient. This project operates one layer deeper: **falsification**. The core question is not *"what is the treatment effect?"* but *"can we statistically rule out the possibility that this effect is noise?"*

Using the **Lalonde (1986) experimental dataset** — the canonical benchmark of causal inference — I operationalized the scientific method to adjudicate between two competing narratives: that a job training program meaningfully lifted real earnings, or that the observed difference was indistinguishable from random variation. This is the pivot from Estimation to **Proof by Statistical Contradiction**.

---

## Technical Approach

**Parametric Testing — Welch's T-Test (Signal-to-Noise Ratio)**
- Framed the treatment effect as a signal-to-noise problem: the difference in group means (signal) scaled by pooled standard error (noise), yielding the T-statistic.
- Applied **Welch's T-Test** (`scipy.stats.ttest_ind`, `equal_var=False`) rather than Student's T to avoid the assumption of equal variances across treated and control groups — a critical design choice given known heterogeneity in earnings distributions.
- Controlled for **Type I error** (false positive rate) by setting α = 0.05, establishing the decision threshold prior to analysis to prevent post-hoc threshold manipulation.

**Non-Parametric Validation — Permutation Test (10,000 Resamples)**
- Earnings data is notoriously heavy-tailed and zero-inflated, violating the normality assumptions that underpin classical inference. To stress-test the T-test result, I constructed an **empirical null distribution** via permutation testing (`scipy.stats.permutation_test`, 10,000 resamples).
- The logic: if treatment labels are meaningless, randomly shuffling them should produce a distribution of differences centered at zero. The observed statistic is then evaluated against this simulated null — no distributional assumptions required.
- Convergence between the parametric p-value and the permutation p-value confirms that the result is **robust to distributional violations**, not an artifact of Gaussian assumptions.

---

## Key Findings

The analysis identified a statistically significant Average Treatment Effect (ATE) of approximately **+$1,795 in real 1978 earnings**, rejecting the null hypothesis of no treatment effect. Both the Welch's T-Test and the Permutation Test produced consistent p-values below α = 0.05, confirming the signal survives both parametric and non-parametric scrutiny.

---

## Business Insight: Hypothesis Testing as the Safety Valve of the Algorithmic Economy

In high-velocity data environments, the greatest risk is not failing to find a signal — it is **confidently acting on a false one**. A/B tests are run continuously, dashboards surface hundreds of metrics daily, and the combinatorial opportunity for spurious correlations scales with every new feature and experiment. Without a rigorous falsification framework, organizations fall into **data dredging**: iterating on noise, shipping regressions dressed as improvements, and compounding Type I errors across the experiment pipeline.

Rigorous hypothesis testing — with pre-registered thresholds, assumption validation, and non-parametric cross-checks — functions as the **safety valve** of this system. It is the institutional mechanism that separates a result worth acting on from a result that merely looks compelling. Companies like Netflix and Uber have internalized this at scale: decision thresholds are not sacred statistical conventions, they are **calibrated business parameters** that reflect the asymmetric cost of false positives versus false negatives in a given product context. A 95% confidence threshold for a medical intervention and a 90% threshold for a UI color test reflect different risk tolerances, not different standards of rigor.

The analyst who understands this distinction — who treats α not as a fixed ritual but as a deliberate design choice — is the one building evidence that organizations can actually trust.

---

*Dataset: Lalonde (1986) Experimental Subset | Tools: Python, SciPy, pandas, seaborn*
