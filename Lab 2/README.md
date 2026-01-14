# Lab 2: The Illusion of Growth & The Composition Effect

## Objective

Built a Python data pipeline to ingest live economic data from the Federal Reserve Economic Data (FRED) API, enabling real-time analysis of U.S. wage dynamics over a 60-year period. The project investigates two critical phenomena in labor economics: the "Money Illusion" (the gap between nominal and inflation-adjusted wages) and the "Composition Effect" (how workforce demographic shifts distort aggregate statistics).

## Tech Stack

- **Python** — Core programming language
- **fredapi** — Federal Reserve API wrapper for live data ingestion
- **pandas** — Time series manipulation and analysis
- **matplotlib** — Economic data visualization

## Methodology

### 1. Real Wage Construction
Fetched Average Hourly Earnings (AHETPI) and Consumer Price Index (CPIAUCSL) series directly from FRED. Calculated real wages by deflating nominal earnings to constant dollars, revealing the true trajectory of American purchasing power since 1964.

### 2. Anomaly Detection
Identified a statistically significant spike in real wages during Q2 2020—an apparent "wage boom" during an economic crisis. This paradox warranted further investigation.

### 3. Composition Effect Correction
To isolate whether the 2020 spike reflected genuine wage growth or statistical artifact, I fetched the Employment Cost Index (ECIWAG). Unlike simple averages, the ECI tracks wage changes for a fixed basket of occupations, controlling for workforce composition shifts. By rebasing both series and plotting them together, I isolated the bias introduced by demographic changes in the labor force.

## Key Findings

### The Money Illusion
Despite nominal wages rising roughly 10x since 1964, real purchasing power has remained essentially flat for American workers. The visual divergence between nominal and real wage curves illustrates how inflation erodes apparent gains—a textbook demonstration of the Money Illusion.

### The Pandemic Paradox
The 2020 "wage boom" was a statistical artifact, not a labor market signal. When low-wage workers in hospitality, retail, and service sectors disproportionately exited the labor force during lockdowns, the *average* wage mechanically increased—even though no individual worker received a raise. The ECI series, which holds occupation weights constant, showed stable growth through the same period, confirming the spike was entirely compositional.

This finding has direct implications for policy interpretation: headline wage statistics during labor market disruptions can be profoundly misleading without proper controls for workforce composition.
