Causal ML — Double Machine Learning for 401(k) Policy Evaluation
Objective: Applied Double Machine Learning to the canonical 401(k) pension eligibility dataset to estimate the causal effect of retirement plan access on household net financial assets, while demonstrating the pitfalls of naive regularization-based approaches to causal inference.
Methodology:

Simulated a high-dimensional data generating process (n=5,000, p=100 covariates) with a known true ATE of $5.00 to demonstrate that naive LASSO shrinks the treatment coefficient toward zero due to regularization bias, producing a biased causal estimate
Constructed a DoubleML Partially Linear Regression model on 9,915 observations from the Chernozhukov & Hansen 401(k) dataset, using Random Forest nuisance learners (200 trees, max depth 5) with 5-fold cross-fitting for both the outcome model (net financial assets) and the propensity model (eligibility)
Estimated the Average Treatment Effect of 401(k) eligibility on net total financial assets, controlling for income, age, education, family size, marital status, and other household covariates
Conducted CATE analysis by splitting the sample into four income quartiles and fitting separate DML models on each subgroup to detect treatment effect heterogeneity
Ran a sensitivity analysis using the DoubleML robustness value framework to assess exposure to unmeasured confounding

Key Findings:

The full-sample ATE came out at -$867 (95% CI: -$1,810 to $76, p=0.072), which is not statistically significant and runs counter to the established literature — this is attributable to a bad control problem: p401 (actual 401k participation) was included as a covariate despite being a downstream outcome of eligibility, absorbing the treatment channel and biasing the estimate toward zero and negative
CATE analysis showed negative point estimates across Q2 through Q4 (-$1,546, -$865, and -$1,658 respectively) with wide confidence intervals spanning zero, and a small positive estimate for Q1 ($384) — all consistent with the bad control contaminating results across subgroups rather than reflecting genuine treatment effect heterogeneity
The sensitivity analysis returned a robustness value of 1.48%, indicating the estimate is highly sensitive to even minor unmeasured confounding, which further reflects the instability introduced by the model misspecification rather than a credible causal finding
This lab reinforced a core lesson in causal ML: getting the variable assignment right (outcome, treatment, and covariates) matters more than the sophistication of the ML model itself — a correctly specified OLS would outperform a misspecified DML
