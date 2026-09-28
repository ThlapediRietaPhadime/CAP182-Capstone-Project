# Model 2: Random Forest

**Notebook:** scripts/model2_random_forest.ipynb

**How to use:** run preprocessing.ipynb and feature_engineering.ipynb first, then run this notebook from top to bottom from the same folder. It needs the data/processed folder next to it. It trains the model, evaluates it, and saves models/model2_random_forest.joblib, data/processed/model2_predictions.csv and data/processed/model2_metrics.json. In this repository the model is stored in models/ and the predictions and metrics in experimental_results/.

## Why Random Forest

I wanted a model that can pick up non-linear patterns and interactions between features, such as a campaign outcome mattering together with the number of contacts. A linear model cannot do this. Random Forest does, and it is one of the seven models Setiyani et al. (2022) compared on this same dataset.

## Pipeline

A RandomForestClassifier is fitted directly on the encoded feature matrix. There is no scaling step, because tree models only ask yes/no questions at each split and do not care about the numeric range of a feature.

## Hyperparameters

| Parameter | Value | Why |
|---|---|---|
| n_estimators | 400 | Number of trees; enough for stable predictions |
| max_depth | 8 | Stops trees growing deep enough to memorise the training data |
| min_samples_leaf | 5 | Another guard against overfitting to rare patterns |
| class_weight | balanced | Same imbalance handling as Model 1 (about 14 non-deposits for each deposit in training) |
| random_state | 42 | Makes results repeatable |
| n_jobs | -1 | Uses all CPU cores |

Results are in Model2Performance.MD.
