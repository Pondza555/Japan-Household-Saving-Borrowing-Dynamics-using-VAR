# Japan Household Saving & Borrowing Dynamics using VAR

🔹 **Introduction**

This project investigates how changes in interest rates and inflation affect overall household saving and borrowing behavior in Japan after the Tom Yum Goong Crisis (1997 Asian Financial Crisis) using the Vector Autoregression (VAR) model. After the crisis, Japan entered a prolonged period of economic stagnation often referred to as the "Lost Decades," characterized by near-zero and negative interest rates, persistent low inflation, and periods of deflation. These conditions make Japan fundamentally different from standard macroeconomic settings and provide a unique case study for analyzing household financial behavior.

Main notebook: `VAR.ipynb`

🔹 **Methodology**

- Inspired by the paper: *A Brief Note on Thailand Household Debt Dynamics, Fisher Effects, and Monetary Policy Transmission* (Teerapap Pangsapa, Thanaphol Kongphalee, and Maneerat Gongsiang, March 6, 2025).
- Use VAR model to analyze impulse response functions (IRF) for interpretation.
- Seasonal adjustment applied using classical decomposition (additive method) and X13ARIMA.
- Cointegration testing via Engle-Granger two-step method & Johansen test to determine VAR vs VECM.

🔹 **Dataset**

| Item | Detail |
|------|--------|
| Source | CEIC, FRED, IMF |
| Period | 1997Q4 – 2025Q3 (28 years, quarterly) |
| Preprocessing | Seasonal adjustment, first differencing for stationarity |

**Factors:**

| # | Variable | Description |
|---|----------|-------------|
| 1 | Real GDP YOY | Year-over-year real GDP growth |
| 2 | Real Interest Rate | Fisher equation (Interest rate − CPI) |
| 3 | Debt Ratio | Household debt to nominal GDP |
| 4 | Saving Ratio | Household saving to nominal GDP |
| 5 | Dubai Crude Oil | Exogenous variable |

🔹 **EDA & Preprocessing**

- Real GDP has trend → ADF-test using trend.
- Saving Ratio has seasonal patterns → seasonal adjustment using additive method.
- Debt Ratio and Oil also have slight seasonality → seasonal adjustment applied.
- After seasonal adjustment, ADF-test showed only RealGDPYOYsa is stationary.
- First differencing applied → all variables become stationary (maxlag set to 4 for quarterly data).

🔹 **Model Selection**

- VAR(4) selected based on BIC, FPE, and HQIC criteria.
- Portmanteau test confirms no serial autocorrelation.
- Stability condition satisfied — all eigenvalues inside the unit circle.
- Engle-Granger cointegration test: residuals are non-stationary → no cointegration → use VAR (not VECM).
- Oil is used as an exogenous variable.

🔹 **Results (Granger Causality & IRF)**

| Granger Causality | p-value | IRF Interpretation |
|---|---|---|
| SavingRatio → RealGDPYOY | 0.0000 | Positive saving shock → negative GDP response in 1–3 quarters (paradox of thrift) |
| RealGDPYOY → RealIR | 0.0049 | GDP shock → slightly negative real interest rate response; BOJ maintains accommodative stance |
| RealGDPYOY → SavingRatio | 0.0405 | GDP shock → small negative saving ratio response; households consume more when confident |
| DebtRatio → (all) | Not significant | Debt ratio is slow-moving in the Japanese context |

**Note:** All IRF responses are small and short-lived with confidence bands encompassing zero, indicating statistically insignificant responses at individual periods despite significant Granger causality.

🔹 **Conclusion**

1. VAR(4) was selected based on BIC/FPE/HQIC; Engle-Granger confirms no cointegration.
2. Stability and no-autocorrelation conditions are satisfied.
3. SavingRatio strongly Granger-causes RealGDPYOY (p=0.0000): consistent with the paradox of thrift during Japan's Lost Decades.
4. RealGDPYOY Granger-causes RealIR (p=0.0049): reflects BOJ's zero lower bound monetary policy.
5. RealGDPYOY Granger-causes SavingRatio (p=0.0405): consistent with precautionary saving motive hypothesis.
6. Debt Ratio shows no significant short-run Granger causality, reflecting its slow-moving nature.
7. Findings are consistent with existing literature (Horioka 2006, 2024; Lombardi et al. 2017; Koo 2008).

🔹 **Executive Summary**

This project analyzes Japan's household saving and borrowing dynamics after the 1997 Asian Financial Crisis using a VAR(4) model with quarterly data (1997Q4–2025Q3). Five macroeconomic variables — Real GDP growth, Real Interest Rate, Debt Ratio, Saving Ratio, and Oil price (exogenous) — were examined after seasonal adjustment and stationarity testing. Key findings reveal that household saving strongly Granger-causes GDP growth (paradox of thrift), while GDP growth influences both the real interest rate and saving behavior. The debt ratio shows no significant short-run dynamics, consistent with its slow-moving nature in Japan's low-rate environment. All findings align with established literature on Japan's Lost Decades, BOJ's unconventional monetary policy, and household precautionary saving behavior.
