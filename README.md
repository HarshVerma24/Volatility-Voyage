# Volatility Voyage: Quantitative Analysis and Algorithmic Trading Framework

**Finance and Analytics Club, IIT Kanpur**[cite: 2]  
**Research Timeline:** Summer 2025[cite: 2]  
**Mentors:** Anurag Thakur, Arihant Satpathy, Kethan Challa, Shivam Tomar[cite: 2]

---

## Abstract

This repository contains the research, modeling, and backtesting framework developed during the **Volatility Voyage** project[cite: 2]. The project establishes a comprehensive quantitative pipeline starting from foundational Exploratory Data Analysis (EDA) and moving average crossovers, advancing into complex volatility-aware trading strategies[cite: 2]. By integrating risk management protocols, portfolio optimization theories, and advanced econometric volatility forecasting (ARMA, ARIMA, GARCH, EGARCH), the framework culminates in a capital-neutral long-short hedging strategy designed to isolate model-specific forecasting skill (pure alpha)[cite: 2].

---

## 1. Market Microstructure & Volatility-Aware Strategies

The initial phase of the project focuses on extracting market signals using foundational technical indicators and dynamic volatility bands[cite: 2]. 

### 1.1 Trend & Breakout Strategy Progression
The strategy engine evaluates asset prices through multiple iterative models[cite: 2]:

*   **Strategy 1: EMA Crossover:** A trend-following strategy initiating trades based on the crossover of the closing price and the Exponential Moving Average (EMA)[cite: 2]. The smoothing factor is defined as $\alpha=\frac{2}{n+1}$[cite: 2].
*   **Strategy 2: EMA + Dynamic Band:** Introduces a localized volatility band to filter false signals[cite: 2].
    *   Buy Band: $EMA + (Close - Open)$[cite: 2].
    *   Sell Band: $EMA - (Close - Open)$[cite: 2].
*   **Strategy 3: ATR Breakout:** Utilizes the Average True Range (ATR) as a dynamic threshold[cite: 2]. 
    *   Buy Band: $EMA + ATR$[cite: 2].
    *   Sell Band: $EMA - ATR$[cite: 2].

### 1.2 Dynamic Capital Allocation via VIX
To manage exposure dynamically, the framework utilizes the India VIX[cite: 2]. Capital deployment is scaled based on market uncertainty thresholds[cite: 2]:
*   **Threshold 1:** $\mu+0.5\sigma$[cite: 2].
*   **Threshold 2:** $\mu+1.5\sigma$[cite: 2].
*   **Allocation Rules:** 
    *   If VIX < Threshold 1: Allocate 100%[cite: 2].
    *   If Threshold 1 < VIX < Threshold 2: Allocate 75%[cite: 2].
    *   If VIX > Threshold 2: Allocate 50%[cite: 2].

---

## 2. Risk-Managed Trading Framework

Applied to highly volatile assets (e.g., BTC-USD), the system transitions into a hybrid momentum-mean-reversion strategy incorporating strict capital protection mechanisms[cite: 2]. 

### 2.1 Exit Methodologies
The backtesting engine is augmented with five distinct exit criteria to optimize the risk-reward ratio[cite: 2]:
*   **Trailing Stop Loss (TSL):** Dynamically locks in accrued profits[cite: 2].
*   **ATR-Based Stop Loss:** Adapts exits to current market volatility regimes[cite: 2].
*   **Take-Profit (TP):** Exits at fixed return multiples[cite: 2].
*   **Drawdown-Based Exit:** Liquidates positions to protect aggregate portfolio equity[cite: 2].
*   **Time-Based Exits:** Eliminates capital stagnation in non-performing assets[cite: 2].

### 2.2 Risk Performance Impact
Integrating these risk controls demonstrated a profound impact on portfolio stability[cite: 2]:
*   **Max Drawdown:** Reduced significantly to -46.95%[cite: 2].
*   **Sharpe Ratio:** Stabilized at an exceptional 2.55[cite: 2].
*   **Insight:** Risk management successfully curtailed drawdowns and optimized reward-to-risk without sacrificing overall portfolio viability[cite: 2].

---

## 3. Portfolio Optimization & Volatility Forecasting

The framework shifts from isolated asset trading to Modern Portfolio Theory (MPT), constructing risk-optimized multi-asset portfolios[cite: 2].

### 3.1 Mean-Variance Optimization
The portfolio variance equation drives asset weighting, specifically targeting negative correlations to maximize diversification[cite: 2]:

$$Var[R_{p}]=\omega_{a}^{2}\sigma_{a}^{2}+\omega_{b}^{2}\sigma_{b}^{2}+2\omega_{a}\omega_{b}\sigma_{a}\sigma_{b}\rho_{ab}$$

The model systematically plots the Efficient Frontier and isolates the Tangency Portfolio (maximizing the Sharpe Ratio) and the Minimum Variance Portfolio (MVP)[cite: 2].

### 3.2 Volatility Forecasting Models
To project future risk, three mathematical models are implemented[cite: 2]:
*   **Rolling Mean:** Simple historical standard deviation over a defined rolling window[cite: 2].
*   **Exponentially Weighted Moving Average (EWMA):** Prioritizes recent observations[cite: 2]. 
    $$\sigma_{t}^{2}=(1-\lambda)r_{t-1}^{2}+\lambda\sigma_{t-1}^{2}$$
*   **AR(1) Model:** An autoregressive framework predicting future variance based on historical lags[cite: 2].

### 3.3 Investor Risk Profiling
The system dynamically maps forecasted stock volatility against a quantitative investor risk score ($r \in [0,1]$)[cite: 2]. Forecasted volatility ($v$) is normalized[cite: 2]:

$$\tilde{v}=\frac{v-v_{min}}{v_{max}-v_{min}}$$

The algorithm selects assets where the normalized volatility most closely aligns with the inputted investor risk profile[cite: 2].

---

## 4. Advanced Econometrics & Capital-Neutral Hedging

To completely isolate the predictive edge of the models from broad market momentum (systematic risk), the project employs advanced econometric forecasting to build a hedged portfolio[cite: 2].

### 4.1 Econometric Volatility Models
*   **ARMA & ARIMA:** Captures linear dependencies and differencing for non-stationary series[cite: 2].
*   **GARCH:** Models time-varying conditional variance to capture volatility clustering[cite: 2].
*   **EGARCH:** Extends GARCH by modeling the log of conditional variance, explicitly capturing asymmetric volatility (leverage effects) where negative returns impact volatility more heavily than positive returns[cite: 2]:

$$log(\sigma_{t}^{2})=\omega+\sum_{i=1}^{p}\beta_{i}log(\sigma_{t-i}^{2})+\sum_{j=1}^{q}\alpha_{j}\frac{\epsilon_{t-j}}{\sigma_{t-j}}+\gamma_{j}(|\frac{\epsilon_{t-j}}{\sigma_{t-j}}|-E|\frac{\epsilon_{t-j}}{\sigma_{t-j}}|)$$

### 4.2 Final Hedging Strategy (Long-Short)
The ultimate deployment is a capital-neutral long-short strategy executing on NIFTY 50 constituents[cite: 2]:
1.  **Selection:** Identify the top two stocks with the strongest EGARCH/GARCH forecast signals[cite: 2].
2.  **Correlation Mapping:** Calculate daily log returns and find the single most positively correlated NIFTY 50 stock for each selected asset[cite: 2].
3.  **Execution:** Take a long position in the forecasted asset, and an equal-capital short position in its highly correlated pair ($Capital_{long}=Capital_{short}$)[cite: 2].
4.  **Result:** This hedge neutralizes beta (market risk), ensuring that residual returns are purely a product of the model's idiosyncratic forecasting edge[cite: 2].

---

## 5. Final Hedged Portfolio Results

The capital-neutral strategy was evaluated across top stock pairs, yielding the following core metrics[cite: 2]:

**Stock Pair 1 (Hedged):**[cite: 2]
*   **Gross Profit:** 6.65%[cite: 2]
*   **Max Drawdown:** 6.55%[cite: 2]
*   **Win/Loss Ratio:** 11 Wins / 17 Losses[cite: 2]
*   **Trade-wise Sharpe Ratio:** 1.0676[cite: 2]

**Stock Pair 2 (Hedged):**[cite: 2]
*   **Gross Profit:** 22.06%[cite: 2]
*   **Max Drawdown:** 7.40%[cite: 2]
*   **Win/Loss Ratio:** 12 Wins / 12 Losses[cite: 2]
*   **Trade-wise Sharpe Ratio:** 3.0357[cite: 2]
