# Notebooks

This folder contains the modelling notebooks and their documentation for the
STADIOEquities fraud detection and AML alert triage project (SS2, Parts B and C).

## Contents

| File | Purpose |
|---|---|
| `01_preprocessing.ipynb` | Data cleaning, scaling, stratified split, and SMOTE |
| `Preprocessing.MD` | Documentation for the preprocessing notebook |
| `02_feature_engineering.ipynb` | Creates hour-of-day and log-amount features |
| `FeatureEngineering.MD` | Documentation for the feature engineering notebook |
| `03_model1_logistic_regression.ipynb` | Model 1: Logistic Regression |
| `Model1.MD` | Documentation for Model 1 |
| `04_model2_random_forest.ipynb` | Model 2: Random Forest |
| `Model2.MD` | Documentation for Model 2 |
| `05_performance_comparison.ipynb` | Performance metrics and model comparison |
| `Model1Performance.MD` | Model 1 performance results |
| `Model2Performance.MD` | Model 2 performance results |
| `Comparison.MD` | Comparison of Model 1 and Model 2 |

## How to run

1. Install dependencies from the repository root: `pip install -r requirements.txt`
2. Place `creditcard.csv` in the `data/` folder (see the main README for the source).
3. Run the notebooks in numbered order (01 to 05), each from top to bottom.