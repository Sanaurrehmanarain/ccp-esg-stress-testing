# Project Development Log

## Phase 1: Data Acquisition & Analysis
- [x] **Setup Project Structure:** Created robust directory tree for reproducibility.
- [x] **Data Ingestion:** Fetched 6 years of daily data for 7 G-SIBs and ESG proxies (XLE, ICLN).
    - *Challenge:* `yfinance` API version conflict caused missing columns.
    - *Solution:* Implemented a robust column-checker to handle 'Adj Close' vs 'Close' automatically.
- [x] **Feature Engineering:** Calculated Log Returns and Rolling Volatility.
    - *Insight:* Confirmed "Brown" Energy assets (XLE) have 68% higher volatility than the S&P 500.

## Phase 2: Mathematical Modeling
- [x] **Volatility Modeling (GARCH):** Fitted GARCH(1,1) to all clearing members.
    - *Result:* Found high persistence (>0.90) in volatility, justifying the need for dynamic margin models.
- [x] **Correlation Analysis:** Computed rolling correlations and "ESG Betas."
    - *Discovery:* Identified Citigroup as the "Weak Link" with a Brown Beta of 0.74 (highest sensitivity to oil shocks).
- [x] **Simulation Engine:** Built a Multivariate Geometric Brownian Motion (GBM) class.
    - *Feature:* Integrated Cholesky Decomposition to preserve systemic correlations during shocks.

## Phase 3: Stress Testing & Results
- [x] **Scenario Generation:** Simulated 10,000 paths for "Baseline" vs. "Brown Crash" scenarios.
- [x] **Risk Quantification:** Calculated Expected Shortfall (ES) at 99% confidence.
    - *Outcome:* Identified a **47.5% increase** in capital requirements under the ESG shock scenario.

## Summary of Achievements
| Metric | Baseline Model | ESG-Integrated Model | Change |
| :--- | :--- | :--- | :--- |
| **Risk Metric** | Expected Shortfall (99%) | Expected Shortfall (99%) | -- |
| **Citigroup Loss** | -$29.00 | -$42.78 | **+47.5%** |
| **Volatility Assumption** | Static (Normal) | Dynamic (GARCH) | High Precision |
| **Correlation** | Constant | Regime-Dependent | Systemic |