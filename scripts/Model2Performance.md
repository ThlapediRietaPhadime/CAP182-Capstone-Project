# Model 2 Performance: Random Forest

**Notebook:** scripts/model2_random_forest.ipynb

**How to use:** run preprocessing.ipynb and feature_engineering.ipynb first, then run this notebook from top to bottom from the same folder. Its evaluation cells produce the results below and save model2_predictions.csv and model2_metrics.json to data/processed/ (in this repository: experimental_results/).

## Results on the held-out test set (7,880 clients)

| Metric | Value |
|---|---|
| Accuracy | 0.574 |
| ROC-AUC | 0.713 |
| Precision (deposit) | 0.414 |
| Recall (deposit) | 0.805 |
| F1 (deposit) | 0.547 |

Classification report:

```
              precision    recall  f1-score   support

  no_deposit       0.84      0.47      0.60      5366
     deposit       0.41      0.80      0.55      2514

    accuracy                           0.57      7880
   macro avg       0.62      0.64      0.57      7880
weighted avg       0.70      0.57      0.58      7880
```

## Interpretation

Every metric is better than Model 1's. The standout is recall, which rose from 0.473 to 0.805: Random Forest finds about 8 in 10 of the clients who go on to deposit. Precision also improved, from 0.335 to 0.414, so the gain is not simply a trade of one measure for the other.

This agrees with Setiyani et al. (2022), who found tree-based ensembles beat logistic regression on this dataset. The likely reason is that the data has non-linear structure a linear model cannot capture.

The cost is that the model flags many non-depositors: recall for the "no deposit" class is only 0.47, which is why overall accuracy is 0.574. The absolute numbers are still modest for the same reasons as Model 1 (no duration field, a later test set, and drift over time).

## Confusion matrix and confidence intervals

Confusion matrix on the test set (rows are the true class, columns the predicted class):

| | Predicted no deposit | Predicted deposit |
|---|---|---|
| **Actual no deposit** | 2,501 | 2,865 |
| **Actual deposit** | 491 | 2,023 |

The model finds 2,023 of the 2,514 real depositors (only 491 are missed), but it wrongly flags 2,865 clients who do not deposit.

95% confidence intervals show how much each score could move with a different sample of clients. Accuracy, precision and recall use Wilson intervals for a proportion; ROC-AUC uses the Hanley-McNeil approximation.

| Metric | Estimate | 95% CI |
|---|---|---|
| Accuracy | 0.574 | 0.563 to 0.585 |
| ROC-AUC | 0.713 | 0.702 to 0.724 |
| Precision (deposit) | 0.414 | 0.400 to 0.428 |
| Recall (deposit) | 0.805 | 0.789 to 0.820 |

The ROC-AUC interval (0.702 to 0.724) is well clear of Model 1's interval (0.514 to 0.541), so the improvement is not a sampling accident. This agrees with McNemar's test in Comparison.MD.
