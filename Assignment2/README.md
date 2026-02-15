# Audit 02: Deconstructing Statistical Lies

## Overview
Conducted forensic statistical audits on three portfolio companies claiming perfect metrics. Uncovered hidden biases using robust statistics, Bayesian inference, and hypothesis testing.

## Key Findings

### 1. Latency Skew (NebulaCloud)
- **Claim:** Mean latency of 35ms
- **Issue:** 2% of requests exceeded 2000ms (extreme outliers)
- **Method:** Compared Standard Deviation vs Median Absolute Deviation (MAD)
- **Result:** SD was inflated by outliers; MAD revealed true stability

### 2. False Positive Paradox (IntegrityAI)
- **Claim:** 98% accurate plagiarism detector
- **Issue:** In low-prevalence settings (0.1% base rate), only 4.7% of flagged students were actual cheaters
- **Method:** Bayesian inference to calculate P(Cheater|Flagged)
- **Result:** High accuracy meaningless without considering base rates

### 3. Sample Ratio Mismatch (FinFlash)
- **Claim:** Successful A/B test
- **Issue:** 500 users missing from treatment group
- **Method:** Chi-Square Goodness of Fit Test
- **Result:** χ² = 25.0 >> 3.84, indicating invalid experiment due to engineering bias

### 4. Survivorship Bias (Crypto Markets)
- **Issue:** Analyzing only listed tokens inflates mean market cap by 8-10x
- **Method:** Simulated 10,000 token launches using Pareto distribution
- **Result:** 98.6% of tokens fail but are excluded from analysis, creating false impression of success

## Technical Skills
- Robust Statistics (MAD calculation)
- Bayesian Inference
- Hypothesis Testing (Chi-Square)
- Monte Carlo Simulation
- Python (NumPy, Pandas, Matplotlib)
