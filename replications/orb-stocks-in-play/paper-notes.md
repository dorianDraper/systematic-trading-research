# **A Profitable Day Trading Strategy for the U.S. Equity Market**
### Carlo Zarattini, Andrea Barbon, Andrew Aziz (2024)
#### The paper can be found [here](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4729284).

## Introduction
The Opening Range Breakout typically focuses on identifying the highest and lowest prices during the first n-minutes of trading, and then buying or selling when the stock breaks out of this range only in the direction of the opening range. For instance, a positive open suggests a long position upon breaking the high, while a negative open indicates a short position breaking the low. 

## Strategy Definition

### Base Strategy

The n-minute ORB has popular variants including 5-minute, 15-minute, 30-minute and 60-minute timeframes The authors conduct a thorough analysis of the profitability of ORB strategies with particular focus on the **5-minute ORB**. They introduced a crucial parameter to that ORB strategy: 

* If the **first 5-minute candlestick was bearish** (meaning it closed below its opening price), the system would only take **short position** (<ins>they would not go for a long position even if the price broke above the 5-minute opening range candlestick</ins>).
* Similarly, if the first 5-minute candlestick was **bullish** (meaning it closed above the opening price), they would stick to taking only a **long position** (<ins>they would avoid taking a short position, even if the price dropped below the 5-minute opening range candlestick</ins>).

The approach is to exclude penny stocks and other low-liquidity stocks and consider stocks that meet the following requirements:
```
1. Opening price > $5.
2. Average trading volume over the previous 14 days >= 1,000,000 shares/day.
3. ATR over the previous 14 days > $0.50.
```
#### Entry Conditions

With each eligible stock a stop order is placed at a level equal to the high/low of the 5-minute range, in the direction of the opening range. Example: if a stock had a **bullish** move within the first 5 minutes of trading (from 9:30 am until 9:35 am ET), place a stop order at the highest value of 5-minute opening range (aka 5-minute high). Conversely, if stock had **bearish** move within first 5 minutes, a stop oder placed at the lowest value of the 5-minute opening range (aka 5-minute low). In case of a **doji (open = close)** forming in the first 5 minutes, **no order was placed**. 


#### Stop Loss and Profit Target
**<ins>Step 1 — Determine the volatility (ATR)</ins>**

ATR = Average True Range, a volatility indicator introduced by J. Welles Wilder. It measures the typical daily price movement. The paper uses the example of a company called BLDR on January 22, 2024 which daily ATR = $5. The strategy sets the stop loss as **10% x ATR** so **0.10 x $5 = $0.50**, therefore the **Risk per share (R) = $0.50**. This means the maximum loss allowed per share is **$0.50**.
```
ATR = $5
10% x ATR → 0.10 x $5 = $0.50
Risk per share (R) = $0.50
```

**<ins>Step 2 — Entry price</ins>**

The trade is a short position triggered when price breaks the **low of the 5-minute opening range**.
```
Entry: 174.44
```

**<ins>Step 3 — Stop loss placement</ins>**

Since it is a short trade, the stop loss is above the entry.
Distance: 0.50 = 1R</br>
So: Stop = 174.44 + 0.50 = 174.94</br>
Meaning:
```
Event	             Price
Entry (short)	     174.44
Stop loss	         174.94
Risk	             $0.50 = 1R
```

**<ins>Step 4 — Exit price</ins>**

The trade is closed at the end of the day (EOD):
```
Exit: 167.63
Price movement: 174.44 - 167.63 = 6.81
```
Because it is a short trade, this downward move is profit.

**<ins>Step 5 — Convert profit to R multiples</ins>**

We divide the profit by the risk unit:
```
𝑅-multiple = Profit / Risk
= 6.81 / 0.50 = 13.62𝑅
```
So the trade produced: **+13.62R**

Meaning the profit was 13.62 times the initial risk.

#### What this means in practical trading terms

If the trader risks 1% of capital per trade, then:
Example account:
```
Account size = $25,000
Risk per trade = 1% = $250
```
Therefore:
```
Metric          	Value
Risk per trade	    $250
Trade result	    13.62R
Profit	            13.62 × $250 = $3,405
```

This illustrates why trend days dominate the profitability of ORB systems.

Most trades might be:
```
-1R
-1R
-1R
+13R
```

And the strategy is still strongly profitable.

#### The deeper implication

This example illustrates a core property of profitable strategies: **Positive skew**.

Return distribution looks like:
```
Many small losses
Few very large winners
```

Example:
```
Trade	  Result
Trade 1	  -1R
Trade 2	  -1R
Trade 3	  -1R
Trade 4	  +13.6R

Net = +10.6R
```
That is why **profit targets often damage trend strategies**: they cut the **right tail** of the distribution. If the stop loss was not reached intraday, they cosed the position at the end of the trading session (4:00 pm ET).

#### Position sizing
The capital deployed for a position would be 1% and the maximum leverage constraint at 4x in accordance with the majority of US FINRA regulated brokers. The starting capital of $25,000 and incorporated a commission cost of $0.0035 per share(entry-level commission fee charged by Interactive Brokers, Dec 2023).

With these filters and initial investment the resulting net profit was $7,500, during the same period a passive long position in S%P 500 would have equated about $50,000, the ORB underperformed but showed encouraging results (further detail at paper document).

#### Core principle
This strategy aims to identify assets that exhibit an abnormal imbalance between demand and supply in the first few minutes of the trading session. The hypothesis is that this imbalance will persist throughout the session, creating exploitable intraday trends. This abnormality can be assessed by comparing its opening range volume to its recent average.

### Stocks in Play Strategy: newly refined Base Strategy 

A stock is considered *in play* when it shows unusual trading activity throughout the day which often results in an expansion of its daily price range. The stock is typically expected to be in play in response to a major fundamental catalyst that prompts institutional investors to re-evaluate their financial positions in it, majors catalysts are: 

* Earnings reports
* FDA approvals
* Mergers/acquisitions 
* Alliances, partnerships
* Major contracts wins/loses
* Restructuring, layoffs
* Stock splits, buybacks
* ...

While this catalyst is often necessary to determine a *stock in play*, it is not always sufficietn, if the market has already priced in the catalyst, the institutional investors may not react significantly. To determine if a catalyst is causing unusually high trading activity is by using *Relative Volume*. This is a statistical comparison of the day's trading volume against the average volume from the previous days. The authors focused on the RV during the opening range: RV for each stock after the first 5 minutes of each trading day.

Building upon the previous basic filters they analyzed the relationship between the RV and the PnL in R with a strong positive correlation between the RV and PnL (net of commissions). The paper illustrates in **Figure 4** the correlations.

#### New filters

To enhance the effectiveness of the *Base Strategy* the authors proposed an additional constraint: focus only on stocks whose Relative Volume was at least 100%. Furthermore, they focused on those top 20 stocks experiencing the highest Relative Volume. Revised strategy incorporated the folliwng filters:
```
1. Opening price > $5.
2. Average trading volume over the previous 14 days >= 1,000,000 shares/day.
3. ATR over the previous 14 days > $0.50.
4. The Relative Volume had to be at least 100%.
5. Trade the stocks with the top 20 Relative Volume.
```
Following the same ORB 5-minute, stop loss, end-of-day exit, capital allocatedand leverage. With this strategy it outperformed against the passive buy-and-hold approach, growing to around $435,000. Further detail in the paper

#### Comparison to various time frames
The authors explored other popular time frames such as 15, 30, or 60 minutes. According to the results, the 5-minute significantly outperformed the other time frames as well as a passive exposure in the S&P 500. The authors don't provide a rationale for this performance but conclude that a plausible explanation might be that the shorter the time-frame that define the opening range, the greater the portion of the move captured by the ORB on trend days.

When comparing the 25 best and 25 worst performing stocks the authors highlight the importance of selecting stocks with high Relative Volume for day trading, additionally the popularity of these stocks might also increase the likelihood of price movements that are conducive to the ORB strategy, as these stocks are more susceptible to rapid shifts in sentiment and momentum.

#### Conclusions
* The authorse underscore the strategy's effectiveness in generating **consistent and uncorrelated returns**, addressing a long-standing debate about the feasibility of day trading as a sustainable income source. 
* They emphasize the critical role of selecting **stocks with substantial trading activity, driven by underlying fundamental news**.
* Their rigorous statistical analysis, **grounded in economic rationale rather than retrospective optimization**, suggests that the their findings could mantain their robustness and significance in future applications.
* While the ORB strategy presents a promising avenue for day traders, it is crucial to approach it with thorough research, disciplined risk management, and a clear understanding of market dynamics.