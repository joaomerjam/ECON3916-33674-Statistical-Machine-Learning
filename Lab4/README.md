# Lab 4: Descriptive Statistics & Anomaly Detection
## Robustness in a Skewed World

A computational laboratory analyzing the California Housing Dataset to understand the limitations of traditional statistics and implement modern anomaly detection techniques.

---

## Objective

Move beyond trusting the average as a reliable metric. In skewed, real-world data (marketplaces like Airbnb, Zillow), the mean becomes a "vanity metric" that obscures economic reality.

**Progression:**
1. **Manual Statistical Forensics** — Identify outliers using the IQR/Tukey Fence method
2. **Algorithmic Anomaly Detection** — Use Isolation Forest ML to detect multivariate anomalies

---

## Dataset

**California Housing Dataset** (via `sklearn.datasets`)

Key feature: `MedHouseVal` (Median House Value) is capped at 5.0 ($500k), creating an artificial "ceiling effect" common in tech/marketplace data.

---

## Requirements

```python
pandas
seaborn
matplotlib
scikit-learn
```

---

## Lab Phases

### Phase 1: Setup & Inspection
Load data and visualize the ceiling effect in house values.

### Phase 2: Manual Statistical Forensics
Implement the **1.5 × IQR Rule (Tukey Fence)** to flag univariate outliers in median income.

```python
def flag_outliers_iqr(df, column):
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    return (df[column] < lower_bound) | (df[column] > upper_bound)
```

### Phase 3: Algorithmic Anomaly Detection
Use **Isolation Forest** to detect multivariate anomalies—patterns the IQR method misses (e.g., low income but 10 bedrooms).

Features analyzed: `MedInc`, `HouseAge`, `AveRooms`, `AveBedrms`, `Population`

### Phase 4: Comparative Forensics Report
Generate a robustness report comparing:
- Mean vs. Median (the "Inequality Wedge")
- Standard Deviation vs. MAD (Median Absolute Deviation)
- Distribution visualizations for normal vs. outlier populations

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Breakdown Point** | The proportion of outliers a statistic can handle before becoming unreliable |
| **Tukey Fence** | Q1 - 1.5×IQR to Q3 + 1.5×IQR defines the "normal" range |
| **Isolation Forest** | Unsupervised ML that isolates anomalies by their "few and different" nature |
| **MAD** | Median Absolute Deviation—robust alternative to standard deviation |

---

## Course

ECON 3916 — Financial Econometrics  
Northeastern University
