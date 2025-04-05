---
layout: default
title: March Madness Prediction Challenge
description: Machine learning models to predict NCAA basketball tournament outcomes
---

# March Madness Prediction Challenge

## Predicting NCAA Tournament Outcomes with Machine Learning

![Basketball Tournament](/march-madness-prediction/assets/basketball.jpg)

This project explores the use of machine learning to predict outcomes in the NCAA Men's Basketball Tournament (March Madness). We developed multiple prediction models that leverage historical game data, team performance metrics, expert ratings, and betting market information.

## Project Highlights

- 🏆 Achieved a **0.459 log loss score**, outperforming the previous year's winning submission (0.515)
- 📊 Engineered 22 predictive features from multiple data sources
- 🔍 Compared multiple machine learning approaches including classification and regression models
- 📈 Found that betting odds significantly improved prediction accuracy

## Key Results

Our comprehensive model comparison revealed that classification models consistently outperformed regression approaches, and that incorporating betting odds data provided substantial predictive power:

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

## Project Sections

- [Methodology](methodology.html) - Learn about our data sources, feature engineering approach, and model development process.
- [Models & Results](results.html) - Detailed explanation of each model and comprehensive performance analysis.
- [Code & Implementation](code.html) - Technical details and implementation notes.
- [View on GitHub](https://github.com/yourusername/march-madness-prediction) - Browse the complete codebase.

## Team

This project was developed by **Samuel Kellum** and **Ethan Perello** under the mentorship of **Prof. Mattei** for a course taught by **Prof. Zheng**.