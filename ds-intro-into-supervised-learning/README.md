# Predicting Customer Churn Based on Their Behaviour and Contract Termination

## Data:

Features

- `RowNumber` — data string index
- `CustomerId` — unique customer identifier
- `Surname` — surname
- `CreditScore` — credit score
- `Geography` — country of residence
- `Gender` — gender
- `Age` — age
- `Tenure` — period of maturation for a customer’s fixed deposit (years)
- `Balance` — account balance
- `NumOfProducts` — number of banking products used by the customer
- `HasCrCard` — customer has a credit card
- `IsActiveMember` — customer’s activeness
- `EstimatedSalary` — estimated salary

Target

- `Exited` — сustomer has left

## Goal:

In technical terms, we should build a model for churn prediction with the maximum possible F1 score. Additionally, measure the AUC-ROC metric and compare it with the F1. 

## Libraries used:

pandas | 
numpy |
matplotlib.pyplot |
plotly.express |
seaborn |
sklearn |
scipy |
optuna

Note: the notebook should be trusted to allow correct display of plots.

## Contents

* Introduction
* Dataset
* Data overview and pre-processing
* ML
  * Examine the balance of classes. Train the model without taking into account the imbalance.
  * Fix class imbalance, choose a more promissing model.
  * Employ Random Search and Bayesian optimization for hyperparameter search.
  * Perform the final testing.
* Conclusions

## Summary

### Dataset
1. We had a dataset of 10000 observations with mostly relevant data types and 10% missing values in one variable; the target variable had 20% of positive outcomes (churned customers).
2. The data looked clean; 36% of observations had zero balance; until we could get inputs from our colleagues on this, we assumed that those were customers of credit products who are not necessary supposed to have a balance on their account.
3. Data for customers with zero and non-zero balance looked slightly different:
    - For customers with a non-zero balance, those who left were older on average; we also had an indication that the proportion of those who left was higher among customers with only one product.
    - Among those with a zero-balance, the age difference between those who left and those who stayed was even more pronounced; the proportion of those who stayed was much higher for those having two products and zero-balance.
    - Finally, among customers with the highest credit score, mostly those left who had positive balance on their account; one can hypothesize that among those who stayed with the bank were younger people who had been building their credit history, while more established customers, especially those who had already build the highest credit rating and hold non-zero balance, had more flexibility in choosing their bank.
    
### Data pre-processing

To prepare the data, we:

1. Dropped non-meaningful columns - `RowNumber`, `Surname`, `CustomerId`.
2. Scaled the numerical features.
3. Applied OHE to prepare the data for building a Logistic Regression model.
4. Applied Label Encoding to prepare the data for building a Random Forest model.
5. We will split the two resulting datasets separately.

### Modeling

1. Initially, we run both a `RandomForestClassifier()` and a `LogisticRegression()` models for two versions of features, `OHE` based and `Labeled-encoding` based.
2. We  split and scaled the dataframes separately for the two cases, and run both estimators for three cases:
    - unbalanced;
    - class weight balanced;
    - upsampled.
3. The random forest model performed better better than the logistic regression; interestingly, it ended up in slightly better results on the `OHE` dataframe - `F1` score of 0.60 and `roc_auc` score of 0.85.
4. We applied the RandomizedSearchCV method to optimize hyperparameters. It is cost-effective (computationally less intensive) and time-effective (faster – less computational time) [link](https://jmlr.csail.mit.edu/papers/volume13/bergstra12a/bergstra12a.pdf). We used `f1` as the value of the `refit` parameter, and managed to only slightly improve the result - got `F1` score of 0.61 and `roc_auc` score of 0.87.
5. We also applied [bayesian optimization](https://distill.pub/2020/bayesian-optimization/) and implemented it using the [optuna library](https://optuna.org), another open source hyperparameter optimization framework which automates hyperparameter search.
6. With the bayesian optimization we were able to slightly improve both scores, while reaching got `F1` score of 0.62 and `roc_auc` score of 0.87.
7. In the final test, the value of 0.62 was achieved for the `F1` score, and 0.86 for the `roc_auc`.

## Challenges

### Encoding category variables

OHE method to encode categorical data is inappropriate for a tree-based model. Unlike regression model, the tree-based model do not have access to the whole spectrum of features at once and process only one feature at a time. That means  that such a model does not have complete information about the original categorical variable. Splitting a categorical variables into a set of dummies, results in sparse variables, when the importance of the initial variable is dispersed across many dummy variables. The decision tree algorithms, in turn almost never choose sparse variables as splitting near the root of the tree, which means that, even if they were a good predictor, those categorical variables are not considered by the tree in the first place.

Thus, for tree-based models we apply Label encoding, i.e. we encode a categorical variable with numbers in a single column, so the algorithm is able to treat the variable as continuous. We should remember, that Label Encoding helps to transform categories into numbers; however the new numerical variables contain information that does not reflect real facts and sometimes can result in splits that do not make much sense; nevertheless, practical applications show that it still often results in useful splits.

### Hyperparameter tuning

We applied two methods, Random Search and Bayesian optimization, though did not achieve much of improvement in the scores.


