# Heavy-Bet Anomaly Study — Football Odds vs Results  
**Six Major Leagues · Three Seasons · Calibration/Anomaly Analysis**

---

## Contents

- Project Overview  
- Motivation  
- Data Sources  
- Research Questions & Hypotheses  
- Methodology  
- Analysis Plan

---

## Project Overview
This project explores the relationship between betting market expectations and real match outcomes in 5 major European football leagues to look for an answer to the widespread speculations about rigged referees, match fairness, and potential suspicious matches, the project aims to investigate how "stronger" teams perform relative to the bookmaker odds.

Using multi season, real world football datasets, we try to examine whether teams that experience significant support in the betting markets (measured through opening and closing odds movement) tend to meet, exceed, or underperform their expected probabilities. Since actual bet volume is not publicly available, line movement serves as a practical proxy for identifying the “heavily-backed side.”

The central research question is: **"Do teams that receive strong betting market support before kick-off underperform relative to their final implied probabilities?"**

To answer this, the project conducts an end-to-end analysis pipeline:
- Collecting match results from the five major European leagues across three consecutive seasons,  
- Merging them with bookmaker opening and closing odds,  
- Converting odds into implied probabilities,  
- Identifying matches with significant line movement,  
- Performing calibration tests and anomaly detection to compare expected vs. actual outcomes.

The aim is not to confirm wrongdoing or intentional underperformance, but rather to provide a systematic, transparent, and reproducible statistical assessment. By grounding the discussion in data—rather than anecdotes or sensational claims—the project seeks to offer clearer insight into how well betting markets anticipate real match results and whether heavily-backed teams deviate meaningfully from market expectations.

---

## Motivation

The relationship between betting market expectations and actual football match outcomes has become an increasingly relevant topic in sports analytics. As a data science enthusiast with a strong interest in football-related behavioral patterns, I am motivated to explore this issue through rigorous, data-driven methods rather than public speculation. Understanding how odds evolve, how bettors collectively react to information, and whether these reactions align with real performance can provide valuable insights into market efficiency, forecasting accuracy, and potential biases. Such analysis not only contributes to a more objective understanding of match expectations, but also helps inform broader discussions about transparency, fairness, and the reliability of market-based predictions in modern football.



## Data Sources

### 1. Football-Data.co.uk Dataset (Kaggle Mirror)

**Source:** https://www.kaggle.com/datasets/jamieandrews/footballdatacouk  
**Original Website:** https://www.football-data.co.uk/

**Contains:**

- Match results (goals, outcomes)  
- Dates and league information  
- Pre-match **opening odds** from multiple bookmakers  

**Usage in this project:**

- Serves as the **primary match results dataset**  
- Provides **opening odds** needed to compute initial implied probabilities  
- Used to build the foundation of the merged dataset (team names, leagues, dates)

This dataset is clean, standardized, and covers major European leagues.

---

### 2. Beat-the-Bookie Worldwide Football Dataset

**Source:** https://www.kaggle.com/datasets/austro/beat-the-bookie-worldwide-football-dataset  

**Contains:**

- **Opening and closing odds** from multiple bookmakers  
- Multi-league global football coverage  
- High-resolution pricing data near kickoff  

**Usage in this project:**

- Provides **closing odds**, essential for line-movement analysis  
- Enables identification of the **“heavily-backed” side**  
- Allows comparison of opening vs closing implied probabilities  
- Used to test market efficiency and bias

This dataset fills the gap that Football-Data.co.uk lacks (closing odds).

---
## Data Collection and Preparation

### 1. Data Cleaning

- Standardized team names across both datasets to ensure accurate merging.  
- Converted all date fields to a consistent `datetime` format.  
- Filtered matches to include only the six major European leagues and the selected three seasons.  
- Removed rows with missing or inconsistent odds values (e.g., missing home/draw/away odds).  
- Eliminated impossible or erroneous entries such as negative odds or matches duplicated across bookmakers.  

---

### 2. Aggregation & Integration

- **Football-Data.co.uk:** Extracted match results and opening odds as the base dataset.  
- **Beat-the-Bookie:** Extracted multi-bookmaker opening and closing odds to capture line movement.  
- Created a canonical match index using *(date, home team, away team)* as the join key.  
- Merged datasets on this index to produce a unified structure containing:  
  - match results  
  - opening odds  
  - closing odds  
  - league metadata  
- Ensured bookmaker-level odds were aggregated appropriately (e.g., averaging across bookmakers when needed).  

---

### 3. Feature Engineering

- Computed **vig-free implied probabilities** for both opening and closing odds.  
- Calculated **line movement** features:  
  - ΔOdds (closing − opening)  
  - ΔProbability (closing_prob − opening_prob)  
- Identified the **heavily-backed side** as the outcome with the largest positive probability shift.  
- Created binary classification targets:  
  - `HeavySide_Won` (1 if heavily-backed side wins, else 0)  
- Generated additional match-level features such as  
  - `IsFavorite`  
  - `FavoriteProbabilityGap`  
  - `OutcomeCorrectness` (whether the market-predicted favorite actually won)  
- Normalized numeric columns for machine learning models and encoded categorical variables (teams, leagues).  

---

### 4. Data Enrichment

- Integrated bookmaker metadata to examine pricing consistency across firms.  
- Added league-level contextual variables (e.g., average goals per league per season).  
- Derived match difficulty indicators such as  
  - relative team strength (computed through rolling win percentages).  
- Enriched the dataset with temporal features:  
  - `Season`  
  - `Matchday`  
  - `WeekOfYear`  
- Resulted in a more comprehensive dataset suitable for calibration studies, statistical testing, and ML modeling.  

---

## Research Questions & Hypotheses

This project investigates several central questions:

### RQ1 — Are football betting markets efficient?

Do implied probabilities from bookmakers match actual match outcomes?  
→ Tested via calibration analysis & chi-square goodness-of-fit.

### RQ2 — Does the “heavily-backed side” underperform?

If bettors strongly back one side (sharp odds drop), does that team  
actually perform better—or worse—than implied?

**Hypothesis:**  
Bettors systematically overreact → heavily-backed sides underperform relative to implied probability.

### RQ3 — Do opening or closing odds better reflect true probabilities?

Closing odds are expected to be more efficient as they incorporate more information.

### RQ4 — Is there a “favorite–longshot bias” in football?

Bookmakers and bettors may undervalue favorites and overvalue underdogs.

---

## Methodology

1. **Data Collection**
   - Football-Data dataset for match results + opening odds  
   - Beat-the-Bookie dataset for opening + closing odds  
   - Filtered for major European leagues (e.g., Premier League, La Liga, Serie A, Bundesliga, Ligue 1, Eredivisie)

2. **Data Cleaning & Matching**
   - Normalize team names  
   - Merge on (date, home team, away team)  
   - Remove mismatches or incomplete rows  

3. **Odds Transformation**
   - Compute **vig-free implied probabilities** from opening and closing odds  
   - Compute probability deltas (closing – opening)  

4. **Heavy-Bet Identification**
   - For each match, identify the side with the largest increase in implied probability  
   - Label this side as **`heavy_bet_side`**

5. **Exploratory Data Analysis**
   - Descriptive statistics for odds and outcomes  
   - Distribution of opening & closing odds  
   - Line movement patterns across leagues and seasons  
   - Home vs away win probability patterns  

6. **Hypothesis Testing**
   - Market calibration tests (implied vs actual frequencies)  
   - Heavy-bet underperformance test (z-test / chi-square test of proportions)  
   - Favorite–longshot bias analysis  

---

## Analysis Plan

The project proceeds in three phases:

### Phase 1 — Data Collection & EDA
- Build merged dataset from both sources  
- Basic descriptive statistics  
- Visualization of odds, implied probabilities, and line movement  
- League-by-league comparison

### Phase 2 — Statistical Hypothesis Tests
- Test whether markets are calibrated  
- Test whether heavily-backed sides underperform relative to implied probabilities  
- Examine favorite–longshot bias in different odds ranges

### Phase 3 — Interpretation & Reporting
- Summarize the key findings  
- Discuss market efficiency and behavioral biases  
- Describe limitations (data coverage, matching issues, model assumptions)  
- Suggest directions for future research or model extensions  

---
