# Heavy-Bet Anomaly Study — Football Odds vs Results  
**Six Major Leagues · Three Seasons · Calibration/Anomaly Analysis**

## Project Overview
Public debate around football often includes claims that **referees influence outcomes** or that **major teams underperform intentionally** in coordination with betting markets. This project does **not** attempt to prove intent or wrongdoing. Instead, it asks a narrower, testable question:

> **Do sides that appear to be *heavily backed before kick-off* underperform relative to their market-implied (vig-free) winning probabilities?**

Because true staking/volume data (“most performed bets”) is rarely public, the project **approximates heavily backed sides using odds movement** from opening to closing prices (aggregated across bookmakers). We then compare realized outcomes to the market’s final implied probabilities and quantify any **calibration gaps / statistical anomalies** at match, team, league, and season levels.

## Scope
- **Leagues:** Premier League, La Liga, Serie A, Bundesliga, Ligue 1, Süper Lig  
- **Seasons:** Three consecutive seasons (e.g., 2021–22, 2022–23, 2023–24)  
- **Market:** Pre-match **1X2** (win/draw/win); extensions (O/U, BTTS) may be considered later.

## Data Sources (to be added by the user)
- **Match results + closing odds:** football-data.co.uk mirrors available on open data portals/Kaggle.
- **Opening & time-series odds:** Kaggle datasets that track pre-match odds across time (used to compute opening probabilities and odds movement).
> The repository will reference sources; users must download data per original licenses.

## What We Wi
