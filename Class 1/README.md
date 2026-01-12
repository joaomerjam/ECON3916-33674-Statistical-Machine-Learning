🌍 Global Purchasing Power Parity Analysis via the Big Mac Index
Objective

To empirically test the Law of One Price by using the Big Mac Index as a real-world proxy for Purchasing Power Parity (PPP) and evaluating whether exchange rates correctly reflect relative price levels across countries.

Methodology

Collected and structured raw price and exchange rate data from The Economist’s 2015 Big Mac Index using Python dictionaries.

Converted the dataset into a Pandas DataFrame for systematic analysis.

Calculated Implied PPP exchange rates by comparing the local price of a Big Mac to its price in the United States.

Computed the percentage valuation or misvaluation of each currency relative to the U.S. dollar.

Interpreted deviations from PPP in terms of currency overvaluation and undervaluation, highlighting potential market inefficiencies.

Key Findings

The results show substantial deviations from Purchasing Power Parity across countries, indicating that exchange rates do not fully adjust to equalize price levels in the short run. Most currencies in the sample were undervalued relative to the U.S. dollar, with particularly large misalignments observed in emerging markets. For example, the Russian ruble appeared to be undervalued by nearly 70%, making it the most undervalued currency in the dataset. South Africa, Indonesia, Egypt, Hong Kong, and China also showed strong undervaluation, generally ranging between 40% and 50%, suggesting that local purchasing power was significantly higher than what nominal exchange rates implied.

Several developed economies exhibited smaller but still meaningful deviations from PPP. The Japanese yen, Mexican peso, Philippine peso, South Korean won, euro, Australian dollar, and British pound were all moderately undervalued, indicating mild currency misalignment rather than extreme distortions.

On the overvaluation side, Brazil stood out as slightly overvalued, at around 10%, implying that goods priced in reais were relatively expensive compared to the U.S. benchmark. The most striking result was Norway, whose krone appeared to be overvalued by roughly 30%, making it the most overvalued currency in the sample. This is consistent with Norway’s high income levels, strong currency, and higher domestic price structure.

Overall, these findings reinforce that while Purchasing Power Parity provides a powerful theoretical benchmark, real-world exchange rates deviate substantially due to structural factors such as differences in wages, productivity, taxes, market competition, and capital flows. The observed misalignments highlight where arbitrage opportunities may theoretically exist, though in practice they are constrained by transaction costs, trade frictions, and non-tradable components of goods like labor and rent.
