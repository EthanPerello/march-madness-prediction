---
layout: default
title: Models & Results
description: Detailed results of our March Madness prediction models
---

# Models & Results

## Model Performance Summary

We developed multiple machine learning models to predict NCAA tournament outcomes. The table below summarizes the performance of each model measured by average log loss (lower is better):

<div class="results-container">
  <table class="results-table">
    <thead>
      <tr>
        <th>Model</th>
        <th>Using 538 Rankings</th>
        <th>Using Betting Data & Pomeroy Ratings</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Regression</td>
        <td>0.578</td>
        <td>0.521</td>
      </tr>
      <tr>
        <td>Classification</td>
        <td>0.566</td>
        <td><strong>0.459</strong></td>
      </tr>
      <tr>
        <td>XGBoost Regression</td>
        <td>0.570</td>
        <td>0.488</td>
      </tr>
      <tr>
        <td>XGBoost Classification</td>
        <td>0.630</td>
        <td>0.470</td>
      </tr>
    </tbody>
  </table>
</div>

![Model Performance Comparison](/march-madness-prediction/assets/model_comparison.png)

## Detailed Model Analysis

### Baseline Models with FiveThirtyEight Ratings

Our initial models used a limited feature set with data from 2016-2021:
- Seed difference
- FiveThirtyEight ratings difference
- Win percentage difference
- Average margin difference

#### Linear Regression Model
- **Approach**: Predict score difference, convert to probability
- **Average Log Loss**: 0.578
- **Performance by Year**:
  - 2017: 0.580
  - 2018: 0.596
  - 2019: 0.530
  - 2021: 0.607

#### Logistic Regression Model
- **Approach**: Directly predict win probability
- **Average Log Loss**: 0.566
- **Performance by Year**:
  - 2017: 0.547
  - 2018: 0.593
  - 2019: 0.509
  - 2021: 0.614

### Enhanced Models with Betting Odds & Pomeroy Ratings

Our advanced models incorporated additional data sources and extended the historical range to 2002-2021:
- Added 17 Pomeroy rating features
- Incorporated pre-tournament betting odds
- Expanded to 20 years of tournament data

#### Logistic Regression with Enhanced Features
- **Approach**: Classification with expanded feature set
- **Average Log Loss**: 0.459
- **Key Observation**: 19% improvement over baseline
- **Performance by Year**:
  - Best performance in 2019 (0.378)
  - Consistent improvement across all seasons

#### XGBoost Regression with Enhanced Features
- **Approach**: Gradient boosted trees for score difference
- **Average Log Loss**: 0.488
- **Key Insight**: Better at capturing non-linear relationships
- **Decision Tree Visualization**:
  ![XGBoost Decision Tree](/march-madness-prediction/assets/feature_importance.png)

## Feature Importance Analysis

Our analysis of feature importance revealed several key insights:

![Feature Importance](/march-madness-prediction/assets/feature_importance.png)

1. **Betting Odds Feature**: Championship win probability difference was the single most predictive feature, highlighting the wisdom of crowds in sports prediction

2. **Pomeroy Efficiency Metrics**: Team efficiency ratings were highly predictive, particularly:
   - Adjusted efficiency margin difference
   - Offensive efficiency difference 
   - Defensive efficiency difference

3. **Traditional Metrics**: Seeding and win percentage remained important but were enhanced by the advanced metrics

## Year-by-Year Performance

The graph below shows how our best model performed across different tournament years:

![Year by Year Performance](/march-madness-prediction/assets/yearly_performance.png)

Key observations:
- Most consistent performance in recent years
- 2019 tournament was the most predictable (0.378 log loss)
- 2011 tournament was the most challenging (0.560 log loss)

## Conclusion

Our results demonstrate that:

1. **Classification outperformed regression** approaches for tournament prediction
2. **Market-based features** (betting odds) provided significant predictive power
3. **Advanced team metrics** from Pomeroy ratings substantially improved model accuracy
4. Our best model (**0.459 log loss**) would have been competitive in the Kaggle competition, which had a winning score of 0.515 in the previous year

These findings suggest that combining expert systems (Pomeroy ratings), market wisdom (betting odds), and traditional statistics creates a powerful predictive framework for tournament outcomes.