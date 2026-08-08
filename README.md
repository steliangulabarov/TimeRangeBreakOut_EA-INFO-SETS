# TimeRangeBreakOut_EA-INFO-SETS
How It Works
TimeBreak is a time-based breakout system. It does not use indicators, moving averages, or price patterns. It trades specific pre-market and opening session ranges when institutional volume enters the market.
Core principles:
One setup per day, per instrument
Hard stop loss on every trade
No martingale, no grid, no averaging
Fully customizable — you control days, hours, trailing, break-even, stop loss, take profit, time exits, candle confirmation vs instant entry
The EA is a framework, not a black box. I share what works for me. You can test your own ideas.
Strategy Logic
DAX Setup
Time Range: 2 hours before Frankfurt cash market open (07:00–09:00 Germany time)
Why this time?
Before the Frankfurt open, the DAX futures market enters a quiet consolidation period. When the cash market opens at 09:00, institutional volume floods in. If there is a strong directional move, we want to enter fast, secure the position quickly (break-even), and let it run until midday.
The logic step by step:
07:00–09:00 — The EA records the high and low of the 2-hour pre-market range
09:00 — Frankfurt opens. If price breaks above the range high, we go long. If it breaks below the range low, we go short
Entry — Instant on breakout OR on candle close confirmation (configurable)
Risk/Reward — 1:4 ratio. Stop loss is placed at the opposite side of the range
Break-Even — When price reaches +1R profit, stop loss moves to entry price (BE). If the move reverses, we exit at $0, not a loss
Exit — Take profit at 4R OR time-based exit at 12:00 Germany (midday, before the banking lunch lull) OR at 16:00 Germany (end of European session)
Why exit at midday?
Most of the DAX daily movement happens in the first 2–3 hours after the open. After 12:00, volatility often dies and price chops sideways. We capture the morning impulse and avoid the afternoon noise.
Key stats (backtest 2022–Aug 2026, 1% risk):
Table
Metric	Value
Total Net Profit	~$30,558
Total Trades	1,158
Profit Factor	1.22
Recovery Factor	3.61
Max Drawdown	~29–30%
Win Rate	~15.6%
Avg Winner	~$927
Avg Loser	~$138
Max Consecutive Losses	14
Note on win rate: The low win rate is because break-even trades (exiting at $0) are counted as "losses" in MT5 backtests. The true "non-losing" rate is higher. The system wins rarely but wins big.
S&P 500 / Nasdaq Setup
Both indices share the same logic. The setfiles work identically on S&P 500 and Nasdaq.
Time Range: First hour after New York open (09:30–10:30 New York Time)
Why this time?
The first hour after the US open is the most volatile period of the day. European traders are still active, and US institutional volume enters. When the price breaks the 1-hour opening range, it often continues in that direction for the rest of the session.
Two setfiles included:
Set 1: Standard (Long + Short)
Trades both directions
If price breaks above the 1-hour range → Long
If price breaks below the 1-hour range → Short
Exit at 15:45 New York Time (15 minutes before US close)
Captures the full directional impulse of the day
Key stats (backtest 2022–Aug 2026, 1% risk):
Table
Metric	Value
Total Net Profit	~$15,821
Total Trades	1,151
Profit Factor	1.15
Recovery Factor	2.82
Max Drawdown	~20%
Win Rate	~46.9%
Avg Winner	~$225
Avg Loser	~$170
Max Consecutive Losses	8
Set 2: Only Long
Only long trades. No short positions
Same entry logic: break above the 1-hour range → Long
Exit at 15:45 New York Time
Why only long?
Fundamentally, equity indices have an upward bias over time due to:
Inflation (money loses purchasing power, assets rise in nominal terms)
Central bank monetary expansion (money printing)
Economic growth and earnings growth
Since the 1970s, the long-term trend of major indices is up
Statistically, buying dips and riding the upward drift has produced better risk-adjusted returns than trying to short the market. Shorting works in bear markets, but bull markets last longer and recover faster.
This set captures the structural upward bias with lower drawdown and a smoother equity curve.
Key stats (backtest 2022–Aug 2026, 1% risk):
Table
Metric	Value
Total Net Profit	~$8,038
Total Trades	754
Profit Factor	1.22
Recovery Factor	3.74
Max Drawdown	~12%
Win Rate	~39.7%
Avg Winner	~$150
Avg Loser	~$78
Max Consecutive Losses	12
I personally use this Only Long set at 2% risk per trade.
Risk Management
This is the most important section. Read it twice.
The EA has built-in risk controls, but it does NOT automatically reduce risk during drawdown. That is your job.
Option A: Set & Forget (Small Risk)
If you do not want to check your account weekly, use this:
Risk per trade: 0.5% of account equity
Fixed. Never change it.
Expected max drawdown: ~12–15%
Expected annual return: ~10–12%
Time horizon: 3+ years
This is boring. This is slow. This is how you survive.
Option B: Dynamic Risk Reduction
This is how I trade. It requires 5 minutes per week.
Every Sunday (or your chosen day), check your account equity vs. the all-time high. Adjust the EA's risk setting according to this table:
Table
Drawdown from ATH	Risk per Trade
0% to –15%	0.9%
–15% to –25%	0.7%
–25% to –35%	0.5%
Above –35%	STOP. Reassess.
Why this works:
When the system enters a losing streak, you automatically reduce exposure. You survive the storm with capital intact. When conditions improve, you are still in the game.
Rules:
Baseline max: 1% per trade. Never above 2%.
Hard stop loss on every trade. No exceptions.
1 position per instrument per day. No manual "recovery" trades.
5–15 consecutive losses are statistically normal. Your risk must be small enough that 15 losses in a row does not destroy your account.
Evaluate performance over 12+ months, not weeks.
Example:
At 1% risk × 15 losses = 15% drawdown. Uncomfortable but survivable.
At 5% risk × 15 losses = 75% drawdown. Account destroyed.
The math is simple. The consequences are real.
Account Structure Rules
CRITICAL: Use separate accounts for each instrument.
Table
Account	Instrument	Setfile	Risk
Account 1	DAX	DAX 2h Pre-Market	Your choice (0.5%–1%)
Account 2	S&P 500 OR Nasdaq	Only Long OR Standard	Your choice (0.5%–2%)
Do NOT run multiple sets on the same account.
Do NOT run DAX and S&P 500 on the same account.
Why?
Each instrument has its own volatility profile, drawdown timing, and risk characteristics. If you combine them:
Drawdowns can overlap and compound
You cannot apply the dynamic risk table correctly (which ATH do you measure from?)
You lose clarity on which set is performing and which is not
You panic and override rules
One instrument. One set. One account.
If you want to trade both DAX and S&P 500, open two accounts. Split your capital. Apply the risk rules independently to each.
Broker Time Zone Setup
This system lives or dies by correct timing. The EA uses your broker's server time, not your local time.
How to check your broker time:
Open MetaTrader 5
Look at the Market Watch window (top left)
Check the time displayed next to any symbol
Compare this to UTC
Market times in UTC:
Table
Event	Summer UTC	Winter UTC
DAX Range Start	05:00	06:00
DAX Range End	07:00	08:00
DAX Close Option 1	10:00	11:00
DAX Close Option 2	14:00	15:00
S&P 500 Range Start	13:30	14:30
S&P 500 Range End	14:30	15:30
S&P 500 Close	19:45	20:45
Calculate your broker time:
Find the difference between your broker's server time and UTC. Add that offset to the UTC times above.
Examples:
Broker GMT+2 (winter): DAX Range = 07:00–09:00
Broker GMT+3 (summer): DAX Range = 08:00–10:00
Broker GMT+0: DAX Range = 05:00–07:00
If your broker time does not match the setfile, you MUST adjust the EA parameters. Trading at the wrong hours means trading random noise instead of real volume.
If you are unsure, contact me before going live.
Recommended Brokers
I personally trade through Admiral Markets on MetaTrader 5.
The following brokers provide suitable conditions:
Admiral Markets
IC Markets
Vantage Markets International
Pepperstone
Requirements:
ECN or RAW spread account
Low spreads during European and US session opens
Reliable execution without significant slippage
Correct server time alignment
I am not affiliated with any broker. Choose what works for you.
Backtest Results
All backtests use 97–99% tick modeling quality with real tick data over the period 2022 – August 2026.
Table
Set	Instrument	Net Profit	Max DD	Win Rate	Profit Factor
DAX 2h Pre-Market	DAX	~$30,558	~29–30%	~15.6%	1.22
S&P 500 Standard	S&P 500 / Nasdaq	~$15,821	~20%	~46.9%	1.15
S&P 500 Only Long	S&P 500 / Nasdaq	~$8,038	~12%	~39.7%	1.22
Important: Backtests exclude commissions and swaps. Real results will be lower. Past performance does not guarantee future results.
Live trading context:
I have traded this system live on futures (Interactive Brokers) and CFDs (Admiral Markets MT5). The MT5 live track record is approximately 140–150 trades — sufficient to confirm the system functions in live conditions, but not statistically significant as standalone proof. Use the multi-year backtests as your primary reference.
Important Disclaimers
Trading carries substantial risk of loss. Only trade with capital you can afford to lose entirely.
Past performance does not guarantee future results. Markets change. Edges degrade.
Backtests are hypothetical. Real trading involves spreads, slippage, commissions, swaps, and execution delays that reduce returns.
The author is not responsible for losses resulting from improper risk settings, manual intervention, incorrect time zone configuration, or deviation from the recommended money management rules.
This is not financial advice. This is a tool. How you use it determines your outcome.
Contact
MQL5 Market Chat: I respond to all messages
Discord: NorthComfort#1614
I am a systematic trader building a broader framework. I code what I trade, and I trade what I code.
Questions about time zones, broker compatibility, or risk settings? Message me before going live.
