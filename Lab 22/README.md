Clustering World Economies with K-Means & PCA
Objective: Applied unsupervised machine learning to identify natural groupings among world economies using 10 World Bank development indicators, benchmarking algorithmic clusters against official income classifications to evaluate where GDP-centric labels align with — and diverge from — multidimensional development patterns.
Methodology:

Sourced 10 cross-country development indicators (GDP per capita PPP, life expectancy, infant mortality, primary enrollment, Gini index, energy use, internet access, trade openness, unemployment, urbanization) via the wbgapi World Bank API; retained 89 countries after dropping observations missing more than 3 of 10 indicators
Imputed remaining sparse values with column medians prior to modeling
Standardized all features with StandardScaler prior to clustering to prevent high-variance indicators (e.g. GDP/capita) from dominating Euclidean distance calculations
Fit K-Means (K=4, k-means++ initialization) and projected the 10-dimensional feature space to 2D via PCA for visualization; PC1 and PC2 captured 41.9% and 13.6% of total variance respectively (55.6% combined)
Selected optimal K using elbow method (WCSS) and silhouette analysis across K=2–10; silhouette score peaked at K=2
Cross-tabulated algorithmic cluster assignments against World Bank income group classifications (Low / Lower-Middle / Upper-Middle / High) across 54 matched economies
Extended the full pipeline to the California Housing dataset (~20,600 census tracts, 8 features), identifying 2 natural neighborhood archetypes by income, density, and geography

Key Findings:

Silhouette analysis identified K=2 as the statistically optimal partition for both the World Bank and California Housing datasets, suggesting that at a global scale, development outcomes are most cleanly separated into a high-income versus lower-income binary rather than four discrete tiers
Despite fitting K=4, cluster-to-income-group alignment was strongest at the extremes: Clusters 0 and 1 captured nearly all 35 High-income economies with minimal contamination, while Cluster 3 absorbed the majority of Low and Lower-Middle income countries; the greatest ambiguity appeared at the Lower-Middle / Upper-Middle boundary, consistent with the overlapping multidimensional profiles of emerging economies
PC1 alone explained 41.9% of total variance, confirming that global development outcomes are largely structured along a single dominant axis — consistent with the empirical development economics literature on convergence
California Housing extension confirmed pipeline generalizability: optimal K=2 clusters mapped onto a recognizable high-income coastal versus moderate-income inland divide with no dataset-specific tuning required
