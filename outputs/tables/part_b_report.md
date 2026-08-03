## Part B — Analysis

### 1. Performance: Fear vs Greed

| sentiment_group   |   n_trader_days |   avg_daily_pnl |   median_daily_pnl |   avg_win_rate |   pnl_std |
|:------------------|----------------:|----------------:|-------------------:|---------------:|----------:|
| Fear              |             790 |         5185.15 |            122.737 |          0.842 |   31224.1 |
| Greed             |            1174 |         4144.21 |            265.248 |          0.856 |   29252   |
| Neutral           |             376 |         3438.62 |            167.552 |          0.836 |   17447.9 |

Mann-Whitney U test on daily PnL (Fear vs Greed): U=440966, p=0.0618
Mann-Whitney U test on win rate (Fear vs Greed): U=248690, p=0.2644

**Drawdown proxy** (avg of each account's worst daily PnL, by sentiment):
| sentiment_group   |   total_pnl |
|:------------------|------------:|
| Fear              |   -16652.6  |
| Greed             |   -26728.1  |
| Neutral           |    -7949.36 |

### 2. Behavior: Fear vs Greed

| sentiment_group   |   avg_trades_per_day |   avg_trade_size_usd |   avg_long_short_ratio |   avg_volume_usd |
|:------------------|---------------------:|---------------------:|-----------------------:|-----------------:|
| Fear              |               105.36 |              8529.86 |                   2.24 |           756720 |
| Greed             |                76.91 |              5954.63 |                   1.63 |           351829 |
| Neutral           |               100.23 |              6963.69 |                   2.23 |           479367 |

### 3. Trader Segments


**Size/Leverage-proxy segment**
| size_tier   |   n_accounts |   avg_total_pnl |   avg_win_rate |   avg_trades_per_day |
|:------------|-------------:|----------------:|---------------:|---------------------:|
| Low size    |           16 |          335527 |           0.85 |               126.87 |
| High size   |           16 |          305378 |           0.83 |                98.3  |

**Frequency segment**
| freq_tier   |   n_accounts |   avg_total_pnl |   avg_win_rate |   avg_trades_per_day |
|:------------|-------------:|----------------:|---------------:|---------------------:|
| Infrequent  |           16 |          257052 |           0.84 |                32.77 |
| Frequent    |           16 |          383853 |           0.84 |               192.4  |

**Consistency segment**
| consistency_tier   |   n_accounts |   avg_total_pnl |   avg_win_rate |   avg_trades_per_day |
|:-------------------|-------------:|----------------:|---------------:|---------------------:|
| Inconsistent       |           16 |          276823 |           0.77 |                93.16 |
| Consistent         |           16 |          364083 |           0.91 |               132.01 |

### 4. Segment behavior across sentiment


**Size tier x Sentiment**
|                          |   avg_daily_pnl |   avg_win_rate |   avg_trades |
|:-------------------------|----------------:|---------------:|-------------:|
| ('Low size', 'Fear')     |         2575.66 |          0.842 |      111.931 |
| ('Low size', 'Greed')    |         4589.85 |          0.842 |       96.792 |
| ('Low size', 'Neutral')  |         2655.21 |          0.809 |      121.925 |
| ('High size', 'Fear')    |         9540.17 |          0.843 |       94.402 |
| ('High size', 'Greed')   |         3347.14 |          0.896 |       41.356 |
| ('High size', 'Neutral') |         4837.14 |          0.892 |       61.496 |

**Frequency tier x Sentiment**
|                           |   avg_daily_pnl |   avg_win_rate |   avg_trades |
|:--------------------------|----------------:|---------------:|-------------:|
| ('Infrequent', 'Fear')    |         2524.84 |          0.816 |       33.809 |
| ('Infrequent', 'Greed')   |         3590.23 |          0.847 |       38.525 |
| ('Infrequent', 'Neutral') |         2891.88 |          0.835 |       45.643 |
| ('Frequent', 'Fear')      |         7955.44 |          0.863 |      179.876 |
| ('Frequent', 'Greed')     |         4942.35 |          0.866 |      132.218 |
| ('Frequent', 'Neutral')   |         4130.28 |          0.836 |      169.283 |

**Consistency tier x Sentiment**
|                             |   avg_daily_pnl |   avg_win_rate |   avg_trades |
|:----------------------------|----------------:|---------------:|-------------:|
| ('Inconsistent', 'Fear')    |         1855.77 |          0.822 |       88.923 |
| ('Inconsistent', 'Greed')   |         3820.38 |          0.851 |       71.515 |
| ('Inconsistent', 'Neutral') |         1694.52 |          0.812 |       82.316 |
| ('Consistent', 'Fear')      |        11490.2  |          0.878 |      136.498 |
| ('Consistent', 'Greed')     |         4826.14 |          0.865 |       88.278 |
| ('Consistent', 'Neutral')   |         7026.08 |          0.876 |      137.073 |