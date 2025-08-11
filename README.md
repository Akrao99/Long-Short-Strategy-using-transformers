# Long-Short-Strategy-using-transformers

# Multi-Stock Long/Short Trading Strategy with Transformer & BorutaShap

This repository contains the code for a sophisticated quantitative trading strategy that uses a Transformer deep learning model to generate long/short signals for a portfolio of stocks. The strategy is designed to be market-neutral by holding an equal value of long and short positions.

[![Python Version](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/Framework-PyTorch-orange.svg)](https://pytorch.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE.md)

---

## 📈 Key Features

* **Advanced ML Model**: Utilizes a **Transformer** architecture, which is state-of-the-art for sequence analysis, to capture complex temporal patterns in financial data.
* **Intelligent Feature Selection**: Employs **BorutaShap** with a GPU-accelerated XGBoost model to systematically select the most relevant predictive features for each individual stock.
* **Extensive Feature Engineering**: Generates over 1,000 potential features, including returns-based metrics, volatility indicators, momentum factors, and relative strength comparisons against the market and sectors.
* **Robust Data Handling**: Fetches data from Alpha Vantage with a fallback to yfinance, handles missing values, and aligns time-series data across all assets to prevent lookahead bias.
* **Realistic Backtesting Engine**: The backtest includes key real-world factors like **transaction costs**, **slippage**, a defined **rebalancing frequency**, and **leverage constraints**.
* **Clear Visualizations**: Automatically generates comprehensive performance charts and reports using Plotly and Matplotlib.

---

## ⚙️ Technology Stack

* **Programming Language**: Python 3
* **Machine Learning**: PyTorch, XGBoost, Scikit-learn
* **Feature Selection**: BorutaShapPlus
* **Data Analysis & Manipulation**: Pandas, NumPy
* **Financial Data & Indicators**: Alpha Vantage, yfinance, TA-Lib, ta
* **Visualization**: Plotly, Matplotlib, Seaborn

---

## 🔧 Strategy Pipeline

The strategy follows a rigorous pipeline from data ingestion to signal generation:

1.  **Data Fetching**: Historical daily price data (Open, High, Low, Close, Volume) is downloaded for a user-defined `STOCK_UNIVERSE`. Market data (SPY, VIX) is also fetched for context.

2.  **Feature Engineering**: A comprehensive set of features is engineered for each stock, focusing on returns rather than prices to improve stationarity. This includes:
    * Core returns (daily, multi-day, intraday).
    * Volatility metrics (historical, Parkinson, Garman-Klass).
    * Momentum and Mean-Reversion indicators.
    * Volume-based and risk-adjusted return metrics.
    * Market-relative features (e.g., Beta, Alpha).
    * Sector-relative features (e.g., rank within the sector).

3.  **Feature Pruning**: To improve efficiency and model stability, features that are highly correlated (default threshold > 0.98) are removed.

4.  **Target Creation**: A target signal (`1` for Long, `-1` for Short, `0` for Neutral) is created for each stock based on its relative forward return performance compared to its peers.

5.  **Feature Selection**: For each stock, BorutaShap is used on the training data to identify the most impactful features for predicting its future return.

6.  **Model Training**: A separate Transformer model is trained for each stock using its unique set of selected features. The training process uses a weighted cross-entropy loss to handle class imbalance and employs early stopping to prevent overfitting.

7.  **Backtesting**: The trained models are used to run a backtest on out-of-sample data. The portfolio is rebalanced every 5 days by going long the top 5 stocks with the highest "buy" signal confidence and short the bottom 5 stocks with the highest "sell" signal confidence.

8.  **Signal Generation & Visualization**: Finally, the script generates the latest trading signals for the current day and produces detailed charts visualizing the backtest performance.

---

