## Tree-Based Models — Random Forests

**Objective:** Benchmarked decision tree, Ridge regression, and Random Forest
regressors on the California Housing dataset to evaluate the performance gains
from ensemble methods and assess feature-level drivers of housing prices.

**Methodology:**
- Trained a single decision tree, Ridge regression (α-tuned), and Random Forest
  on 16,512 training observations across 8 features
- Tuned RF hyperparameters (`n_estimators`, `max_depth`, `max_features`) via
  GridSearchCV with cross-validation
- Compared MDI vs. permutation feature importance to diagnose high-cardinality
  bias in tree-based importance measures
- Extended to a classification task (above/below median price) and benchmarked
  RF AUC against logistic regression
- Built an interactive Plotly + ipywidgets dashboard to visualize how
  `n_estimators` and `max_features` affect Train/Test R² in real time

**Key Findings:**
- Random Forest achieved Test R² = 0.8051 vs. Ridge at 0.5759, confirming
  strong non-linear structure in the feature-price relationship
- The single decision tree perfectly fit training data (R² = 1.00) but
  generalized poorly (Test R² = 0.62), a clear variance problem
- `MedInc` ranked as the dominant predictor under both importance methods;
  geographic features (`Latitude`, `Longitude`) were systematically
  underweighted by MDI relative to permutation importance
- Test R² stabilized past ~100 trees, illustrating diminishing returns to
  additional estimators under bagging
