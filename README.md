# Loan Credit Scoring: Probabilistic Classification of Loan Defaults

Do take note of the license for this repo before you copy any work!  Please let me know if you have any suggestions or ideas to further improve the project!

## Introduction

Traditionally, information from external credit bureau and extensive past payment behaviour is used heavily to develop credit scoring models which support the credit decisioning process of the lending function in banks. Alongside busines rules developed within the banks, the credit scores are then used to approve or deny credit applications.

The heavy reliance of traditional credit scoring models on credit bureau information creates challenges for a huge proportion of consumers, who have little to no relevant credit file such as recent graduates, pensioners, retirees etc., to gain access to loan and credit facilities. This leaves large segments of the population who are underbanked or unbanked, some of which are potential revenue sources for the bank.

The final report on the project can be found [here](Docs/Report/Report.pdf).

## Objective

This Proof-of-Concept (POC) project hopes to focus on the underbanked segments, consumers who are “thin-file” or “no-files”, with very limited to no credit history who may be better served by such analytical models. The project hopes to develop a credible credit risk scoring model for unsecured retail loan applicants who have limited credit history with the bank. The credit risk score developed would be based on the probability of a default on the loan by the consumer, the higher the probability of loan default, the higher the credit risk.

## Data

Raw data used is from Bondora, an online marketplace for peer-to-peer consumer lending that allows users to invest in loans granted through the Bondora Group, retrieved on 13/02/2021. More details and data can be obtained from Bondora's [website](https://www.bondora.com/en/public-reports).

Bondora's data was used as it provides a substaintial amount of loan data, containing data on the loan applicants' personal attributes such as age, education level etc. which are typically difficult to find from other financial institutions such as banks. See [EDA for raw loan data](Code/EDA/rawLoanDataEDA.ipynb) and [EDA for raw repayments data](Code/EDA/rawRepaymentsData.ipynb) for more details on the datasets.


## Process

In order to fit the narrative of the POC, Bondora's data is first processed into a suitable dataset (see [DataConstruction](Code/DataConstruction/DataConstructionFinal.ipynb)). Further [EDA](Code/EDA/POALoanDataEDA.ipynb) was carried out on the [processed dataset](Data/Processed/) and a SciKit-Learn preprocessing pipeline was [developed](Code/DataPrep/POAPreprocessing.ipynb) to consolidate all preprocessing steps to be carried out on the dataset into a pipeline to avoid data leakage.

With the preprocessing pipeline built, binary probabilistic classification modelling pipelines were built using SciKit-Learn and several techniques as:
* Cross-validation
* Hyperparameter tuning - Grid Search & Random Search
* Probability Calibration
* Under and Oversampling
* Threshold Optimisation
were further used to develop and improve the models as seen [here](Code/Model/Modelling/Modelling.ipynb). Models built as SciKit-learn's pipeline allows for future easy deployment into production.

## Conclusion

Train dataset cross-validation result:

|Model|ROC AUC|Log Loss|Brier Score|
|-----|-------|--------|-----------|
|Naïve – Stratified|10.157193|0.294081|0.498761|
|Naïve – Most frequent|0.500000|4.309549|0.124774|
|Naïve – Prior|0.500000|0.410715|0.119779|
|LR – Tuned| 0.724631| 0.371227| 0.110702|
|DT – Tuned| 0.709649| 0.366089| 0.107761|
|RF – Tuned| 0.721236| 0.353999| 0.104156|
|__XGB – Tuned Isotonic__| __0.734359__| __0.346553__|__0.102394__|

Test dataset evaluation:

|Model|MCC|F1|Precision|Recall|
|-----|-------|--------|-----------|----|
|Naïve – Stratified|-0.001961|0.158951|0.123571|0.222717|
|LR – Tuned| 0.234272| 0.335621| 0.221295| 0.694321|
|DT – Tuned| 0.190831| 0.311643| 0.238546| 0.449332|
|RF – Tuned| 0.235409| 0.345528| 0.278554| 0.454900|
|XGB – Tuned Isotonic| 0.241395| 0.351035| 0.270295| 0.500557|

The final model built upon XGBoost’s implementation of a gradient boosted tree was evaluated to perform 46.9%, 15.6% and 14.5% better in the ROC AUC, log loss and Brier score metrics than naïve classifiers. In terms of classification performance, it performed 120% better than the naïve classifier based on F1 scores when the model was G-Means (sensitivity/specificity) optimised.

## Challenges and Future Work

While the classification and probabilistic estimation performances of the developed models are optimistic, there are significant challenges in interpreting the feature importance. Tree-based models such as the DT, RF and XGB all indicated that country is of top importance. It is not clear why and if there are confounding factors such as different economic situation, access to alternative credit sources and cultural views on credit in the different countries, which could lead to a potentially spurious association between country and default rates. Further investigation should be carried out.

An area to work on in the future if I could redo the project is to carry out more feature engineering prior to modelling. For example, carry out unsupervised clustering of loan applications based on their profile features or factor analysis and use this as another feature input to the model.

Another area to consider would be to perform multivariate analysis during the EDA process as this may highlight certain trends that could help improve the modelling process. Lastly, it would be to have a deeper engagement with the relevant stakeholders to better understand the business context to facilitate feature engineering and interpretation of the feature importance.
