# Feature Engineering

**Notebook:** scripts/feature_engineering.ipynb

**How to use:** run preprocessing.ipynb first, then run this notebook from top to bottom from the same folder. It reads data/processed/train_clean.csv and test_clean.csv, and writes X_train.csv, X_test.csv , y_train.csv and y_test.csv to data/processed/. In this repository these files are stored in `datasets/`.

## What this notebook does

In the pdays column (days since the client was last contacted in an earlier campaign), the value 999 is a placeholder meaning the client was never contacted before. It is not a real number of days, and a model would treat it as a huge gap. I added two flags:

- previously_contacted: 1 if pdays is not 999 (176 training rows are 1).
- contacted_before: 1 if previous is greater than 0 (2,292 training rows are 1).

The categorical columns are one-hot encoded (each category becomes its own 0/1 column): job, marital, education, default, housing, loan, contact, month, day_of_week and poutcome. "unknown" is treated as its own category, in line with the preprocessing step.

The test set's columns are then aligned to the training set's columns, so nothing in the test set can change how the features are built. Finally each set is split into `X` (features) and `y` (the deposit target). Both sets end up with 63 feature columns.

No scaling is done here. Logistic Regression scales the features inside its own pipeline, and Random Forest does not need scaling.

## Links to STADIOEquities' own data

The same decisions will apply to STADIOEquities' data:

- Placeholder values (for example "no earlier app session") need an explicit flag, just as pdays = 999 does here.
- Categorical fields such as acquisition channel need the same encode-on-train, align-on-test approach.
- Any field that is only filled in after a deposit is the equivalent of duration and must be dropped.

Next step: Model1.MD and Model2.MD.
