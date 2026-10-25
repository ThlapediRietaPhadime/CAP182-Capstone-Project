# STADIOEquities — First-Deposit Activation: Data Extract

Five tables joined on `Client ID`, covering clients registered between September 2024 and August 2026. **Data extraction date: 2026-09-07 10:00:00.** Datetimes are `yyyy-mm-dd hh:mm:ss`. `Acquisition Cost` is ZAR without a currency symbol. `Session Duration` is in **minutes**; `Time to Engagement` is in **hours**; `Time to First Deposit` is in **days** — one consistent unit per field, as requested. No names, identity numbers or telephone numbers are included.

## Files

### `table1_client_registration.csv`
One row per client — registration, demographics and acquisition.  
Rows: 2,110

| Column | Description |
|---|---|
| `Client ID` | String |
| `Registration Date` | Datetime |
| `Age` | Integer |
| `Region` | Categorical |
| `Stated Goal` | Categorical |
| `Risk Appetite` | Categorical |
| `Acquisition Channel` | Categorical |
| `Campaign` | Categorical |
| `Acquisition Cost` | Numeric |
| `Registration Platform` | Categorical |
| `Registration Device` | Categorical |

### `table2_app_web_onboarding.csv`
Session-level app and web behaviour with onboarding progress.  
Rows: 7,730

| Column | Description |
|---|---|
| `Client ID` | String |
| `Session Date` | Datetime |
| `Session Duration` | Numeric |
| `Screens Viewed` | Integer |
| `Feature Used` | Categorical |
| `Onboarding Step` | Categorical |
| `Onboarding Progress` | Numeric |
| `Onboarding Steps Completed` | Integer |
| `Step Completion Status` | Categorical |
| `Abandoned Step` | Categorical |
| `Abandonment Status` | Boolean |
| `First Session Flag` | Boolean |
| `Platform` | Categorical |
| `Event Type` | Categorical |
| `Event Date` | Datetime |
| `Last Session Date` | Datetime |

### `table3_email_nudge_engagement.csv`
One row per marketing message sent.  
Rows: 11,584

| Column | Description |
|---|---|
| `Client ID` | String |
| `Message Date` | Datetime |
| `Message Type` | Categorical |
| `Campaign` | Categorical |
| `Message Delivered` | Boolean |
| `Message Opened` | Boolean |
| `Message Clicked` | Boolean |
| `Nudge Engaged` | Boolean |
| `Time to Engagement` | Numeric |

### `table4_account_kyc_funding.csv`
TARGET table — KYC status and first-deposit outcome.  
Rows: 2,104

| Column | Description |
|---|---|
| `Client ID` | String |
| `KYC Status` | Categorical |
| `KYC Progress` | Numeric |
| `First Deposit Date` | Datetime |
| `First Deposit Status` | Boolean |
| `Time to First Deposit` | Integer |

### `table5_timing_quality.csv`
Record-level timing, source and missingness metadata.  
Rows: 21,418

| Column | Description |
|---|---|
| `Client ID` | String |
| `Observation Start` | Datetime |
| `Observation End` | Datetime |
| `Activation Window` | Integer |
| `Record Timestamp` | Datetime |
| `Missing Value Indicator` | Boolean |
| `Data Source` | Categorical |
| `Data Extraction Date` | Datetime |

## Data quality notes

This is an extract from live operational systems and has **not** been cleaned. Expect:

- Missing values in optional / conditionally-populated fields
- Inconsistent casing and stray whitespace in some free-text and categorical fields
- A small number of duplicate records
- Occasional out-of-range or implausible values from data-capture errors
- Some date fields captured in inconsistent formats

**Agreed timing parameters** (these were left open in the request and are now fixed):
- **Activation window: 30 days** from registration. `Activation Window` in table 5 states this on every record.
- **Predictor observation period: the first 7 days** after registration. `Observation Start` and `Observation End` in table 5 give the exact window per client. Every behavioural and marketing record in tables 2 and 3 falls inside that window, so those two tables are safe to use in full — no filtering needed.

**Target:** `First Deposit Status` in `table4_account_kyc_funding.csv` — True where a first deposit was made within 30 days of registration.

**Leakage warning:** `First Deposit Date` and `Time to First Deposit` in table 4 are the label. `KYC Status` and `KYC Progress` are the client's **final** values at extract time, which for some clients is after the 7-day observation window closed. Use `Onboarding Progress`, `Onboarding Steps Completed` and `Step Completion Status` from table 2 instead if you want a strictly observation-window view of KYC.

**Table 5 is record-level metadata**, not client-level — it has one row per record in tables 2, 3 and 4, so it does not join one-to-one with the client. `Missing Value Indicator` is True where a required field is genuinely absent: SMS messages have no open/click tracking, and non-activated clients have no deposit date. Those blanks are documented rather than filled with arbitrary values.

**Field notes**
- `Abandoned Step` is populated only on rows where `Step Completion Status` is Abandoned.
- `Last Session Date` is the client's most recent session inside the observation period and repeats on every one of their session rows.
- `Nudge Engaged` is populated only for in-app nudges; `Message Opened` / `Message Clicked` are blank for SMS.
- `Campaign` in table 1 is blank for organically acquired and referred clients.

_Synthetic data supplied for the CAP182 capstone project. Not real client data._