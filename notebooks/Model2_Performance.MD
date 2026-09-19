# Model 2 Performance: Random Forest

This document presents the performance of Model 2 (Random Forest) applied to the
publicly available Credit Card Fraud Detection dataset described in Part A.

## Related file

- **Notebook:** `notebooks/05_performance_comparison.ipynb`

This notebook is self-contained. It loads the processed data, retrains both
models, and computes all performance metrics. The Model 2 results in this
document are produced by the "Model 2: Random Forest - Performance" section of
that notebook.

## How to run

1. Ensure the project dependencies are installed (`pip install -r requirements.txt`).
2. Run `notebooks/02_feature_engineering.ipynb` first, so the processed data
   exists in `data/processed_fe/`.
3. Open `notebooks/05_performance_comparison.ipynb` and run the cells in order.
   The Model 2 performance cell prints the classification report, the fraud-class
   metrics, and the confusion matrix values.

## Evaluation approach

Model 2 is evaluated on the same untouched test set of 56,962 transactions used
for Model 1, which keeps its real fraud rate of 0.172 percent (98 fraudulent
transactions). Using identical evaluation data for both models ensures the
results are directly comparable. As with Model 1, accuracy is not used as the
headline measure, because of the severe class imbalance. The metrics below focus
on the fraud class.

## Performance metrics (fraud class)

| Metric | Value |
|---|---|
| Precision | 0.7523 |
| Recall | 0.8367 |
| F1 score | 0.7923 |
| ROC-AUC | 0.9837 |
| PR-AUC | 0.8639 |

## Confusion matrix

|  | Predicted Legitimate | Predicted Fraud |
|---|---|---|
| **Actual Legitimate** | 56,837 | 27 |
| **Actual Fraud** | 16 | 82 |

- True Negatives (legitimate correctly identified): 56,837
- False Positives (legitimate wrongly flagged as fraud): 27
- False Negatives (fraud missed): 16
- True Positives (fraud correctly caught): 82

## Interpretation

Model 2 achieves a precision of 0.7523 and a recall of 0.8367, giving a balanced
F1 score of 0.7923. It catches 82 of the 98 fraudulent transactions while
flagging only 27 legitimate transactions as fraudulent.

Its ROC-AUC of 0.9837 and, more importantly for an imbalanced problem, its
PR-AUC of 0.8639 are both strong, indicating good ranking ability between the
fraud and legitimate classes.

In practical terms for STADIOEquities, Model 2 catches the large majority of
fraud while keeping false positives to a very low level. A volume of 27 flagged
legitimate transactions is realistic for a compliance team to review by hand,
which is exactly the outcome the project aims for. Compared with Model 1, this
model gives up a small amount of recall in exchange for a very large improvement
in precision. The full comparison is presented in Comparison.MD.