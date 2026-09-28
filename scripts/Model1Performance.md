# Model 1 Performance: Logistic Regression

**Notebook:** scripts/model1_logistic_regression.ipynb 

**How to use:** run preprocessing.ipynb and feature_engineering.ipynb first, then run this notebook from top to bottom from the same folder. Its evaluation cells produce the results below and save model1_predictions.csv and model1_metrics.json to data/processed/ (in this repository: experimental_results/).

## Results on the held-out test set (7,880 clients)

| Metric | Value |
|---|---|
| Accuracy | 0.532 |
| ROC-AUC | 0.527 |
| Precision (deposit) | 0.335 |
| Recall (deposit) | 0.473 |
| F1 (deposit) | 0.392 |

Classification report:

```
              precision    recall  f1-score   support

  no_deposit       0.69      0.56      0.62      5366
     deposit       0.34      0.47      0.39      2514

    accuracy                           0.53      7880
   macro avg       0.51      0.52      0.51      7880
weighted avg       0.58      0.53      0.55      7880
```

## What the numbers mean

- **Accuracy** is the share of all clients classified correctly.
- **ROC-AUC** measures how well the model ranks depositors above non-depositors (0.5 is chance, 1.0 is perfect). It is the most reliable single number here because the classes are imbalanced.
- **Precision** is the share of clients flagged as depositors who really deposit.
- **Recall** is the share of real depositors the model finds.
- **F1** balances precision and recall.

## Interpretation

An ROC-AUC of 0.527 is only just above chance. This is expected for three reasons:

1. I removed duration, the strongest predictor in the raw data, because it is only known after the call and leaks the outcome (Moro, Cortez and Rita, 2014).
2. The test set is a later slice of the campaign, and this dataset drifts over time: the test set has 31.9% deposits against 6.6% in training.
3. Logistic Regression is a linear model, and clients who deposit and clients who do not overlap heavily and depend on feature interactions.

A recall of 0.473 means the model finds fewer than half of the clients who go on to deposit. On its own it is a weak tool, but it is an honest benchmark for Model 2.

## Confusion matrix and confidence intervals

Confusion matrix on the test set (rows are the true class, columns the predicted class):

| | Predicted no deposit | Predicted deposit |
|---|---|---|
| **Actual no deposit** | 3,006 | 2,360 |
| **Actual deposit** | 1,325 | 1,189 |

The model finds 1,189 of the 2,514 real depositors and wrongly flags 2,360 clients who do not deposit.

95% confidence intervals show how much each score could move with a different sample of clients. Accuracy, precision and recall use Wilson intervals for a proportion; ROC-AUC uses the Hanley-McNeil approximation.

| Metric | Estimate | 95% CI |
|---|---|---|
| Accuracy | 0.532 | 0.521 to 0.543 |
| ROC-AUC | 0.527 | 0.513 to 0.542 |
| Precision (deposit) | 0.335 | 0.320 to 0.351 |
| Recall (deposit) | 0.473 | 0.453 to 0.494 |

The ROC-AUC interval (0.513 to 0.542) sits just above 0.5, so the model is better than chance but only slightly.
