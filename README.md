# GARCH-Based Volatility Forecasting

This project models and forecasts the volatility of Tesla Inc. (TSLA) stock returns using **GARCH** (Generalized Autoregressive Conditional Heteroskedasticity) models. It includes comprehensive modeling: from data collection and exploratory analysis to diagnostics, forecasting, and comparison with historical realized volatility — including both static and rolling forecast methods.

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Workflow & Methodology](#workflow--methodology)
  - [1. Data Collection](#1-data-collection)
  - [2. Exploratory Data Analysis (EDA)](#2-exploratory-data-analysis-eda)
  - [3. Stationarity Testing](#3-stationarity-testing)
  - [4. GARCH Model Training](#4-garch-model-training)
  - [5. Forecasting and Visualization](#5-forecasting-and-visualization)
  - [6. Rolling Forecasts](#6-rolling-forecasts)
- [Performance Insights](#performance-insights)
- [Future Work](#future-work)

---

## Overview

This project focuses on **forecasting volatility**, a key risk metric in financial markets, by fitting GARCH models to daily log returns of TSLA. Both **static 60-day forecasts** and **rolling 1-step-ahead forecasts** are compared to realized volatility. The goal is to understand and model **volatility clustering**, a hallmark of financial time series.

---

## Installation

Clone this repo and install the required dependencies:

```bash
pip install yfinance pandas numpy matplotlib seaborn arch statsmodels
```

## Workflow & Methodology

### 1. Data Collection

- **Stock**: Tesla Inc. (TSLA)  
- **Source**: `yfinance` API  
- **Period**: Jan 1, 2015 – Dec 31, 2023  
- **Metric**: Daily log returns computed from closing prices  

---

### 2. Exploratory Data Analysis (EDA)

- Visualized daily log returns
- Plotted:
  - Histogram + KDE to assess skew and kurtosis
  - Boxplot to detect outliers
  - Rolling statistics (mean and std)
  - ACF and PACF plots
  - ACF of squared returns to confirm conditional heteroskedasticity (volatility clustering)

---

### 3. Stationarity Testing

- **ADF Test**: Null = non-stationary → **Rejected**
- **KPSS Test**: Null = stationary → **Not Rejected**

**Conclusion**: Return series is stationary and suitable for GARCH modeling

---

### 4. GARCH Model Training

- Trained 4 models: `GARCH(1,1)`, `GARCH(1,2)`, `GARCH(2,1)`, `GARCH(2,2)`
- Evaluated using AIC/BIC scores:
  - `GARCH(1,1)` had the lowest AIC and BIC → best fit with minimal complexity
- Analyzed residuals and standardized residuals
- Performed **Ljung-Box test** on squared residuals to ensure no remaining ARCH effects

---

### 5. Forecasting and Visualization

- Generated **60-day ahead volatility forecasts** using `GARCH(1,1)`
- Compared GARCH volatility forecasts with **realized 30-day rolling volatility**

**Interpretation**:
- Forecasted vol > Realized vol → Future turbulence expected  
- Forecasted vol < Realized vol → Expected calm or mean reversion  

---

### 6. Rolling Forecasts

- Scaled log returns for numerical stability
- Used a rolling 250-day window to:
  - Fit `GARCH(1,1)`
  - Forecast 1 day ahead
  - Append and advance the window by one step
- Compared **rolling forecasted volatility** with **historical realized volatility**

**Result**: Rolling forecasts adapt better to local volatility regimes

---

## Performance Insights

- `GARCH(1,1)` captures volatility clustering and explains time-varying risk effectively
- Residual diagnostics confirm persistence of volatility shocks
- Rolling forecasts better mimic real-world forecasting needs

---

## Future Work

- Explore **asymmetric volatility models**:
  - `EGARCH`, `GJR-GARCH` for leverage effects
- Incorporate **exogenous variables** (e.g., macro indicators, sentiment)
- Extend to **multivariate models** (`DCC-GARCH`) for portfolio-level risk
- Benchmark against **deep learning models** (e.g., LSTM, Transformers)
- Deploy into a **real-time dashboard** or **risk management pipeline**

