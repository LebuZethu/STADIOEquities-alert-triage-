# Model 1 Performance: Logistic Regression

This document presents the performance of Model 1 (Logistic Regression) applied
to the publicly available Credit Card Fraud Detection dataset described in Part A.

## Related file

- **Notebook:** `notebooks/05_performance_comparison.ipynb`

This notebook is self-contained. It loads the processed data, retrains both
models, and computes all performance metrics. The Model 1 results in this
document are produced by the "Model 1: Logistic Regression - Performance"
section of that notebook.

## How to run

1. Ensure the project dependencies are installed (`pip install -r requirements.txt`).
2. Run `notebooks/02_feature_engineering.ipynb` first, so the processed data
   exists in `data/processed_fe/`.
3. Open `notebooks/05_performance_comparison.ipynb` and run the cells in order.
   The Model 1 performance cell prints the classification report, the fraud-class
   metrics, and the confusion matrix values.

## Evaluation approach

Model 1 is evaluated on the untouched test set of 56,962 transactions, which
keeps its real fraud rate of 0.172 percent (98 fraudulent transactions).
Accuracy is deliberately not used as the headline measure, because a model that
labels every transaction as legitimate would still score around 99.8 percent
while catching no fraud. The metrics below focus on the fraud class, which is
the class of interest.

## Performance metrics (fraud class)

| Metric | Value |
|---|---|
| Precision | 0.0555 |
| Recall | 0.9184 |
| F1 score | 0.1046 |
| ROC-AUC | 0.9705 |
| PR-AUC | 0.7236 |

## Confusion matrix

|  | Predicted Legitimate | Predicted Fraud |
|---|---|---|
| **Actual Legitimate** | 55,331 | 1,533 |
| **Actual Fraud** | 8 | 90 |

- True Negatives (legitimate correctly identified): 55,331
- False Positives (legitimate wrongly flagged as fraud): 1,533
- False Negatives (fraud missed): 8
- True Positives (fraud correctly caught): 90

## Interpretation

Model 1 achieves a high recall of 0.9184, meaning it catches 90 of the 98
fraudulent transactions. However, its precision is very low at 0.0555, because
it also flags 1,533 legitimate transactions as fraudulent. The low F1 score of
0.1046 reflects this imbalance between high recall and low precision.

The ROC-AUC of 0.9705 is strong, but for a heavily imbalanced problem the PR-AUC
of 0.7236 is the more informative single measure, and it shows there is clear
room for improvement.

For STADIOEquities, Model 1 would catch most fraud but would overwhelm the compliance team with 1,533 false alarms. This illustrates the core
challenge of the project: catching fraud is not difficult in isolation, but
doing so without generating an unmanageable number of false positives is. This
performance sets the baseline that Model 2 is measured against.