# Volume-Weighted-CSMOM-Repo

## 1) Introduction

This strategy builds on the findings of [Huang, Sangiorgi & Urquhart (2024)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4825389). I first attempted to replicate the paper’s core idea using Binance Global as my data source and a universe of 77 cryptocurrencies selected from the top 100 by market capitalization (as of November 2021) that had a average daily trading volume >$1 million, excluding stablecoins. In my implementation, frequent portfolio rebalancing generated substantial turnover, and once transaction costs were assumed the strategy’s performance deteriorated, resulting in a negative Sharpe ratio.

I also tested a more selective approach that only took positions in assets exhibiting the strongest time-series momentum signals, but performance remained unstable across parameter choices. While differences in data construction between Binance Global and the paper’s data source (CoinMarketCap) and different time horizons may contribute to discrepancies, the primary driver of underperformance in my backtests appears to be turnover and transaction costs, which may not have been fully incorporated in the original study.

As a result, I decided to build on top of this paper using a hybrid time series momentum and cross sectional momentum signal construction process. Additionally, I incorporated a BTC moving average regime filter to develop net long and short positions when the BTC price is above/below a certain threshold.

Here is the OOS strategy performance net of Tcosts:

**Net Sharpe: 1.16**

**Gross Sharpe: 1.68**

**Max DD: 29.7%**

**Max DDD: 247 days** 

**Beta: 0.00027**

**Annualized Alpha: 0.31%**

**Alpha T-stat: 1.38**

**Information Ratio: 0.97**

**Note:-** A 20 BPS assumption was made to account for slippage and TCOST's. Additionally, performance was benchmarked against buy and hold BTC with the OOS period being 2023-11-19 to 2025-11-19.


## 2) Data collection and preprocessing

OHLCV data was sourced from Binance Global via its API. The universe consists of 77 cryptocurrencies selected from the top 100 by market capitalization (as of November 2021), excluding stablecoins and requiring average daily trading volume above $1M. These filters are intended to focus the strategy on more liquid assets and reduce the impact of illiquidity and microstructure noise.

A grid search was conducted across key strategy parameters (for example, lookback window, holding period, and the number of long and short positions) using the in-sample period 2021-11-19 to 2023-11-19. To reduce the risk of overfitting, all parameter selection was completed in-sample before reporting out-of-sample performance results.

## 3) CSMOM volume-weighted strategy

## 4) Results and discussion

## 5) Limitations and future work
