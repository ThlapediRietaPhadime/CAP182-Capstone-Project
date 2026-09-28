# Comparing Model 1 and Model 2

**Notebook:** scripts/compare_models.ipynb

**How to use:** run both model notebooks first, then run this notebook from top to bottom from the same folder. It reads the predictions and metrics from data/processed/, confirms both models were scored on the same test set, runs McNemar's test and saves comparison_report.json to data/processed/. In this repository these files are in experimental_results/.

## Results side by side

Both models were scored on the same 7,880-client test set.

| Metric | Logistic Regression | Random Forest | Difference |
|---|---|---|---|
| Accuracy | 0.532 | 0.574 | +0.042 |
| ROC-AUC | 0.527 | 0.713 | +0.186 |
| Precision | 0.335 | 0.414 | +0.079 |
| Recall | 0.473 | 0.805 | +0.332 |
| F1 | 0.392 | 0.547 | +0.154 |

Random Forest is better on every measure, and most of all on recall.

## Is the difference real? McNemar's test

To check the gap is not down to chance, I used McNemar's test. It looks only at the clients where the two models disagreed, so it is suited to two models scored on the same test set.

| | Count |
|---|---|
| Logistic Regression wrong, Random Forest right | 1,575 |
| Logistic Regression right, Random Forest wrong | 1,246 |

There were 2,821 disagreements. The test statistic is 38.14 and the p-value is about 6.6 x 10^-10, so the difference is statistically significant.

## Trade-offs

- Logistic Regression is easy to explain through its coefficients. Random Forest is closer to a black box, although its feature importances give a rough view of what drives it.
- Random Forest finds far more of the clients who will deposit, which is what STADIOEquities needs for targeting. It also flags more non-depositors (recall for "no deposit" is 0.47), so contacting every flagged client has a cost.

## Recommendation

I recommend Random Forest as the main model, with Logistic Regression kept as a simple reference where explainability matters. The reasons and next steps are in the Part D report.
