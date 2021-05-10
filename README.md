# Loan-Credit-Scoring-Probabilistic-Classification-of-Loan-Defaults-

In this project, proof-of-concept predictive loan default probability-based scoring models for no/limited credit history applicantions were developed. Through the models, the project also hopes to identify key/important consumer features that contribute to the loan risk scores. Binary probabilistic classification models were built and several techniques such as cross-validation, hyperparameter tuning, probability calibration and under/oversampling were carried out to develop and improve the models. 

Overall, the final model built upon XGBoost’s implementation of a gradient boosted tree was evaluated for its probability prediction performance on the testing dataset where it performed 46.9%, 15.6% and 14.5% better in the ROC AUC, log loss and Brier score metrics than naïve classifiers. In terms of classification performance, it performed 120% better than the naïve classifier based on F1 scores when the model was G-Means (sensitivity/specificity) optimised. Finally, feature importances were explored, highlighting country and language as key features.