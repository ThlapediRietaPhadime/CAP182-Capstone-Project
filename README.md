# CAP182-Capstone-Project

## Motivation 
Digital platforms are increasingly being used in the financial services industry to provide clients with accessible investment opportunities. STADIOEquities is a digital investment platform with approximately 2.3 million registered accounts. However, a significant proportion of these accounts have not been funded or converted into active clients. This presents an important business challenge because the value of acquiring an account is largely realised when a client funds the account and continues to use the platform.
The existing challenge is particularly evident in the activation gap. Approximately 41% of registered accounts have never made a deposit, while the sign up to first deposit conversion rate has declined from 64% to 59%. Accounts becoming dormant within six months have also increased from 22% to 31%. Activation is associated with factors such as acquisition channel, onboarding progress, first session behaviour and the time taken to make the first deposit. However, STADIOEquities currently uses onboarding emails and nudges according to a fixed schedule, without sufficiently accounting for differences in individual client behaviour.
Addressing this issue is important because STADIOEquities is not primarily faced with a shortage of registered clients, but with the challenge of converting its existing customer base into funded and active investors. The company incurs a cost to acquire accounts, while an unfunded account generates almost no value. Clients who register but do not fund their accounts also do not progress to using the platform’s investment services. A better understanding of activation behaviour could therefore help STADIOEquities make more informed decisions about how and when to engage clients during the onboarding process.
Data Science methods may assist by identifying patterns in the factors associated with first deposit activation. STADIOEquities has access to app and web behaviour, onboarding and KYC data, account and funding information, demographic information, trading activity, and acquisition and marketing data. These data provide an opportunity to investigate whether early client characteristics and behaviours can be used to distinguish clients who are more likely to activate from those who are likely to remain inactive.
Therefore, this study seeks to generate evidence that may support more targeted onboarding and activation decisions. By improving the understanding of first deposit activation, the findings may assist STADIOEquities in directing activation efforts more effectively and addressing the gap between registered accounts and active clients.

## Problem Statement 
Despite STADIOEquities having approximately 2.3 million registered accounts and extensive client, onboarding and behavioural data, approximately 41% of registered accounts have never made a deposit, and the sign up to first deposit conversion rate has declined from 64% to 59%. It remains unclear whether client characteristics and early behaviours can reliably distinguish newly registered clients who are likely to make a first deposit from those who are likely to remain inactive.
Therefore, this study aims to investigate whether predictive modelling can be used to classify newly registered STADIOEquities clients according to their likelihood of first deposit activation, using available client, onboarding, acquisition and behavioural data, to provide evidence that may support more targeted onboarding and activation decisions.


## Repository structure 
| folder | Description |
|:-------- |:-------------|
|'datasets/'| contains all datasets used in the project|
|'models/' | contains trained machine learning models |
|'experimental setup/' | contains configuration and setup files |
|'experimental results/' | contains results from experiments |
|'scripts/statistical/' | contains statistical helper scripts and scripts used to compare models and results|
|'scripts/visualisation/' | contains scripts used to create visualisation |


## RAAIDD Log

| RAAIDD | Description |
| :--- | :--- |
| **Risks** |1. Target data may be incomplete or incorrectly recorded, particularly for clients who have registered but never made a deposit, which could affect the accuracy of the activation model. 2. Data leakage may occur if information recorded after the first deposit outcome is included as a predictor. 3. Class imbalance may occur because a substantial proportion of registered clients are never funded, which may cause the model to favour the majority class.|
| **Actions** |1. Obtain and integrate the required demographic, registration, app/web behaviour, marketing, account and funding data. 2. Clean and validate the datasets, handle missing values and check that Client IDs correctly link records. 3. Define the First Deposit Status as the target variable and establish the appropriate observation period with stakeholders. 4. Perform exploratory data analysis to identify behavioural patterns associated with activation. 5. Train, evaluate and compare suitable classification models using appropriate performance measures.|
| **Assumptions** |1. Client records can be reliably linked across the demographic, marketing, behavioural and account/funding datasets using a common Client ID. 2. The available historical data contains sufficient information about client behaviour before the first deposit outcome to train a predictive model. 3. The definition of account activation and the target observation period can be agreed with STADIOEquities stakeholders.|
| **Issues** |1. The current sign up to first deposit conversion has decreased from 64% to 59%, indicating that the existing activation process is not converting all registered clients effectively. 2. KYC abandonment has increased from 13% to 18%, which may create incomplete records and affect prediction of activation.|
| **Decisions** |1. Use First Deposit Status as the target variable for the predictive modelling project. 2. Select the final predictive model based on agreed evaluation metrics and its ability to identify clients likely to activate, rather than relying only on overall accuracy. 3. Exclude variables that contain information only available after the prediction point to reduce data leakage.|
| **Dependencies** |1. Data acquisition and validation must be completed before reliable exploratory analysis can begin. 2. The target definition and observation period must be agreed before the modelling dataset can be finalised. 3. Data cleaning and feature preparation must be completed before model training. 4. Model evaluation must be completed before selecting the final model and making recommendations for targeted onboarding interventions.|
