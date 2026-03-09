# Spurious Correlation & Multicollinearity in Macroeconomic Time Series
> An econometric diagnostics study using FRED API data · Financial Econometrics

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-4051b5?style=flat-square)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=flat-square&logo=plotly&logoColor=white)
![FRED API](https://img.shields.io/badge/FRED_API-c8102e?style=flat-square)

---

## Overview

This project investigates a core pitfall of applied macroeconometrics: the tendency for non-stationary level variables to exhibit artificially inflated correlations that misrepresent true economic relationships. Using live data from the **Federal Reserve Economic Data (FRED) API** — CPI, unemployment, federal funds rate, industrial production, and M2 money supply — the lab demonstrates how raw level series generate spurious correlations and severe multicollinearity, then applies a principled transformation pipeline to recover the true underlying structural relationships.

---

## Methodology

### 1 · Data Collection & Baseline Correlation Visualization
- Retrieved monthly FRED series into a unified `pandas` DataFrame, aligning all series on a common date index
- Computed Pearson correlation matrices on **raw level data** and rendered annotated heatmaps with `seaborn` — exposing near-unity coefficients between structurally unrelated variables driven purely by shared trends

### 2 · Multicollinearity Diagnostics via VIF
- Applied **Variance Inflation Factor (VIF)** analysis using `statsmodels` to quantify redundancy in the raw feature space
- VIF scores routinely exceeded 10–100×, confirming a near-singular design matrix where standard OLS estimators are unreliable

### 3 · Stationarity Transformation — Year-over-Year Growth Rates
- Converted each level series to its **YoY percentage growth rate** via `pct_change(12)`, eliminating deterministic trends
- Post-transformation correlation matrices showed a dramatic collapse in off-diagonal values, demonstrating the inflation was entirely an artifact of shared drift

### 4 · Causal Structure Mapping with DAGs
- Constructed **Directed Acyclic Graphs (DAGs)** using `NetworkX` to encode theoretically motivated causal relationships (e.g., Fed Funds → CPI via transmission lag, CPI → Unemployment via the Phillips Curve)
- Contrasted DAG structure against raw correlation matrices to distinguish causal paths, confounded back-door paths, and purely spurious trend artifacts

---

## Key Findings

| Finding | Detail |
|---|---|
| Spurious correlation in raw levels | Pearson *r* between M2 and CPI approached **+0.95** — driven by shared trend, not economic co-movement |
| Near-singular design matrix | All raw predictors exceeded VIF threshold of 10, rendering OLS coefficients statistically unreliable |
| YoY transformation resolves inflation | Off-diagonal correlations collapsed to **±0.10–0.35** post-transformation; only structurally meaningful relationships persisted |
| DAGs surfaced confounding structure | Visual causal maps separated direct effects, back-door paths, and spurious associations that correlation matrices cannot distinguish |

---

## Interactive Dashboard

An interactive **Plotly heatmap dashboard** allows toggling between the raw-levels and YoY-transformed correlation matrices via a dropdown menu, making the spurious correlation effect immediately visible.

```bash
python fred_correlation_heatmap.py   # generates fred_correlation_heatmap.html
```

---

## Project Structure

```
├── data/
│   └── fred_pull.py              # FRED API data retrieval
├── notebooks/
│   └── analysis.ipynb            # Full analysis walkthrough
├── fred_correlation_heatmap.py   # Interactive Plotly dashboard
└── README.md
```

---

## Setup

```bash
pip install pandas seaborn statsmodels plotly networkx fredapi
```

```python
from fredapi import Fred
fred = Fred(api_key="YOUR_API_KEY")  # https://fred.stlouisfed.org/docs/api/api_key.html
```
