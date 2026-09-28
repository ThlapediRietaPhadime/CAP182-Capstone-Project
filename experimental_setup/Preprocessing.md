# Preprocessing

**Notebook:** scripts/preprocessing.ipynb

**How to use:** put bank-additional-full.csv in the same folder as the notebook, then run all cells from top to bottom. The notebook writes data/processed/train_clean.csv and data/processed/test_clean.csv. In this repository the raw file and the cleaned files are stored in datasets/.

## What this notebook does

The raw UCI Bank Marketing file (41,188 rows, 21 columns) uses semicolons instead of commas, so it is loaded with `sep=";"`. Column names are lowercased and dots are replaced with underscores (for example emp.var.rate becomes emp_var_rate).

The target column `y` (yes/no) is renamed deposit and converted to 1/0. In the raw data 4,640 clients said yes and 36,548 said no, so about 11.3% are positives.

I dropped the duration column. It records how long the phone call lasted, so it is only known after the call has happened, and a duration of 0 always means the client said no. Keeping it would let the model use information that does not exist at prediction time. STADIOEquities should apply the same rule to its own data: anything recorded because a deposit already happened cannot be used to predict one.

I removed 1,784 exact duplicate rows, which leaves 39,404 rows and 20 columns. Values of "unknown" in categorical columns are kept as their own category rather than filled in, because a missing answer can itself carry information.

## Train/test split

The data is split chronologically, not randomly. The file is sorted by contact date, so the first 80% of rows are the training set and the last 20% are the test set. This copies real use, where the model scores clients who registered after the training period.

| Set | Rows | Share of deposits |
|---|---|---|
| Train | 31,524 | 6.6% |
| Test | 7,880 | 31.9% |

The big gap in deposit rates is real drift in this dataset (the later months of the campaign had much higher success rates), not an error in the split. It is the main reason absolute model performance is modest, and it is discussed in Model1Performance.MD.

## Why these choices

| Decision | Reason |
|---|---|
| Drop duration | It leaks the outcome |
| Chronological split | It mimics real deployment and avoids over-optimistic scores |
| Keep "unknown" as a category | Missing answers can be informative |
| Remove duplicates | Repeated records would be counted twice |

Next step: FeatureEngineering.MD.
