---
layout: default
title: Code & Implementation
description: Technical details of our March Madness prediction implementation
---

# Code & Implementation

This page provides technical details about our implementation, highlighting key code components and explaining how to use our repository.

## Repository Structure

Our project is organized as follows:

```
march-madness-prediction/
│
├── data/                    # Data files and documentation
├── models/                  # Model implementation notebooks
├── notebooks/               # Exploratory analysis notebooks
├── scripts/                 # Data collection scripts
├── images/                  # Visualizations
└── docs/                    # Documentation (this website)
```

## Key Components

### 1. Data Preparation

Our data pipeline transforms raw game data into feature-rich training examples. Here's a sample of how we process the data:

```python
# Calculate team statistics from game results
win_count = df_season_results.groupby(['Season', 'WTeamID']).count()
loss_count = df_season_results.groupby(['Season', 'LTeamID']).count()

# Calculate winning margins
win_margin = df_season_results.groupby(['Season', 'WTeamID']).mean().reset_index()
win_margin = win_margin[['Season', 'WTeamID', 'ScoreDiff']].rename(columns={'ScoreDiff': 'AverageWinMargin'})

# Calculate win percentage feature
df_features_season['WinPercentage'] = df_features_season['WinCount'] / (df_features_season['WinCount'] + df_features_season['LossCount'])

# Calculate average margin feature
df_features_season['GapAvg'] = (
    (df_features_season['WinCount'] * df_features_season['AverageWinMargin'] - 
     df_features_season['LossCount'] * df_features_season['AverageLossMargin']) / 
    (df_features_season['WinCount'] + df_features_season['LossCount'])
)
```

### 2. Feature Engineering

For each potential matchup, we engineer features based on team differentials:

```python
# Compute difference between teams for each feature
diff_cols = ['Seed', 
             'WinPercentage', 
             'GapAvg', 
             'AdjEM',
             'AdjO Eff',
             'AdjD Eff',
             'Championship Win Probability']

for col in diff_cols:
    df[col + 'Diff'] = df[col + 'A'] - df[col + 'B']
```

### 3. Model Training & Evaluation

We use a time-based cross-validation approach:

```python
def kfold_validation(df, df_test=None, verbose=0, mode="reg"):
    seasons = df['Season'].unique()
    log_loss_scores = []
    
    for season in seasons[1:]:
        # Training data uses previous seasons
        df_train = df[df['Season'] < season].reset_index(drop=True).copy()
        
        # Validation data uses current season
        df_val = df[df['Season'] == season].reset_index(drop=True).copy()
        
        # Feature normalization
        df_train, df_val = rescale(features, df_train, df_val)
        
        # Model selection
        if mode == "reg":
            model = LinearRegression()
        elif mode == "cls":
            model = LogisticRegression()
            
        # Model training
        model.fit(df_train[features], df_train[target_y])
        
        # Prediction and evaluation
        if mode == "reg":
            pred = model.predict(df_val[features])
            pred = (pred - pred.min()) / (pred.max() - pred.min())
        elif mode == "cls":
            pred = model.predict_proba(df_val[features])[:, 1]
            
        # Calculate log loss
        score = log_loss(df_val['WinA'].values, pred)
        log_loss_scores.append(score)
        
    return np.mean(log_loss_scores)
```

### 4. Web Scraping Tools

We implemented scrapers to collect additional data sources. Here's a simplified example of our Pomeroy ratings scraper:

```python
def scrape_pomeroy_ratings(year):
    """Scrape Pomeroy basketball ratings for a given year."""
    url = f"https://kenpom.com/index.php?y={year}"
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Find the ratings table
    table = soup.find('table', {'id': 'ratings-table'})
    
    # Extract team names and ratings
    teams = []
    for row in table.find_all('tr')[1:]:  # Skip header row
        cells = row.find_all('td')
        if len(cells) >= 8:
            team_data = {
                'Year': year,
                'Team': cells[1].text.strip(),
                'AdjEM': float(cells[4].text),
                'AdjO': float(cells[5].text),
                'AdjD': float(cells[7].text),
                # Extract other metrics...
            }
            teams.append(team_data)
    
    return pd.DataFrame(teams)
```

### 5. XGBoost Implementation

For our advanced models, we used XGBoost:

```python
import xgboost as xgb

# XGBoost for classification
model = xgb.XGBClassifier(
    objective='binary:logistic',
    max_depth=3,
    learning_rate=0.1,
    n_estimators=100
)

# Training
model.fit(X_train, y_train)

# Prediction
probabilities = model.predict_proba(X_test)[:, 1]

# Feature importance visualization
xgb.plot_importance(model)
plt.figure(figsize=(10, 6))
plt.show()
```

## Running the Code

To run our code, follow these steps:

1. **Set up the environment**:
   ```bash
   git clone https://github.com/yourusername/march-madness-prediction.git
   cd march-madness-prediction
   pip install -r requirements.txt
   ```

2. **Data preparation**:
   - Download the original dataset from Kaggle or use our processed data
   - Run data preparation scripts to generate feature sets

3. **Model exploration**:
   ```bash
   jupyter notebook models/betting_model.ipynb
   ```

4. **Generate predictions**:
   - The notebooks output prediction files for the tournament matchups
   - Results are saved as CSV files compatible with competition submission format

## Technologies Used

- **Python** - Primary programming language
- **Pandas** - Data manipulation and analysis
- **Scikit-learn** - Implementation of classification and regression models
- **XGBoost** - Gradient boosting framework
- **Matplotlib/Seaborn** - Data visualization
- **BeautifulSoup** - Web scraping for additional data sources
- **Jupyter Notebooks** - Interactive code development and documentation

## Future Improvements

We've identified several potential improvements for the project:

1. **Additional Features**:
   - Player-level statistics (NBA draft picks, experience)
   - Coach performance history
   - Tournament performance history
   
2. **Advanced Modeling**:
   - Ensemble methods combining multiple models
   - Neural networks for feature learning
   - Bayesian approaches for uncertainty quantification

3. **Real-time Updates**:
   - Integration with live APIs for in-season updates
   - Adjusting predictions based on tournament progression
