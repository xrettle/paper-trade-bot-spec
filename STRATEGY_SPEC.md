# Strategy Specification: "Sector Rotation Momentum"

## Concept
This strategy is based on **Cross-Sectional Momentum**. It assumes that assets which have outperformed in the recent past (e.g., last 6-12 months) will continue to outperform in the near future. It rotates capital into the strongest US sectors while using a "Regime Filter" (Broad Market Trend) to exit to cash during bear markets.

## Universe
The strategy trades a fixed list of liquid US Sector ETFs.

- **Risk Assets (Sectors)**:
    - `XLK`: Technology
    - `XLF`: Financials
    - `XLE`: Energy
    - `XLV`: Health Care
    - `XLI`: Industrials
    - `XLP`: Consumer Staples
    - `XLY`: Consumer Discretionary
    - `XLU`: Utilities
    - `XLB`: Materials
- **Benchmark / Filter**: `SPY` (S&P 500)
- **Safe Asset**: `BIL` (Treasury Bills) or Cash.

## Logic Rules (Deterministic)

### 1. Regime Filter (Market Health)
**Rule**: Is `SPY` closing price > `SPY` 200-day Simple Moving Average (SMA)?
- **YES**: Market is "Healthy". Proceed to Selection.
- **NO**: Market is "Bearish". Move 100% of portfolio to `BIL`/Cash.

### 2. Momentum Calculation (Selection)
If Market is Healthy:
1.  For each Sector ETF, calculate **Momentum Score**.
    - *Formula*: `(Current_Price / Price_126_Trading_Days_Ago) - 1`
    - (Approx. 6 months lookback).
2.  Rank ETFs by Score (Highest to Lowest).
3.  Select the **Top 3** ETFs.

### 3. Allocation
- If Healthy: Allocate **33.3%** of equity to each of the Top 3 ETFs.
- If Bearish: Allocate **100%** to Cash/Safe Asset.

### 4. Rebalancing Schedule
- **Frequency**: Monthly.
- **Timing**: First trading day of the month, 30 minutes after market open (to avoid opening volatility).

## Risk Management (Hard Constraints)
These rules override the strategy logic:
- **Max Drawdown Stop**: If Account Equity drops > 15% from High Water Mark, liquidate all to cash and pause bot (requires manual restart).
- **Position Limit**: No single asset can exceed 40% of portfolio (buffer for the 33% target).
- **Wash Sale Avoidance**: (Optional) Do not buy back an asset sold at a loss within 30 days.

## Backtesting Expectations
- **Metric Goals**: 
    - Sharpe Ratio > 0.8
    - Max Drawdown < 20%
- **Benchmarks**: Compare vs. Buy-and-Hold SPY.

## Evaluation
Before live paper trading, run a backtest using `yfinance` data (free) for the period 2010-Present.
