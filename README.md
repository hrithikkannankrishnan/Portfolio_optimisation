# Regularized Portfolio Optimization with 100 Fama-French Portfolios 📈

Portfolio optimization project revisiting Markowitz using daily returns from 100 size–profitability portfolios. Compare equal-weight, minimum variance and regularized ML-based portfolios (LASSO, Ridge, Elastic Net, Huber, quantile) via rolling backtests and Sharpe/turnover metrics.

---

## 1. Project Overview

This repo explores how to build more stable and realistic equity portfolios by combining:

- Classic **Markowitz mean–variance optimization**
- Modern **regularized regression** methods
- A **rolling out-of-sample backtest** that mimics real portfolio management

We take the “100 Portfolios Formed on Size and Operating Profitability” from the **Kenneth R. French Data Library**, treat each of the 100 portfolios as an investable asset, and ask:

> Can we improve on the raw Minimum Variance (MinVar) portfolio by using ML-style regularization, while keeping the spirit of mean–variance?

---

## 2. Data

- Daily returns for **100 portfolios** sorted on:
  - **Size** (market equity)
  - **Operating profitability**
- Long history (multi-decade); project typically uses:
  - A **training window** (e.g., last 126 trading days)
  - A **validation / test window** for genuine out-of-sample evaluation
- Basic preprocessing:
  - Date parsing and sorting
  - Converting % returns to decimals
  - Handling missing values / sanity checks

(You are expected to download the data yourself from the Kenneth French Data Library and place it under `data/`.)

---

## 3. Methods

**Baselines**

- **Equal-Weight (EW)**: 1% in each of the 100 assets  
- **Minimum Variance (MinVar)**: Markowitz optimization with full covariance matrix

**Regularized Portfolio Models**

We reframe portfolio construction as a **regression problem**:

- Deviations from EW are treated as regression coefficients (portfolio weights)
- Constraint: weights sum to 1 (and optionally other constraints)

On top of this, we fit:

- **LASSO (L1)** regression  
- **Ridge (L2)** regression  
- **Elastic Net** (L1 + L2)  
- **Huber regression** (robust to outliers)  
- **Pinball / quantile regression** (focus on downside / tail behaviour)

**Backtesting**

- **Rolling window** (e.g., 126-day lookback, daily rebalancing)
- Each day:
  1. Use past 126 days to fit the model
  2. Compute portfolio weights
  3. Apply weights to next-day returns (out-of-sample)
- Repeat over the test period to obtain a return series for each strategy

**Evaluation Metrics**

- Mean return and volatility  
- **Sharpe ratio**  
- Turnover (trading intensity)  
- Optional: drawdowns, distribution plots, weight dispersion

---

## 4. Repository Structure

```text              
├─ notebooks/            # EDA, baselines, regularized models, backtests    # Python dependencies
└─ README.md
