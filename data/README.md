# March Madness Prediction Dataset

This directory contains the datasets used for our March Madness prediction models.

## Data Sources

### 1. Kaggle Competition Data
Original source: [Kaggle Men's March Madness Competition](https://www.kaggle.com/competitions/mens-march-mania-2022/data)

Files:
- `MTeams.csv` - Team identification information
- `MRegularSeasonCompactResults.csv` - Regular season game results (2003-2022)
- `MNCAATourneyCompactResults.csv` - NCAA tournament game results (2003-2022)
- `MNCAATourneySeeds.csv` - Tournament seeding information (2003-2022)
- `MSampleSubmissionStage1.csv` - Submission format for the competition

### 2. FiveThirtyEight Ratings
- `538ratingsMen.csv` - Team ratings from the FiveThirtyEight prediction system (2016-2022)

### 3. Pomeroy Ratings
- `pomeroy_data.csv` - Advanced team metrics scraped from KenPom.com (2003-2022)

This dataset includes multiple rating systems:
- Adjusted Efficiency Margin (AdjEM) - Overall team strength
- Adjusted Offensive Efficiency (AdjO) - Points scored per 100 possessions
- Adjusted Defensive Efficiency (AdjD) - Points allowed per 100 possessions
- Adjusted Tempo (AdjT) - Possessions per 40 minutes
- Luck - Deviation from expected winning percentage
- Strength of Schedule metrics

### 4. Betting Odds Data
- `betting_data.csv` - Pre-tournament championship odds (2003-2022)

Scraped from Sports Odds History, this data provides the "wisdom of crowds" through betting markets.

## Processed Data

For our models, we've created several processed datasets that combine these sources:

- `features_538.csv` - Features using only FiveThirtyEight ratings
- `features_full.csv` - Combined features from all data sources

## Data Dictionary

### Game Results Data
- `Season` - Season of the game
- `DayNum` - Day number relative to the start of the season
- `WTeamID` - Winning team ID
- `WScore` - Winning team score
- `LTeamID` - Losing team ID
- `LScore` - Losing team score
- `ScoreDiff` - Score differential (WScore - LScore)

### Team Performance Features
- `WinPercentage` - Ratio of wins to total games
- `GapAvg` - Average score differential (weighted by wins/losses)

### Pomeroy Rating Features
- `AdjEM` - Adjusted Efficiency Margin
- `AdjO Eff` - Adjusted Offensive Efficiency
- `AdjD Eff` - Adjusted Defensive Efficiency
- `AdjTempo` - Adjusted Tempo
- `Luck` - Deviation from expected record
- Multiple ranking features for each metric

### Betting Data Features
- `Championship Win Probability` - Implied probability from moneyline odds

## Notes

- Due to data licensing restrictions, some raw data files aren't included in this repository
- The processed data includes all features needed for model training
- No data is available for 2020 due to tournament cancellation (COVID-19)
