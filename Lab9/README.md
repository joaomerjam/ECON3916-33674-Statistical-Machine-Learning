# Recovering Experimental Truths via Propensity Score Matching

## Objective

Applied Propensity Score Matching (PSM) to an observational dataset to eliminate selection bias and recover a causal treatment effect estimate consistent with experimental ground truth.

## Methodology

- **Modeled Selection Bias:** Diagnosed the structural differences between treated and control units in the Lalonde observational dataset, where non-random program assignment produced a severely confounded baseline comparison.
- **Estimated Propensity Scores:** Trained a logistic regression model on pre-treatment covariates to estimate each unit's conditional probability of receiving treatment — collapsing the high-dimensional confounder space into a single balancing scalar.
- **Applied Nearest-Neighbor Matching:** Matched each treated unit to its closest control-group counterpart in propensity score space, constructing a pseudo-experimental sample with approximately balanced covariate distributions.
- **Estimated the Average Treatment Effect on the Treated (ATT):** Computed the matched difference-in-means as the causal effect estimate, benchmarking against both the naive observational estimate and the known RCT benchmark.

## Key Findings

The naive (unmatched) comparison of treated and control earnings yielded an estimate of **−$15,204** — a figure driven entirely by selection bias, as program participants were systematically drawn from lower baseline earnings groups. Following propensity score matching and covariate rebalancing, the recovered treatment effect converged to approximately **+$1,800**, closely replicating the experimental benchmark established by the original Lalonde (1986) randomized controlled trial.

This ~$17,000 swing between the naïve and matched estimates illustrates the severity of confounding in observational labor market data and validates PSM as an effective identification strategy when selection on observables is plausible.

---

*Tools: Python, Pandas, Scikit-Learn | Dataset: Lalonde (1986) Observational Subset*
