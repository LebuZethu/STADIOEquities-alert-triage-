# Comparison of Model 1 and Model 2

This document compares the performance of Model 1 (Logistic Regression) and
Model 2 (Random Forest) on the publicly available Credit Card Fraud Detection
dataset described in Part A, and identifies which model is more suitable for the
STADIOEquities alert-triage problem.

## Related file

- **Notebook:** `notebooks/05_performance_comparison.ipynb`

The comparison in this document is produced by the "Model Comparison" section of
that notebook, which builds the side-by-side comparison table and the comparison
bar chart.

## How to run

1. Ensure the project dependencies are installed (`pip install -r requirements.txt`).
2. Run `notebooks/02_feature_engineering.ipynb` first, so the processed data
   exists in `data/processed_fe/`.
3. Open `notebooks/05_performance_comparison.ipynb` and run the cells in order.
   Both models are trained and evaluated on identical data, then compared in the
   comparison table and bar chart at the end of the notebook.

## Fair comparison

Both models were trained on the same SMOTE-balanced training set and evaluated
on the same untouched test set of 56,962 transactions (98 fraudulent). Using
identical data for both models means any difference in the results comes from the
models themselves, not from differences in the data, so the comparison is fair.

## Comparison table (fraud class)

| Metric | Model 1: Logistic Regression | Model 2: Random Forest |
|---|---|---|
| Precision | 0.0555 | 0.7523 |
| Recall | 0.9184 | 0.8367 |
| F1 score | 0.1046 | 0.7923 |
| ROC-AUC | 0.9705 | 0.9837 |
| PR-AUC | 0.7236 | 0.8639 |
| False positives | 1,533 | 27 |
| Fraud caught (of 98) | 90 | 82 |

## Analysis

**Precision.** This is the largest difference between the two models. Model 1
has a precision of just 0.0555, while Model 2 reaches 0.7523. In real terms,
Model 1 flags 1,533 legitimate transactions as fraud, whereas Model 2 flags only
27. This is a reduction in false positives of more than 98 percent.

**Recall.** Model 1 has slightly higher recall (0.9184 versus 0.8367). It
catches 90 of the 98 fraud cases, while Model 2 catches 82. Model 1 therefore
catches 8 more fraud cases than Model 2.

**Balanced measures.** On the metrics that balance precision and recall, Model 2
is clearly ahead. Its F1 score is 0.7923 against 0.1046, and its PR-AUC, which is
the most informative single measure for an imbalanced dataset, is 0.8639 against
0.7236. Its ROC-AUC is also slightly higher.

**The trade-off.** Model 2 gives up a small amount of recall in exchange for a
very large gain in precision. The practical question is whether catching 8 more
fraud cases is worth generating roughly 1,500 additional false positives, which
is the cost Model 1 pays for its higher recall.

## Conclusion

For the STADIOEquities alert-triage problem, Model 2 (Random Forest) is the more
suitable model. The purpose of the project is to reduce the false-positive
burden that overwhelms the compliance team, while still detecting the majority
of genuine fraud. Model 2 achieves exactly this. It catches most of the fraud
(82 of 98) while flagging only 27 legitimate transactions, a volume that human
investigators can realistically review.

Model 1 catches marginally more fraud, but it does so by producing an
unmanageable number of false alarms, which is the very problem the project set
out to solve. On the balance of the evidence, and given the business objective,
Model 2 is the recommended model.