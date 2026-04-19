FedSpeak Analysis — NLP on FOMC Minutes
Objective: Applied natural language processing to 20+ years of Federal Reserve meeting minutes to quantify shifts in monetary policy tone, extract sentiment signals using a finance-specific lexicon, and identify distinct language regimes through unsupervised clustering.
Methodology:

Loaded FOMC meeting minutes from the HuggingFace vtasca/fomc-statements-minutes dataset, filtering to full meeting minutes and parsing chronologically across a 2000–2026 date range
Preprocessed raw text through a standard NLP pipeline: lowercasing, non-alphabetic character removal, stop word filtering, and WordNet lemmatization via NLTK
Constructed a TF-IDF document-term matrix (unigrams + bigrams, min_df=5, max_df=0.85, max_features=2000) to represent each meeting as a sparse numerical vector
Computed Loughran-McDonald sentiment scores for each document — net sentiment (positive minus negative word frequency, normalized by document length) and an uncertainty score — using a finance-domain lexicon calibrated for central bank and SEC filing language
Reduced the TF-IDF matrix to 50 dimensions via Truncated SVD, then applied K-Means (K=3) to identify distinct language regimes; evaluated cluster quality with silhouette analysis
Compared pre-COVID (before March 2020) and post-COVID sentiment distributions using overlaid kernel density plots

Key Findings:

K-Means clustering identified three distinct language regimes in Fed communication: Cluster 0 (79 documents, March 2000 – October 2008) capturing the pre-crisis expansion vocabulary; Cluster 1 (134 documents, December 2008 – March 2026) reflecting the post-GFC era of crisis response, forward guidance, and tightening language that has dominated Fed communication ever since; and Cluster 2 (27 documents, spanning 2000–2022) isolating a transitional regime of mixed policy language — the silhouette score of 0.175 is consistent with expectations for high-dimensional text data where boundaries between regimes are gradual rather than discrete
The most negative meeting in the dataset was December 2008 — the month the Fed cut rates to the zero lower bound at the height of the financial crisis — and the most positive was June 2004, during the mid-2000s expansion; this passes a strong face validity test for the LM sentiment measure
Post-COVID minutes showed a meaningful decline in net sentiment relative to the pre-COVID baseline (0.0028 vs. 0.0081), reflecting the Fed's shift toward more cautious, negatively-toned language around inflation, supply disruptions, and aggressive rate tightening; uncertainty scores were marginally lower post-COVID (0.0212 vs. 0.0219), suggesting the Fed became more declarative and less hedged as it committed to a clear tightening path after 2022
