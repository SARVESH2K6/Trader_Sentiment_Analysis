# Trader Performance vs Market Sentiment

Analysis of how Bitcoin market sentiment (Fear/Greed) relates to trader behavior and performance on Hyperliquid.

Primetrade.ai Data Science/Analytics Intern — Round-0 Assignment.

## Contents

```
.
├── README.md                          <- this file (setup + how to run)
├── WRITEUP.md                         <- methodology, insights, strategy recommendations (Part B & C)
├── trader_sentiment_analysis.ipynb    <- main notebook, run top to bottom
├── data/
│   ├── fear_greed_index.csv
│   └── historical_data.csv
├── scripts/
│   ├── 01_data_prep.py                <- Part A: load, clean, align, build daily metrics
│   ├── 02_analysis.py                 <- Part B: Fear vs Greed stats + segmentation
│   ├── 03_charts.py                   <- chart generation
│   ├── 04_bonus_model.py              <- bonus: predictive model + clustering
│   └── build_notebook.py              <- assembles the .ipynb from the scripts above
└── outputs/
    ├── tables/                        <- CSV/markdown outputs (cleaned data, segments, reports)
    └── charts/                        <- 9 PNG charts
```

## Setup

Requires Python 3.10+.

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn tabulate jupyter nbconvert
```

## How to run

**Option A — Notebook (recommended):**

```bash
jupyter notebook trader_sentiment_analysis.ipynb
# Run All
```

**Option B — Scripts directly** (run from the project root, in order):

```bash
python3 scripts/01_data_prep.py     # Part A: cleans + merges data -> outputs/tables/
python3 scripts/02_analysis.py      # Part B: Fear vs Greed stats + segmentation
python3 scripts/03_charts.py        # generates 9 charts -> outputs/charts/
python3 scripts/04_bonus_model.py   # bonus predictive model + clustering
```

All outputs (cleaned tables, segment tables, markdown reports, and charts) are written to `outputs/`.

## Summary of approach

1. **Part A**: loaded both CSVs, documented shape/missing values/duplicates, parsed timestamps, aligned trade data to the Fear/Greed index at a daily grain, and built per-account daily metrics (PnL, win rate, avg trade size, trade count, long/short ratio).
2. **Part B**: compared performance and behavior across Fear/Greed/Neutral days (with significance tests), then segmented the 32 accounts into size, frequency, and consistency tiers and examined how each segment behaves differently across sentiment regimes.
3. **Part C**: derived two concrete, segment-specific rules of thumb from the data (see `WRITEUP.md` §3).
4. **Bonus**: trained a Random Forest to predict next-day account profitability from sentiment + behavior features, and ran k-means clustering to surface rough trader archetypes.

Full methodology, insights, and strategy recommendations are in **[WRITEUP.md](WRITEUP.md)**.
