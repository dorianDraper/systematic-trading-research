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

1. Opening price > $5.
2. Average trading volume over the previous 14 days >= 1,000,000 shares/day.
3. ATR over the previous 14 days > $0.50.

#### Entry Conditions

With each eligible stock a stop order is placed at a level equal to the high/low of the 5-minute range, in the direction of the opening range. Example: if a stock had a **bullish** move within the first 5 minutes of trading (from 9:30 am until 9:35 am ET), place a stop order at the highest value of 5-minute opening range (aka 5-minute high). Conversely, if stock had **bearish** move within first 5 minutes, a stop oder placed at the lowest value of the 5-minute opening range (aka 5-minute low). In case of a **doji (open = close)** forming in the first 5 minutes, **no order was placed**. 


#### Stop Loss and Profit Target
**<ins>Step 1 — Determine the volatility (ATR)</ins>**

ATR = Average True Range, a volatility indicator introduced by J. Welles Wilder. It measures the typical daily price movement. The paper uses the example of a company called BLDR on January 22, 2024 which daily ATR = $5. The strategy sets the stop loss as 10% x ATR so 0.10 x $5 = $0.50, therefore the **Risk per share (R) = $0.50**. This means the maximum loss allowed per share is **$0.50**.
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

This example illustrates a core property of profitable strategies:

**Positive skew**
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

hat is why **profit targets often damage trend strategies**: they cut the **right tail** of the distribution. If the stop loss was not reached intraday, they cosed the position at the end of the trading session (4:00 pm ET).


