# Lab-Assignment-3-Time-Series

## Description
This project performs **Time Series Analysis** on Apple Inc. (AAPL) stock price data using four approaches:
1. **Time Series Properties Analysis** — Trend, Seasonality, Stationarity, ACF/PACF
2. **Hidden Markov Model (HMM) — From Scratch** — manually implemented Forward Algorithm, Viterbi Algorithm, Future State Prediction
3. **Hidden Markov Model (HMM) — Using `hmmlearn` Library** — Gaussian HMM trained on real continuous returns, posterior probabilities, regime visualization
4. **ARIMA** — stationarity check, model fitting, forecasting with metrics

All parts share the same dataset and are compared at the end.

---

## Objective
- Analyze time series properties (trend, seasonality, stationarity)
- Implement HMM **from scratch** to detect hidden market states (Bull/Bear/Neutral)
- Implement HMM using **`hmmlearn` library** on continuous return data for real-world regime detection
- Implement ARIMA to forecast future stock prices
- Compare Manual HMM vs hmmlearn HMM vs ARIMA

---

## Algorithm

### Time Series Properties
1. Plot rolling mean and rolling std to identify trend
2. Seasonal decomposition (trend + seasonal + residual)
3. ADF test to check stationarity
4. Apply differencing if non-stationary
5. Plot ACF & PACF to determine ARIMA parameters

### Hidden Markov Model — From Scratch (Part 2)
1. Define hidden states: Bull 📈, Bear 📉, Neutral ➡️
2. Define observations: Up (U), Down (D), Stable (S)
3. Define transition matrix A, emission matrix B, initial probability π
4. **Forward Algorithm** — compute P(observation sequence | model)
5. **Viterbi Algorithm** — find most likely hidden state sequence
6. Predict future hidden states using transition probabilities

### Hidden Markov Model — hmmlearn Library (Part 2B)
1. Compute daily returns and log returns from AAPL closing prices
2. Fit a **3-state Gaussian HMM** using Baum-Welch (EM) algorithm
3. Learn transition matrix A, emission parameters, and initial π from data
4. Decode hidden states using built-in Viterbi (`.predict()`)
5. Compute posterior state probabilities (`.predict_proba()`)
6. Predict future states using the learned transition matrix
7. Compare results with manual HMM

### ARIMA(p, d, q)
1. Check stationarity using ADF test → d=1 (one differencing)
2. Use ACF/PACF plots → p=1, q=1
3. Fit ARIMA(1,1,1) on training data
4. Forecast on test set → evaluate with RMSE and MAE
5. Forecast future prices

---

## Dataset

| Property | Details |
|---|---|
| Source | Yahoo Finance via `yfinance` |
| Stock | Apple Inc. (AAPL) |
| Period | 2015-01-01 to 2023-01-01 |
| Feature used | Daily Closing Price |
| Total rows | ~2,000 |
| Download | Automatic — no manual download needed |

---

## Notebook Structure

```
Step 1  → Import all libraries (once, shared)
Step 2  → Load AAPL stock dataset (once, shared)
──────────────────────────────────────────────────
Part 1  → Time Series Properties Analysis (Steps 3–6)
            Step 3  → Trend + Rolling Mean & Std
            Step 4  → Seasonal Decomposition
            Step 5  → Stationarity Check (ADF Test)
            Step 6  → ACF & PACF Plots
──────────────────────────────────────────────────
Part 2  → Hidden Markov Model — From Scratch (Steps 7–12)
            Step 7  → User Input (U/D/S sequence)
            Step 8  → HMM Parameters (A, B, π)
            Step 9  → Forward Algorithm
            Step 10 → Viterbi Algorithm
            Step 11 → Visualize Observations vs Hidden States
            Step 12 → HMM Future State Prediction
──────────────────────────────────────────────────
Part 2B → Hidden Markov Model — hmmlearn Library (Steps 12B-1 to 12B-8)
            Step 12B-1 → Install & Import hmmlearn
            Step 12B-2 → Prepare Features (Returns, Log Returns)
            Step 12B-3 → Fit Gaussian HMM (Baum-Welch / EM)
            Step 12B-4 → Decode Hidden States (Built-in Viterbi)
            Step 12B-5 → Visualize Regimes on Price Chart
            Step 12B-6 → Posterior State Probabilities
            Step 12B-7 → Future State Prediction
            Step 12B-8 → Manual HMM vs hmmlearn Comparison
──────────────────────────────────────────────────
Part 3  → ARIMA Implementation (Steps 13–16)
            Step 13 → Train/Test Split
            Step 14 → Fit ARIMA(1,1,1)
            Step 15 → Forecast & Evaluate (RMSE, MAE)
            Step 16 → Future Price Forecast
──────────────────────────────────────────────────
Part 4  → Manual HMM vs hmmlearn HMM vs ARIMA Comparison (Step 17)
```

> **Note:** Dataset is loaded **once** in Step 2 and shared across all parts.

---

## HMM Parameters

### Part 2 — Manual HMM (User-Defined)

#### Hidden States
| State | Meaning |
|---|---|
| Bull | Market is rising 📈 |
| Bear | Market is falling 📉 |
| Neutral | Market is stable ➡️ |

#### Observations
| Symbol | Full Name | Meaning |
|---|---|---|
| U | Up | Price went up |
| D | Down | Price went down |
| S | Stable | Price stayed flat |

#### Transition Matrix A
| | Bull | Bear | Neutral |
|---|---|---|---|
| **Bull** | 0.6 | 0.2 | 0.2 |
| **Bear** | 0.2 | 0.5 | 0.3 |
| **Neutral** | 0.3 | 0.3 | 0.4 |

#### Emission Matrix B
| | Up | Down | Stable |
|---|---|---|---|
| **Bull** | 0.7 | 0.1 | 0.2 |
| **Bear** | 0.1 | 0.7 | 0.2 |
| **Neutral** | 0.3 | 0.3 | 0.4 |

---

### Part 2B — hmmlearn HMM (Learned from Data)

| Parameter | Details |
|---|---|
| Model | `GaussianHMM` (hmmlearn) |
| Number of states | 3 (Bull / Neutral / Bear — auto-labeled by mean return) |
| Input features | Daily Returns + Log Returns |
| Covariance type | Full |
| Training algorithm | Baum-Welch (Expectation-Maximization) |
| Iterations | 200 |
| Parameters learned | Transition matrix A, emission means & covariances, initial π |

> Parameters are **not manually defined** — they are learned automatically from ~2,000 data points.

---

## ARIMA Parameters

| Parameter | Value | Determined By |
|---|---|---|
| p (AR order) | 1 | PACF plot |
| d (differencing) | 1 | ADF test |
| q (MA order) | 1 | ACF plot |

**Model:** ARIMA(1, 1, 1)

---

## Results

### Time Series Properties
- AAPL shows a clear **upward trend** over 2015–2023
- Original series is **non-stationary** (ADF p > 0.05)
- First differencing makes it **stationary** ✅

### HMM — From Scratch
- Successfully identifies Bull/Bear/Neutral market regimes
- Forward Algorithm computes observation sequence probability
- Viterbi Algorithm finds the most likely hidden state path

### HMM — hmmlearn Library
- Gaussian HMM converges via Baum-Welch on real return data
- Viterbi decoding applied to full ~2,000-day AAPL history
- Posterior probabilities show soft regime transitions
- States auto-labeled Bull/Neutral/Bear by learned mean returns

### ARIMA
| Metric | Value |
|---|---|
| RMSE | ~varies by run |
| MAE | ~varies by run |

---

## Manual HMM vs hmmlearn vs ARIMA Comparison

| Feature | Manual HMM | hmmlearn HMM | ARIMA |
|---|---|---|---|
| Type | Probabilistic | Probabilistic | Statistical |
| Input | Discrete (U/D/S) | Continuous (returns) | Continuous prices |
| Parameters | Hand-defined | Learned from data | Determined by ACF/PACF |
| Forward Algorithm | Hand-coded | Built-in `.score()` | — |
| Viterbi | Hand-coded | Built-in `.predict()` | — |
| Posterior probs | Not computed | `.predict_proba()` | — |
| Output | Hidden states | Hidden states | Numeric price forecast |
| Stationarity needed | No | No | Yes (differencing) |
| Scalability | Small sequences | Full historical data | Moderate |
| Best for | Education / demos | Real-world regime detection | Short-term price forecast |

---

## How to Run

1. Open in **Google Colab**
2. Run all cells in order
3. At **Step 7** — enter observation sequence when prompted:
   - Option 1: Manual — type e.g. `U D U S D U`
   - Option 2: Random — generates 10 random observations
4. At **Step 12** — enter number of future days for manual HMM prediction
5. At **Step 12B-1** — `hmmlearn` is auto-installed if not present
6. At **Step 12B-7** — enter number of future days for hmmlearn HMM prediction
7. At **Step 16** — enter number of future days for ARIMA forecast

---

## Requirements

Most libraries are pre-installed in Google Colab. `hmmlearn` is installed automatically in Step 12B-1.

```
numpy
pandas
matplotlib
seaborn
statsmodels
yfinance
scikit-learn
hmmlearn          ← auto-installed in Step 12B-1
```

---

## Applications

**HMM (Manual & hmmlearn):**
- Stock market regime detection (Bull/Bear/Neutral)
- Speech recognition
- Bioinformatics (gene sequencing)
- Weather pattern prediction

**ARIMA:**
- Stock price forecasting
- Sales demand forecasting
- Economic indicator prediction
- Energy consumption forecasting

---

## Conclusion

This assignment performed Time Series Analysis on AAPL stock prices using four approaches:

1. **Properties Analysis:** Clear upward trend identified. Original series was non-stationary, first differencing fixed it. ACF/PACF confirmed ARIMA(1,1,1).

2. **Manual HMM:** Forward and Viterbi algorithms hand-coded from scratch. Effectively identifies Bull/Bear/Neutral market regimes from discrete observation sequences (U/D/S). Best for understanding HMM internals.

3. **hmmlearn HMM:** Gaussian HMM fitted on real continuous return data using the Baum-Welch algorithm. Automatically learns all parameters and decodes regimes across the full price history. Posterior probabilities reveal soft transitions between Bull/Bear/Neutral states. Best for real-world applications.

4. **ARIMA(1,1,1):** Fitted on training data, forecasted test prices with measurable RMSE/MAE. Future price forecasting implemented.

**Key takeaway:** Manual HMM builds intuition about HMM mechanics; hmmlearn scales that to real data by learning parameters automatically. Together with ARIMA, these three methods form a comprehensive framework — HMM identifies *what kind of market* we're in, while ARIMA predicts *exact future prices*.

---

**Name:** Parimal Ahire  
**PRN:** 202301040067  
**Course:** Deep Learning Lab
