# Model 1: Logistic Regression

This document describes the first model applied to the STADIOEquities fraud
detection and AML alert triage project.

## Related file

- **Notebook:** `notebooks/03_model1_logistic_regression.ipynb`

## How to run

1. Ensure the project dependencies are installed (`pip install -r requirements.txt`).
2. Run `notebooks/02_feature_engineering.ipynb` first, so that the processed data
   exists in `data/processed_fe/`.
3. Open `notebooks/03_model1_logistic_regression.ipynb` and run the cells in
   order from top to bottom. The notebook loads the processed data, trains the
   model, evaluates it, and produces a confusion matrix.

## Why logistic regression

Logistic regression was chosen as the first model because it serves as an
interpretable baseline. It is one of the most transparent classifiers, which
means the influence of each feature on a prediction can be understood directly.
This supports the need, identified in the literature review, for a triage model
whose decisions can be explained to compliance investigators. It also provides a
benchmark that the second model can be compared against.

## Model details

- **Technique:** Logistic Regression (scikit-learn `LogisticRegression`).
- **Hyperparameters:**
  - `max_iter=1000` — the maximum number of training iterations. The default of
    100 is not always enough for the model to converge on a dataset of this size,
    so it was raised to 1000.
  - `random_state=42` — fixes the randomness so the result is reproducible.
- **Training data:** the SMOTE-balanced training set from `data/processed_fe/`,
  which contains 454,902 transactions with an equal number of legitimate and
  fraudulent cases.
- **Evaluation data:** the untouched test set of 56,962 transactions, which
  keeps its real fraud rate of 0.172 percent.

## Evaluation metrics

Accuracy is deliberately not used as the main measure of performance, because
the test set is so imbalanced that a model predicting "legitimate" for every
transaction would still score around 99.8 percent while catching no fraud at
all. The following metrics are used instead, focusing on the fraud class.

| Metric (fraud class) | Value |
|---|---|
| Precision | 0.06 |
| Recall | 0.92 |
| F1 score | 0.10 |
| ROC-AUC | 0.9705 |
| PR-AUC | 0.7236 |

## Confusion matrix

|  | Predicted Legitimate | Predicted Fraud |
|---|---|---|
| **Actual Legitimate** | 55,331 | 1,533 |
| **Actual Fraud** | 8 | 90 |

## Interpretation

The model catches 90 of the 98 fraudulent transactions, giving a high recall of
92 percent. However, it does this at the cost of a very low precision of 6
percent, because it also flags 1,533 legitimate transactions as fraudulent.
These 1,533 cases are false positives.

This result illustrates the central problem of the project. It is relatively
easy to catch fraud if a large number of false positives is acceptable. The
difficult and valuable goal is to catch fraud while keeping false positives low.
Logistic regression sets a clear baseline here, with strong recall but weak
precision. The second model is evaluated against this baseline, with particular
attention to whether it can reduce the 1,533 false positives while keeping recall
high.