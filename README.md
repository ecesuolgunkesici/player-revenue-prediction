<p>
  <a href="#">
    <img alt="Lang" src="https://img.shields.io/static/v1?label=lang&message=en&color=blue" target="_blank" />
  </a>
</p>

# 🎮 D7 Revenue Prediction for Mobile Game Users

This project analyzes gameplay and ad revenue events from a mobile game and implements a robust D7 revenue prediction model using **XGBoost** and **Random Forest** regressors. The model predicts each user's 7-day revenue based on their first 24-hour activity and ad interactions.

## 📦 Setup & Installation

### 1️⃣ Install Dependencies

Ensure you have Python 3.8+ installed, then run:

```bash
pip install -r requirements.txt
```

⚠️ On some macOS systems (especially with M1/M2 chips), you may need:

```
bash brew install libomp
```
## :clipboard: Project Structure
This project was executed in five major stages:
  - *Revenue Segmentation:* Users were grouped by their D0, D3, and D7 revenue windows. Revenue segments were created using quantile-based or clustering (Birch) methods.
  - *Segment-Level Analysis:* For each user segment, computed some KPIs 
  - *Exploratory Analysis:* Used Looker Studio to create visual dashboards
  - *Sessionization:* Each user’s event was assigned a session number using a 30-minute inactivity threshold, mimicking session tracking logic from analytics tools.
  - *D7 Revenue Prediction:* Trained regression models using only first 24h user behavior to predict total D7 revenue.

## 🧠 Feature Engineering

To predict D7 revenue based solely on early user activity, engineered several behavioral and monetization-related features from the **first 24 hours** of interaction. some of them are as follows:

| Feature Name             | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `game_start_count`       | Number of times the user launched the game (`GameStart` events)             |
| `level_start_count`      | Number of levels started (`level_start` events)                             |
| `ad_impression_count`    | Total number of ad views (based on `AdImpressionRevenue` event count)       |
| `ad_revenue_24h`         | Sum of ad revenue generated within the first 24 hours                       |
| `active_hours`           | Count of unique active hours (how many different hours the user was active) |
| `session_count`          | Number of unique sessions (based on 30-minute inactivity rule)              |
| `avg_revenue_per_level`    | D7 revenue divided by total levels started — monetization efficiency        |
| `active_days`              | Number of unique days user interacted with the app (based on `GameStart`)   |
| `early_engagement_score`   | Composite score combining level count, session count, and ad activity        |

## 🔍 Model Training & Evaluation Results

I trained and evaluated multiple regression models to predict user-level **Day 7 (D7) Revenue** using behavioral and monetization features derived from the first 24 hours after installation. The goal was to select a **robust**, **non-overfitting** model that generalizes well.

### ✅ Baseline Model Comparison

| Model          | MAE    | RMSE   | MSE    | R²     |  
|----------------|--------|--------|--------|--------|
| Random Forest  | 0.0929 | 0.2239 | 0.0501 | 0.7941 | 
| XGBoost        | 0.0884 | 0.1868 | 0.0349 | 0.8164 | 

➡️ **XGBoost** outperformed Random Forest in every metric and was selected for hyperparameter optimization.

---

### 🔧 Hyperparameter Tuning (XGBoost)

Using grid search with cross-validation, the best hyperparameters were:

```python
{
  'learning_rate': 0.1,
  'max_depth': 3,
  'n_estimators': 100,
  'subsample': 0.8
}




