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

This strategy combines time-series momentum (TSMOM) and cross-sectional momentum (CSMOM) within a volume-weighted portfolio construction framework. TSMOM measures each asset’s momentum using its own trailing returns, while CSMOM forms relative bets by ranking assets on momentum each day and going long the strongest performers and short the weakest. This is then used to construct the Winner's (Long's) Minus Loser's (Short's) portolfio and positions are then scaled by trading volume so that more liquid assets receive larger weights, which helps reduce exposure to illiquid names and improves tradability under transaction costs. All OHLCV data used is computed on a daily frequency. 

**Signal Construction**

The signal construction function takes in the daily return data for each coin and computes the trailing compounded return over a lookback window L:

<img width="172" height="37" alt="image" src="https://github.com/user-attachments/assets/637b2c42-6723-4af1-8866-d838d1979ebf" />

The function then ranks all coins by this trailing return and goes long the top k coins and short the bottom k coins thus ensuring strategy is market neutral when regime filter conditions are met. To prevent lookahead bias, this signal is then shifted before it's used for portfolio construction.

**Portfolio Construction**

The dataframe computed in the signal construction step is used to define Winners (signal=+1) and Losers (signal=-1), which within each dataframe we weight in direct proportionallity to prior day trading volume. We then normalize each leg so weights sum to 1. Again to ensure no lookahead bias the volume data is shifted so that we use yesterday's trading volume in this portfolio construction process. To prevent rapid turnover from eating away at strategies edge a holding period of H is enforced, thus preventing short term noise from impacting returns.

Because Crypto markets are higly regime and sentiment driven I decided to use the BTC simple moving average as a regime filter, which is widely used in technical anlalysis as well by discretionary traders. When BTC prices is below its 200-day moving average, I classified it as a bear market and took a net short position, whereas if the BTC price is above the 365 day moving average I classified it as a bull market and took a net long position. If the price of BTC is between these thresholds the strategy is market neutral. 


## 4) Results and discussion

## 5) Limitations and future work
