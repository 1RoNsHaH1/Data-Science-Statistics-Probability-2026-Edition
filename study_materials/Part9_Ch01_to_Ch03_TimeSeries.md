# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 9 — Time Series Statistics
# Chapters 9.1–9.3: Fundamentals | Stationarity | Forecasting

---

# Chapter 9.1 — Time Series Fundamentals

> **Data ordered in time — the heartbeat of business analytics, finance, and forecasting.**

---

## 💡 What Is a Time Series?

> **A time series is a sequence of observations collected at successive, equally-spaced points in time.**

**Real-world time series:**

| Series | Frequency | Example Value |
|--------|-----------|--------------|
| Stock price (NIFTY 50) | Every second | 22,450 points |
| Daily temperature (Delhi) | Daily | 38°C |
| Monthly sales (e-commerce) | Monthly | ₹2.3 crore |
| Hourly website traffic | Hourly | 12,500 visits |
| Quarterly GDP growth | Quarterly | 6.3% |
| Annual cricket scores | Annual | Team avg: 280 |
| IPL match scores | Per match | 185 runs |

**Key distinction from cross-sectional data:** The order of observations matters. You cannot shuffle time series data!

---

## 9.1.1 — The Four Components of a Time Series

Every time series can be decomposed into four components:

```
Observed = Trend + Seasonality + Cyclicality + Noise

           TREND              SEASONALITY
           ┌──────────        ┌──────────
           │              ↗  │  /\/\/\/\
           │           ↗     │ /        \
           │        ↗        │/          \
           │     ↗           └──────────
           └──────────
           
           Long-run direction  Regular periodic pattern

           CYCLICALITY         NOISE (Residual)
           ┌──────────         ┌──────────
           │  ╭──╮             │ · · · ·
           │ ╭    ╮            │· · · ·
           │╭      ╮           │ · ·· ·
           └──────────         └──────────
           
           Irregular cycles    Random fluctuations
           (business cycle)    (unexplained variation)
```

### Component 1 — Trend (T)

> **The long-run direction of the series — upward, downward, or flat.**

- Rising: Smartphone sales over 20 years
- Falling: Landline subscriptions over 20 years
- Flat: Temperature of a controlled environment

### Component 2 — Seasonality (S)

> **Regular, predictable patterns that repeat at fixed periods.**

```
Monthly ice cream sales (India):
High: March-June (summer)
Low: November-February (winter)
Pattern repeats EVERY YEAR — seasonal!

Other seasonal patterns:
- Daily: Website traffic peaks at 8-10pm
- Weekly: Retail sales peak on Saturdays
- Yearly: Tax software sales peak in March
- Quarterly: Retail peaks in Diwali/Christmas quarter
```

### Component 3 — Cyclicality (C)

> **Long-wave fluctuations NOT of fixed period — driven by economic or business cycles.**

- Economic recession and recovery (3-10 year cycles)
- Real estate boom-bust cycles
- Technology adoption cycles (Hype Cycle)

**Key difference from seasonality:** Seasonal = fixed known period. Cyclical = irregular, longer-term waves.

### Component 4 — Noise / Residual (R)

> **The unexplained random variation remaining after removing T, S, and C.**

```
Noise = Observed - Trend - Seasonal - Cyclical

If your model is good, noise should be:
- Mean ≈ 0 (no systematic bias)
- No autocorrelation (each residual independent)
- Roughly normally distributed
```

---

## 9.1.2 — Additive vs Multiplicative Decomposition

**Additive Model** (seasonal amplitude is constant):
$$Y_t = T_t + S_t + C_t + R_t$$

Use when: Seasonal fluctuations stay roughly the same size over time.

**Multiplicative Model** (seasonal amplitude grows with trend):
$$Y_t = T_t \times S_t \times C_t \times R_t$$

Use when: Seasonal fluctuations grow proportionally with the trend (common in sales data).

```
Additive:                    Multiplicative:
Sales                        Sales
│  ↗↗↗↗↗↗↗↗↗               │  ↗↗↗
│ ↗ ↗ ↗ ↗ ↗ ↗               │ ↗ ↗
│↗  ↗  ↗  ↗  ↗              │↗   ↗↗↗↗
└──────────────── Time       └──────────────── Time

Seasonal swings same size    Seasonal swings grow with trend
→ Additive                   → Multiplicative
```

---

## 9.1.3 — Autocorrelation

> **Autocorrelation (serial correlation) measures how correlated a time series is with its own past values.**

**Intuition:** Today's temperature is similar to yesterday's. Today's stock price is related to yesterday's. This self-correlation is autocorrelation.

**Autocorrelation Function (ACF):**

$$r_k = \text{Corr}(Y_t, Y_{t-k}) = \frac{\text{Cov}(Y_t, Y_{t-k})}{\text{Var}(Y_t)}$$

Where k = **lag** (how many periods back).

```
ACF Plot example (ACF vs Lag):

r
1.0 │*
    │ *
0.5 │  *
    │   *
0.0 │────*──*──*──*──────────── Lag
    │          *
-0.5│           *

Strong positive autocorrelation at lag 1: today ≈ yesterday
Decaying to zero: each lag matters less
```

**Seasonal ACF:** Spikes at lag = period (e.g., lag 12 for monthly seasonal data).

---

## 9.1.4 — Python: Decomposition

```python
import pandas as pd
from statsmodels.tsa.seasonal import seasonal_decompose
import matplotlib.pyplot as plt

# Decompose time series
result = seasonal_decompose(df['sales'], 
                            model='additive',  # or 'multiplicative'
                            period=12)         # monthly → period=12

# Plot components
result.plot()
plt.show()

# Access components
trend = result.trend
seasonal = result.seasonal
residual = result.resid
```

---

## 🚀 Data Science Applications

| Application | Time Series Use |
|-------------|----------------|
| Demand forecasting | Predict next month's sales |
| Anomaly detection | Flag unusual spikes in metrics |
| Financial prediction | Stock price, exchange rate forecasting |
| Capacity planning | Predict server load |
| A/B test duration | Account for seasonality |
| Churn prediction | Model subscription patterns over time |

---

# Chapter 9.2 — Stationarity

> **The most important concept for time series modeling.**

---

## 💡 Why Stationarity Matters

Almost all classical time series models (ARMA, ARIMA) require **stationarity**. Non-stationary data violates model assumptions, leading to spurious results.

> **Spurious Correlation Example:** Number of Nicolas Cage movies and swimming pool drownings are both trending upward — they appear correlated! But it's just two upward trends — not a real relationship. This is spurious correlation caused by non-stationarity.

---

## 9.2.1 — Definition of Stationarity

A time series is **strictly stationary** if its joint distribution doesn't change with time.

**Weak (covariance) stationarity** (practical definition):
1. **Constant mean:** E[Yₜ] = μ for all t
2. **Constant variance:** Var(Yₜ) = σ² for all t
3. **Constant autocovariance:** Cov(Yₜ, Yₜ₋ₖ) depends only on lag k, not on t

```
STATIONARY:                   NON-STATIONARY:
                              
μ ─────────────────────       μₜ ↗↗↗↗↗↗↗↗↗↗  ← Trend
  ┌─┐   ┌─┐   ┌─┐              variance σₜ↗  ← Growing variance
  │ │   │ │   │ │
──┘ └───┘ └───┘ └───
Mean and variance constant    Mean and/or variance changes with time
```

---

## 9.2.2 — Common Non-Stationarities

| Type | Description | Example |
|------|-------------|---------|
| Trend | Mean changes with time | Rising sales |
| Unit root | Random walk with drift | Stock price |
| Seasonal | Periodic variation in mean | Monthly tourism |
| Structural break | Sudden shift in mean/variance | COVID impact on sales |
| Heteroscedasticity | Variance changes with time | Financial volatility |

---

## 9.2.3 — Testing for Stationarity

### Visual Check
Plot the series. Look for:
- Trending mean → non-stationary
- Changing variance → non-stationary
- Plot ACF: slowly decaying ACF → non-stationary

### Augmented Dickey-Fuller (ADF) Test

**H₀:** Series has a unit root (is non-stationary)
**H₁:** Series is stationary

```python
from statsmodels.tsa.stattools import adfuller

result = adfuller(df['sales'])
print(f'ADF Statistic: {result[0]:.4f}')
print(f'p-value: {result[1]:.4f}')

# Interpret:
# p < 0.05 → reject H₀ → stationary ✅
# p > 0.05 → fail to reject H₀ → non-stationary ❌
```

### KPSS Test (complementary to ADF)

**H₀:** Series IS stationary
**H₁:** Series has unit root

Use both ADF and KPSS for robust conclusion.

---

## 9.2.4 — Making a Series Stationary

### Differencing (Most Common)

**First difference:** ΔYₜ = Yₜ - Yₜ₋₁

```
Original (non-stationary: trending):
100, 105, 110, 112, 118, 125, 130

First difference (often stationary):
5, 5, 2, 6, 7, 5

→ Removed the trend!
```

**Seasonal differencing:** ΔₛYₜ = Yₜ - Yₜ₋ₛ (where s = seasonal period)

For monthly data: Yₜ - Yₜ₋₁₂ removes annual seasonality.

### Log Transformation

For series with growing variance: apply log first, then difference.

```
Returns = log(Pₜ/Pₜ₋₁) = log(Pₜ) - log(Pₜ₋₁)

This is how stock log-returns are computed!
```

---

## 9.2.5 — ACF and PACF

**ACF (Autocorrelation Function):** Correlation of Yₜ with Yₜ₋ₖ for each lag k.

**PACF (Partial Autocorrelation Function):** Correlation of Yₜ with Yₜ₋ₖ after removing the effects of all intermediate lags 1 through k-1.

```
ACF and PACF help identify ARIMA model order:

Model      ACF Pattern          PACF Pattern
AR(p)      Decays to zero       Cuts off after lag p
MA(q)      Cuts off after lag q Decays to zero
ARMA(p,q)  Both decay          Both decay
```

```python
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf

plot_acf(df['sales'], lags=40)
plot_pacf(df['sales'], lags=40)
plt.show()
```

---

# Chapter 9.3 — Forecasting Basics

> **Using the past to predict the future — the ultimate goal of time series analysis.**

---

## 9.3.1 — Simple Forecasting Methods

### Naïve Forecast
Forecast = last observed value.
$$\hat{Y}_{t+1} = Y_t$$
Surprisingly hard to beat for short horizons!

### Average Method
Forecast = mean of all historical values.
$$\hat{Y}_{t+1} = \bar{Y}$$

### Seasonal Naïve
Forecast = value from same period last season.
$$\hat{Y}_{t+1} = Y_{t-s+1}$$

---

## 9.3.2 — Exponential Smoothing

### Simple Exponential Smoothing (SES)

> **Forecast = weighted average of all past observations, with exponentially decaying weights.**

$$\hat{Y}_{t+1} = \alpha Y_t + (1-\alpha)\hat{Y}_t$$

| Symbol | Meaning |
|--------|---------|
| α | Smoothing parameter (0 < α < 1) |
| Yₜ | Most recent actual value |
| Ŷₜ | Previous forecast |

```
Expanding:
Ŷ_{t+1} = αYₜ + α(1-α)Yₜ₋₁ + α(1-α)²Yₜ₋₂ + ...

Weights: α, α(1-α), α(1-α)², ...
         ↓       ↓           ↓
       Most                Least
      recent              recent

Large α: follows recent data closely (more reactive)
Small α: smoother, follows long-run average (more stable)
```

**Example:** α=0.3, previous forecast=100, actual=110.
New forecast = 0.3×110 + 0.7×100 = 33 + 70 = **103**

### Holt's Method (Trend + Level)

Extends SES to handle trends:
- Level equation: Lₜ = αYₜ + (1-α)(Lₜ₋₁ + Bₜ₋₁)
- Trend equation: Bₜ = β(Lₜ - Lₜ₋₁) + (1-β)Bₜ₋₁
- Forecast: Ŷₜ₊ₕ = Lₜ + h×Bₜ

### Holt-Winters Method (Trend + Seasonality)

Extends to handle both trend and seasonality.
Best simple method for most business time series.

---

## 9.3.3 — Moving Averages

**Simple Moving Average (SMA):**

$$\text{SMA}_t(k) = \frac{1}{k}\sum_{i=0}^{k-1} Y_{t-i}$$

The mean of the last k observations. Acts as a smoother and forecaster.

```
7-day SMA of daily sales:
Day:     1   2   3   4   5   6   7   8
Sales:   10  12  11  13  14  12  15  11
SMA(7) at day 7: (10+12+11+13+14+12+15)/7 = 87/7 ≈ 12.4
SMA(7) at day 8: (12+11+13+14+12+15+11)/7 = 88/7 ≈ 12.6
```

**Weighted Moving Average:** More recent observations get higher weight.

**Exponential Moving Average (EMA):** Widely used in finance and ML training metrics.

---

## 9.3.4 — ARIMA Models

**ARIMA(p, d, q):** AutoRegressive Integrated Moving Average

| Component | Letter | Meaning |
|-----------|--------|---------|
| p | AR | AutoRegressive: Yₜ depends on its own past p values |
| d | I | Integrated: number of times differenced to achieve stationarity |
| q | MA | Moving Average: Yₜ depends on past q forecast errors |

**AR(p) Component:**
$$Y_t = c + \phi_1 Y_{t-1} + \phi_2 Y_{t-2} + \cdots + \phi_p Y_{t-p} + \epsilon_t$$

**MA(q) Component:**
$$Y_t = c + \epsilon_t + \theta_1 \epsilon_{t-1} + \theta_2 \epsilon_{t-2} + \cdots + \theta_q \epsilon_{t-q}$$

**ARIMA Workflow:**
```
STEP 1: Plot series → identify non-stationarity
STEP 2: Apply differencing d times → stationary series
STEP 3: Plot ACF/PACF → identify p and q
STEP 4: Fit ARIMA(p,d,q)
STEP 5: Check residuals → should be white noise
STEP 6: Forecast

Auto-ARIMA (auto_arima from pmdarima):
Automatically searches for best (p,d,q) using AIC/BIC
```

**SARIMA(p,d,q)(P,D,Q)[s]:** Extends ARIMA for seasonal patterns.

---

## 9.3.5 — Model Evaluation for Time Series

**Critical rule:** You CANNOT randomly split time series into train/test!

```
WRONG (standard cross-validation shuffles data):
Test:  ──┤  ├──┤  ├──┤  ├── (random samples)
Train: ───────────────────

CORRECT (time series split — test always after train):
Train: ─────────────────────┤
Test:                       ├─────

Walk-forward validation (most robust):
Round 1: Train[1..100] → Test[101..110]
Round 2: Train[1..110] → Test[111..120]
Round 3: Train[1..120] → Test[121..130]
...
```

**Forecast Error Metrics:**

| Metric | Formula | Notes |
|--------|---------|-------|
| MAE | (1/n)Σ\|Yₜ-Ŷₜ\| | Easy to interpret |
| MSE | (1/n)Σ(Yₜ-Ŷₜ)² | Penalizes large errors |
| RMSE | √MSE | Same units as series |
| MAPE | (1/n)Σ\|eₜ/Yₜ\|×100% | % error — interpretable |
| MASE | MAE / MAE_naïve | Scale-free, compares to naïve |

---

## 9.3.6 — Modern Time Series Models

| Model | Type | Strengths |
|-------|------|-----------|
| ARIMA | Statistical | Interpretable, well-understood |
| Prophet (Meta) | Statistical + ML | Handles holidays, missing data, multiple seasonality |
| LSTM | Deep Learning | Complex patterns, multivariate |
| Transformer (Temporal Fusion) | Deep Learning | Long-range dependencies |
| XGBoost with lag features | ML | Fast, handles mixed data, often best in practice |
| N-BEATS / N-HiTS | Deep Learning | State-of-art for univariate forecasting |

---

# 📝 Practice Questions — Part 9

## 🟢 Beginner (8 Questions)

**Q1.** What are the four components of a time series?
**Q2.** Is monthly rainfall data additive or multiplicative? Why?
**Q3.** What does "autocorrelation at lag 3" mean?
**Q4.** A series has a strong linear upward trend. Is it stationary?
**Q5.** First-difference: [100, 105, 98, 110, 107]. Compute ΔYₜ.
**Q6.** SES with α=0.4, previous forecast=50, actual=60. Find new forecast.
**Q7.** What is a 5-day SMA of [10,12,14,13,15,16,14]? Compute for day 5, 6, 7.
**Q8.** What does ARIMA(2,1,0) mean?

## 🟡 Intermediate (8 Questions)

**Q9.** ADF test returns p=0.32. What action do you take?
**Q10.** Monthly sales data: ACF shows spike at lag 12. What does this indicate?
**Q11.** Compare: why is walk-forward validation correct for time series but k-fold is wrong?
**Q12.** MAPE for actual=[100,200,150], forecast=[90,210,160]? Interpret the result.
**Q13.** Holt's exponential smoothing: Level=100, Trend=5, α=0.2, β=0.3. Actual at next period=115. Update Level and Trend.
**Q14.** What is the difference between ACF and PACF? How do you use them to choose ARIMA order?
**Q15.** A company's daily website traffic has weekly seasonality (s=7) and monthly data doesn't. What SARIMA seasonal period would you use?
**Q16.** Log-return of a stock: P₁=1000, P₂=1050. Compute log-return. Why is this preferred over simple return?

## 🔴 Advanced (4 Questions)

**Q17.** Unit root: show why a random walk Yₜ = Yₜ₋₁ + εₜ is non-stationary (compute Var(Yₜ)).
**Q18.** Wold decomposition theorem: every stationary series can be written as MA(∞). How does this justify ARMA models?
**Q19.** Cointegration: two non-stationary series X and Y might have a stationary linear combination aX + bY. How does this differ from spurious regression?
**Q20.** GARCH model: Yₜ = σₜεₜ, σₜ² = ω + αε²ₜ₋₁ + βσ²ₜ₋₁. What financial phenomenon does this capture?

## 🚀 Data Science Scenarios (4 Questions)

**DS1.** You're forecasting Swiggy's daily orders. You have 2 years of daily data. Describe your full modeling workflow: preprocessing, stationarity check, model selection, validation, and metric choice.

**DS2.** Stock price prediction: a junior analyst builds a model using standard 80/20 random train-test split on 5 years of daily stock prices. What is wrong with this approach? What will the artificially inflated accuracy reveal?

**DS3.** You deploy a sales forecasting model in January. By October, model accuracy has degraded significantly (MAPE doubled). What could cause this? List 3 specific reasons and solutions.

**DS4.** Prophet vs ARIMA: You have 3 years of monthly data with clear yearly seasonality and some holiday spikes (Diwali, Christmas). Which model would you choose and why?

---

# ✅ Key Solutions

**A1.** Trend, Seasonality, Cyclicality, Noise (Residual).
**A4.** No — a trending series has a changing mean, violating stationarity.
**A5.** [5, -7, 12, -3].
**A6.** 0.4×60 + 0.6×50 = 24+30 = **54**.
**A7.** Day 5 SMA: (10+12+14+13+15)/5=12.8. Day 6: (12+14+13+15+16)/5=14.0. Day 7: (14+13+15+16+14)/5=14.4.
**A9.** p=0.32 > 0.05 → fail to reject H₀ → non-stationary. Apply differencing, then retest.
**A12.** MAPE = (|10/100|+|10/200|+|10/150|)/3 × 100% = (0.10+0.05+0.067)/3 = 7.2%. Forecasts are off by about 7.2% on average.
**A16.** log(1050/1000) = log(1.05) ≈ 0.0488 = 4.88%. Simple return = (1050-1000)/1000 = 5%. Log-returns are additive over time (sum of daily log-returns = total log-return) and approximately normally distributed — essential for financial statistics.
**A17.** Var(Yₜ) = Var(Y₀) + t×σ²ε → grows linearly with t → variance not constant → non-stationary.

**DS1.** (1) Load 2 years daily data. (2) Visualize: plot series, check for trend, seasonality, outliers. (3) ADF test → likely non-stationary (trend). (4) Apply log transform if variance grows. (5) First difference + seasonal difference (lag 7). (6) Re-run ADF: p<0.05. (7) Plot ACF/PACF → identify p,q. (8) Fit SARIMA(p,1,q)(P,1,Q)[7]. (9) Validate with walk-forward CV on last 2 months. (10) Metric: MAPE (interpretable for business). (11) Monitor drift monthly.

**DS4.** Prophet. Reasons: (1) Built-in seasonal decomposition handles yearly and weekly patterns automatically. (2) Built-in holiday effect modeling (Diwali, Christmas) is a major advantage over ARIMA. (3) More robust to missing data and outliers. (4) Easier to use and interpret for business teams. (5) ARIMA would require manual holiday feature engineering and multiple seasonal terms (SARIMA) making it more complex.

---

# 💼 Interview Questions — Part 9

**IQ1.** "What is stationarity and why does it matter?"
Stationarity means constant statistical properties (mean, variance, autocorrelation) over time. It matters because: (1) ARIMA and related models assume stationarity; (2) Non-stationary series can produce spurious correlations; (3) Forecasting a non-stationary series without differencing gives unreliable predictions. Solution: difference the series (ARIMA's 'd' parameter).

**IQ2.** "How do you choose between ARIMA and Prophet for a business forecasting problem?"
ARIMA: better when you need a pure statistical model, data is relatively clean, seasonality is simple, and you need prediction intervals with statistical guarantees. Prophet: better when there are multiple seasonalities, holiday effects, missing data, non-linear trends, and you need a model that non-statisticians can tune. For most business use cases, start with Prophet.

**IQ3.** "What is the difference between correlation and autocorrelation?"
Correlation: relationship between two different variables X and Y. Autocorrelation: relationship of a variable with its own past values (Yₜ vs Yₜ₋ₖ). Autocorrelation measures how much time series "memory" exists — does today's value predict tomorrow's?

---

# 📌 Part 9 Summary

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| Yₜ = Tₜ + Sₜ + Cₜ + Rₜ | Additive decomposition |
| Yₜ = Tₜ × Sₜ × Cₜ × Rₜ | Multiplicative decomposition |
| rₖ = Corr(Yₜ, Yₜ₋ₖ) | Autocorrelation at lag k |
| ΔYₜ = Yₜ - Yₜ₋₁ | First difference |
| Ŷₜ₊₁ = αYₜ + (1-α)Ŷₜ | Simple exponential smoothing |
| MAPE = (1/n)Σ\|eₜ/Yₜ\|×100% | Mean Absolute Percentage Error |

---
*Part 9 Complete | Next: Part 10 — Modern 2026 Topics*
