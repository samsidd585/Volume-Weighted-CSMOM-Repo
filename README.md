# Volume-Weighted-CSMOM-Repo

## 1) Introduction

This strategy builds on the findings of Huang, Sangiorgi & Urquhart (2024). I attempted to replicate the paper’s core idea using Binance Global data and a universe of 77 cryptocurrencies selected from the top 100 by market capitalization in November of 2021 that had a average daily trading volume >$1 million, excluding stablecoins. In my implementation, frequent portfolio rebalancing generated substantial turnover, and once transaction costs were applied the strategy’s performance deteriorated, resulting in a negative Sharpe ratio.

I also tested a more selective approach that only took positions in assets exhibiting the strongest time-series momentum signals, but performance remained unstable across parameter choices. One possible explanation is differences in data construction or coverage between Binance Global and the data source used in the paper (for example, CoinMarketCap). That said, the primary driver of underperformance in my tests appears to be transaction costs, which may not have been fully incorporated in the original study and materially reduce the strategy’s edge under realistic trading assumptions. As a result, I decided to build on top of this paper using a hybrid time series momentum and cross sectional momentum signal construction process. Additionally, I incorporated a BTC moving average regime filter to develop net long and short positions when BTC price is above/below a certain threshold.

## 2) Data collection and preprocessing

## 3) Backtest setup and parameter search

## 4) CSMOM volume-weighted strategy

## 5) Results and discussion

## 6) Limitations and future work
