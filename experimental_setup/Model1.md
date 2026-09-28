# Model 1: Logistic Regression

**Notebook:** scripts/model1_logistic_regression.ipynb

**How to use:** run preprocessing.ipynb and feature_engineering.ipynb first, then run this notebook from top to bottom from the same folder. It needs the data/processed folder next to it. It trains the model, evaluates it, and saves models/model1_logreg.joblib, data/processed/model1_predictions.csv and data/processed/model1_metrics.json. In this repository the model is stored in models/ and the predictions and metrics in experimental_results/.

## Why Logistic Regression

I chose it because it can be explained: its coefficients show why a client is flagged, which matters to STADIOEquities' teams. It is also the baseline model in all three papers in my literature review (Moro, Cortez and Rita, 2014; Setiyani et al., 2022; Abidin et al., 2025), so it gives a fair comparison.

## Pipeline

The features are scaled with StandardScaler, then a LogisticRegression is fitted. Scaling matters because a column such as nr_employed (in the thousands) would otherwise dominate the 0/1 columns.

## Hyperparameters

| Parameter | Value | Why |
|---|---|---|
| C | 1.0 | Moderate regularisation |
| solver | lbfgs | Standard solver for this size of data |
| max_iter | 1000 | The default of 100 is not always enough to converge with 63 columns |
| class_weight | balanced | Only about 6.6% of training rows are deposits, so this stops the model from just predicting "no" |
| random_state | 42 | Makes results repeatable |

Results are in Model1Performance.MD.
