# Trader Performance vs Market Sentiment — Write-up

## 1. Methodology

**Data**: Two datasets were combined — (1) the Bitcoin Fear/Greed index (2,644 daily records, 2018–2025) and (2) Hyperliquid historical trade fills (211,224 rows, 32 accounts, 246 symbols, May 2023–May 2025). Both were clean: zero missing values and zero true duplicate rows in either file. One data-quality artifact was caught and corrected for: `Trade ID` is stored in the source CSV in scientific notation, which collides distinct IDs when parsed as a float — it was therefore excluded as an identity/dedup key, and exact full-row duplication was used instead (found: 0).

**Alignment**: Trade timestamps (`Timestamp IST`) were parsed and collapsed to a daily grain, then joined to the Fear/Greed index by date. 479 of 480 trading days (100% of resulting trader-days) had matched sentiment.

**Metrics** were built per account per day: trade count, total/avg Closed PnL, win rate (share of PnL-realizing trades that were profitable), average trade size (USD), total volume, buy/sell (long/short) trade ratio. The 5-way sentiment label was collapsed into **Fear** (Fear + Extreme Fear), **Greed** (Greed + Extreme Greed), and **Neutral** for comparison.

**Segmentation**: Accounts were split into three independent two-way tiers using medians/quantiles across the full trading history:
- **Size tier** (proxy for position sizing/leverage appetite — the raw data has no explicit leverage field, so average trade size in USD is used as the closest available proxy)
- **Frequency tier** (trades per active day)
- **Consistency tier** (mean daily PnL divided by its standard deviation — a Sharpe-like stability score)

**Statistical testing**: Mann-Whitney U tests (non-parametric, robust to the heavy-tailed PnL distribution) were used to compare Fear vs Greed day distributions.

## 2. Key Insights

1. **Fear days show higher and more dispersed PnL, but the difference is not strongly significant on its own.** Average daily PnL is higher in Fear ($5,185) than Greed ($4,144) or Neutral ($3,439) regimes, but a Mann-Whitney test on the full population gives p≈0.06 — suggestive, not conclusive, at the whole-market level. The real signal appears once you segment (see #3).

2. **Traders are meaningfully more active and more long-biased during Fear.** Average trades/day is ~105 in Fear vs ~77 in Greed, and the long/short ratio rises to ~2.24 in Fear vs ~1.63 in Greed. In other words, traders don't retreat during Fear — they trade more, and lean more long, which is a classic contrarian/dip-buying pattern rather than risk-off behavior.

3. **The Fear-day PnL edge is concentrated almost entirely in high-size, high-frequency, and "consistent" traders — not the average trader.** Splitting by segment tells a much sharper story than the whole-market average:
   - High-size traders average **$9,540**/day in Fear vs **$3,347** in Greed — roughly 3x.
   - Frequent traders average **$7,955**/day in Fear vs **$4,942** in Greed.
   - "Consistent" traders (top half by PnL-stability score) average **$11,490**/day in Fear vs **$4,826** in Greed — more than 2x, and this segment also holds the highest win rate in every regime (0.86–0.88).
   - By contrast, low-size, infrequent, and inconsistent traders show flat or even *reversed* patterns (inconsistent traders actually do better in Greed than Fear: $3,820 vs $1,856).

   This is the most actionable finding: **Fear-day outperformance is a segment-specific effect, not a market-wide one.**

4. **The drawdown proxy (avg of each account's worst single day) is deeper in Greed than Fear** (−$26,728 vs −$16,653), despite Greed having lower average trades and long/short ratio. This suggests that when things go wrong on Greed days, they go wrong bigger — consistent with crowded, leverage-heavy long positioning unwinding sharply.

## 3. Strategy Recommendations

**Rule 1 — Scale up only if you're already a "consistent" or high-frequency trader, and only on Fear days.**
The data shows Fear-day PnL outperformance is real but concentrated: consistent and frequent traders roughly double their Greed-day PnL during Fear, while inconsistent/low-activity traders see no such lift (and are better off in Greed). Rule of thumb: *"If your trailing PnL-consistency score is in the top half, increase size or add positions during Fear regimes; if it's in the bottom half, keep exposure flat or reduced regardless of sentiment."*

**Rule 2 — Cap long-side sizing in Greed for less consistent traders, since the tail risk is asymmetric.**
The average drawdown proxy is deeper in Greed than in Fear even though average activity is lower, implying concentrated long exposure is punished harder when Greed reverses. Rule of thumb: *"Inconsistent or low-size-tier traders should reduce long-bias sizing specifically during Greed regimes, using a smaller max-position cap than in Fear, since historical worst-case daily losses are largest there."*

## 4. Bonus Results

- **Next-day profitability model** (Random Forest, sentiment + prior-day behavior features): test ROC-AUC 0.647 vs a 0.698 majority-class baseline accuracy — the model beats random ranking but the behavior features (trade count, avg size, total PnL, volume) dominate over sentiment features in importance, meaning **sentiment alone is a weak standalone predictor of next-day profitability; a trader's own recent behavior carries more signal.**
- **K-means clustering (k=3)** on trade count, size, PnL, PnL volatility, win rate, and frequency surfaced three rough archetypes: a large "baseline" group (29 accounts, moderate size/frequency), a small high-size/high-PnL-volatility "whale" cluster (1 account), and a high-frequency/lower-size "scalper" cluster (2 accounts) with the best win rate (0.88). Given only 32 accounts total, this clustering is directional rather than statistically robust — useful for further segmentation ideas, not a production-ready archetype system.

## 5. Limitations

- No explicit leverage field exists in the trade data; "size tier" is a proxy based on average USD trade size, not true margin/leverage.
- 32 accounts is a small sample for account-level segmentation and clustering; segment-level results should be treated as directional rather than statistically bulletproof.
- Fear/Greed index reflects overall BTC market sentiment, not sentiment specific to the 246 traded symbols, some of which are altcoins that may not track BTC sentiment closely.
