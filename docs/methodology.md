---
layout: default
title: Methodology
description: Our approach to predicting March Madness outcomes
---

# Methodology

## Problem Statement

The NCAA Men's Basketball Tournament, known as March Madness, is a single-elimination tournament featuring 68 college basketball teams. Our goal was to build machine learning models to predict the probability of each team winning in every possible matchup scenario.

The predictions were evaluated using log loss, which heavily penalizes high confidence incorrect predictions. This makes it an ideal metric for probabilistic predictions in tournament settings.

## Data Sources

Our models leveraged several key data sources:

### 1. Historical Game Data (2002-2022)
- Regular season games (104,978 games)
- Tournament games (1,245 games)
- Features: teams, scores, locations

### 2. Team Performance Metrics
- Wins and losses
- Scoring margins
- Win percentages

### 3. Expert Rating Systems
- **FiveThirtyEight Ratings**: Statistical ratings from 2016-2022
- **Pomeroy Ratings**: Advanced team efficiency metrics from 2002-2022, including:
  - Adjusted offensive efficiency
  - Adjusted defensive efficiency
  - Adjusted tempo
  - Strength of schedule
  - Luck factor

### 4. Betting Market Data
- Pre-tournament championship odds from 2002-2022
- Converting moneyline odds to implied win probabilities

## Feature Engineering

We engineered features by calculating the difference between paired teams across multiple dimensions:

![Feature Engineering Process](assets/feature_engineering.jpg)

Key features included:
- Seed difference
- Win percentage difference
- Average margin difference
- Rating system differences (17 features from Pomeroy)
- Championship win probability difference (from betting odds)

## Model Development

We explored four primary modeling approaches:

### 1. Linear Regression
- **Target variable**: Score difference
- **Transformation**: Converting predicted score differences to win probabilities

### 2. Logistic Regression
- **Target variable**: Win/loss classification (binary)
- **Output**: Direct win probability prediction

### 3. XGBoost Regression
- **Target variable**: Score difference
- **Advantages**: Captures non-linear relationships and feature interactions

### 4. XGBoost Classification
- **Target variable**: Win/loss classification (binary)
- **Advantages**: Handles complex feature relationships in classification tasks

## Validation Strategy

We implemented a time-based validation approach to respect the temporal nature of sports prediction:

- Used previous seasons as training data
- Validated on each subsequent season
- Repeated for all tournament seasons from 2003-2021
- Averaged log loss scores across seasons

This approach prevents data leakage and provides a realistic evaluation of how models would perform in practice, where you can only use past data to predict future outcomes.

## Data Collection Tools

For the Pomeroy ratings and betting odds data that weren't readily available, we created custom web scrapers:

- **Pomeroy Ratings Scraper**: Collected 21 team metrics across 20 seasons
- **Betting Odds Scraper**: Gathered pre-tournament championship odds for all teams

This additional data proved crucial in improving model performance beyond baseline approaches.
