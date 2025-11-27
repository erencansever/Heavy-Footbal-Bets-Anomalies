# Heavy-Bet Anomaly Study — Football Odds vs Results  
**Six Major Leagues · Three Seasons · Calibration/Anomaly Analysis**

## Project Overview
This project explores the relationship between betting market expectations and real match outcomes in major European football leagues. Motivated by the widespread speculations about referees, match fairness, and potential market manipulation, the project aims to investigate how strongly-backed teams perform relative to the probabilities implied by bookmaker odds.

Using multi-season, real-world football datasets, I examine whether teams that experience significant support in the betting markets—measured through opening and closing odds movement—tend to meet, exceed, or underperform their expected probabilities. Since actual bet volume is not publicly available, line movement serves as a practical proxy for identifying the “heavily-backed side.”

The central research question is: **"Do teams that receive strong betting market support before kick-off underperform relative to their final implied probabilities?"**

To answer this, the project conducts an end-to-end analysis pipeline:
- Collecting match results from the six major European leagues across three consecutive seasons,  
- Merging them with multi-bookmaker opening and closing odds,  
- Converting odds into vig-free implied probabilities,  
- Identifying matches with significant line movement,  
- Performing calibration tests and anomaly detection to compare expected vs. actual outcomes.

The aim is not to confirm wrongdoing or intentional underperformance, but rather to provide a systematic, transparent, and reproducible statistical assessment. By grounding the discussion in data—rather than anecdotes or sensational claims—the project seeks to offer clearer insight into how well betting markets anticipate real match results and whether heavily-backed teams deviate meaningfully from market expectations.

**Data Collection** 


# DSA210-FootballBettingMarketProject

Comparative analysis of football betting markets using opening & closing odds, market efficiency tests, and line-movement–based behavioral indicators.  
A data science project investigating heavy-bet anomalies, bookmaker calibration, and probability distortions across major European football leagues.  
DSA210 Term Project.

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

This project aims to examine the behavior, efficiency, and hidden patterns within football betting markets by comparing **opening and closing bookmaker odds**, analyzing **line movement**, and testing whether the “heavily-backed” side exhibits systematic bias or underperformance.

By combining match results with multi-bookmaker opening/closing odds, the project investigates:

- how betting markets adjust probabilities prior to kickoff,  
- whether odds movements reflect meaningful information,  
- and if certain teams, leagues, or match situations systematically distort market expectations.

Using data analysis, visualization, and statistical hypothesis testing, the project seeks to provide data-driven insights into the dynamics of sports betting markets—especially focusing on **market efficiency**, **anomaly detection**, and **behavioral patterns of bettors**.

---

## Motivation

I am working on this project because football betting markets provide a rich environment for studying **probability estimation**, **market reactions**, and **collective behavior of bettors**.

While betting odds are often assumed to be efficient predictors, practical applications reveal several interesting questions:

- Are opening odds unbiased predictors of match outcomes?  
- How much does the market update before kickoff, and is this update rational?  
- Does a strong line movement (“heavy betting”) actually predict better performance, or do bettors systematically overreact?

Understanding whether markets behave rationally or exhibit biases can contribute to:

### Market Efficiency Analysis
Evaluating whether implied probabilities match actual win percentages.

### Behavioral Finance Perspective
Studying how bettors respond to information, narratives, and team reputation.

### Risk and Forecasting Applications
Understanding calibration and bias helps build more accurate prediction models.

By performing a quantitative analysis of these relationships, the project connects sports analytics, behavioral economics, and data science.

---

## Data Sources

I use two complementary datasets that together provide all the components needed for market analysis.

---

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

### Why two datasets?

These datasets complement each other:

| Component              | Football-Data | Beat-the-Bookie |
|------------------------|---------------|------------------|
| Match results          | ✔             | ✔ (but less clean) |
| Opening odds           | ✔             | ✔ |
| Closing odds           | ❌            | ✔ |
| League consistency     | Excellent     | Good |
| Use in project         | Base data     | Line movement + efficiency |

Using both ensures that the final merged dataset includes:

- match results  
- opening odds  
- closing odds  
- vig-free implied probabilities  
- line movement  
- heavy-bet indicators  

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
