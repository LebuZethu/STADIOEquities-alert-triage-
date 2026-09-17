# Model 2: Random Forest

This document describes the second model applied to the STADIOEquities fraud
detection and AML alert triage project, and compares it against Model 1.

## Related file

- **Notebook:** `notebooks/04_model2_random_forest.ipynb`

## How to run

1. Ensure the project dependencies are installed (`pip install -r requirements.txt`).
2. Run `notebooks/02_feature_engineering.ipynb` first, so that the processed data
   exists in `data/processed_fe/`.
3. Open `notebooks/04_model2_random_forest.ipynb` and run the cells in order from
   top to bottom. The notebook loads the processed data, trains the model,
   evaluates it, and produces a confusion matrix.

## Why random forest

Random forest was chosen as the second model to compare with the logistic
regression baseline. Where logistic regression separates the two classes with a
single linear boundary, a random forest builds many decision trees and combines
their votes, which allows it to capture more complex, non-linear patterns in the
data. The literature review found random forest to be the strongest performer
for this kind of problem, so it is a well-motivated choice to test against the
baseline.

## Model details

- **Technique:** Random Forest (scikit-learn `RandomForestClassifier`).
- **Hyperparameters:**
  - `n_estimators=100` — the number of decision trees built and combined. More
    trees generally give more stable predictions.
  - `max_depth=20` — the maximum depth of each tree. Limiting the depth helps
    prevent the trees from overfitting the training data.
  - `n_jobs=-1` — trains the trees in parallel across all available CPU cores to
    reduce training time.
  - `random_state=42` — fixes the randomness so the result is reproducible.
- **Training data:** the same SMOTE-balanced training set from
  `data/processed_fe/` used for Model 1 (454,902 transactions).
- **Evaluation data:** the same untouched test set of 56,962 transactions used
  for Model 1. Using identical data for both models makes the comparison fair.

## Evaluation metrics

As with Model 1, accuracy is not used as the main measure of performance because
of the class imbalance. The metrics below focus on the fraud class.

| Metric (fraud class) | Value |
|---|---|
| Precision | 0.75 |
| Recall | 0.84 |
| F1 score | 0.79 |
| ROC-AUC | 0.9837 |
| PR-AUC | 0.8639 |

## Confusion matrix

|  | Predicted Legitimate | Predicted Fraud |
|---|---|---|
| **Actual Legitimate** | 56,837 | 27 |
| **Actual Fraud** | 16 | 82 |

## Comparison with Model 1

The two models were trained and evaluated on identical data, so their results
can be compared directly.

| Metric (fraud class) | Model 1: Logistic Regression | Model 2: Random Forest |
|---|---|---|
| Precision | 0.06 | 0.75 |
| Recall | 0.92 | 0.84 |
| F1 score | 0.10 | 0.79 |
| ROC-AUC | 0.9705 | 0.9837 |
| PR-AUC | 0.7236 | 0.8639 |
| False positives | 1,533 | 27 |
| Fraud caught (of 98) | 90 | 82 |

## Interpretation

The random forest dramatically improves on the baseline where it matters most
for this project. It reduces the number of false positives from 1,533 to 27, a
reduction of more than 98 percent, while still catching 82 of the 98 fraudulent
transactions. Its precision rises from 6 percent to 75 percent, and its F1 score
rises from 0.10 to 0.79.

The one trade-off is a modest drop in recall, from 92 percent to 84 percent,
meaning the random forest misses 8 more fraud cases than logistic regression
does. In the context of this project, this is a favourable trade. Logistic
regression caught marginally more fraud but overwhelmed the compliance team with
false alarms, which is the exact problem the project set out to solve. The random
forest catches the large majority of fraud while flagging only 27 legitimate
transactions, which is a workable volume for human investigators to review.

On this evidence, the random forest is the more suitable model for
STADIOEquities' alert-triage problem, because it directly addresses the
false-positive burden that motivated the project, without a large sacrifice in
fraud detection.