High-Dimensional GDP Growth Forecasting with Lasso and Ridge Regularization
Objective
Forecast 5-year average GDP per capita growth rates across 120+ countries using 36 World Development Indicators, demonstrating the OLS overfitting failure mode in high-dimensional cross-sectional settings and applying Lasso and Ridge regularization with cross-validated penalty selection to recover out-of-sample generalization.
Methodology

Downloaded 36 WDI indicators live via the wbgapi API covering trade, macroeconomics, education, infrastructure, health, finance, natural resources, agriculture, and governance for ~150 countries over 2013–2019; averaged across years to construct a clean cross-sectional dataset
Applied a 60% missingness threshold to drop data-sparse countries and indicators, then imputed remaining gaps with cross-country medians — standard practice in cross-country empirics
Demonstrated the OLS failure mode: with a predictor-to-observation ratio approaching 0.5, OLS achieves near-perfect training fit while collapsing on held-out data, quantified via the train–test R² gap
Implemented RidgeCV and LassoCV inside a standardized sklearn pipeline with log-spaced penalty grids and 5-fold cross-validation to select optimal λ
Visualized the full Lasso Path using sklearn.linear_model.lasso_path, identifying which indicators enter the model first as the penalty relaxes — those most robustly associated with growth conditional on all other predictors
Built an interactive Streamlit dashboard allowing real-time exploration of coefficient shrinkage, the Lasso Path, and the train–test R² tradeoff across any λ value and outcome variable

Key Findings
OLS achieved a training R² near 1.0 but a substantially degraded test R², confirming variance explosion at high p/n ratios. Both RidgeCV and LassoCV closed the train–test gap materially. Lasso selected a sparse subset of 5–10 predictors from 30+, with the Lasso Path revealing that indicators entering at high penalty — those surviving the most aggressive shrinkage — represent the most robust cross-country growth signals. Zeroed-out predictors were interpreted as conditionally redundant given the correlation structure of WDI indicators, not as economically irrelevant, a distinction with direct implications for World Bank policy analysis.
Tech Stack
Python · wbgapi · scikit-learn · pandas · numpy · matplotlib · Streamlit
