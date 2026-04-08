Fraud Detection Model Evaluation — Metrics that Matter
Objective
Rigorously evaluate a logistic regression fraud classifier on a severely imbalanced real-world dataset by moving beyond accuracy to confusion-matrix-derived metrics, threshold analysis, and capacity-constrained operating point selection.
Data
Kaggle Credit Card Fraud Detection dataset — 284,807 anonymized European credit card transactions (September 2013), with PCA-transformed features V1–V28, transaction Amount, and a binary fraud label. Positive class prevalence: 0.172%.
Methodology

Established a naïve baseline to demonstrate the accuracy paradox: a classifier that predicts "legitimate" for every transaction achieves 99.83% accuracy while catching zero fraud — exposing accuracy as a misleading metric on imbalanced data
Computed the full confusion matrix and derived class-specific Precision, Recall, and F1-Score, isolating performance on the minority (fraud) class
Plotted ROC and Precision-Recall curves to evaluate model discrimination across all thresholds; used PR-AUC as the primary metric given that ROC-AUC is inflated by the dominant negative class
Conducted threshold sensitivity analysis — scanning τ from 0.01 to 0.99 to map the precision-recall tradeoff and identify the F1-maximizing operating point, which diverges materially from the default 0.50 cutoff
Applied a capacity-constrained decision rule: identified the most aggressive threshold that flags no more than 500 transactions per day (one investigative team's daily bandwidth), achieving 88.78% Recall at τ = 0.01 with only 246 cases flagged — well within capacity

Key Findings
The logistic regression model, trained with balanced class weights, achieved strong discriminative performance on the fraud class despite a 580:1 class imbalance. The capacity constraint proved non-binding: the model's own confidence concentrated fraud probability scores sufficiently that the 500-investigation ceiling was never reached, leaving 254 daily investigation slots available for secondary review. The analysis confirmed that threshold selection is fundamentally a business optimization problem — the cost-minimizing operating point and the F1-maximizing operating point diverge when false negative costs asymmetrically exceed false positive costs, as is standard in fraud detection contexts.
Stack: Python · scikit-learn · pandas · NumPy · matplotlib · seaborn
