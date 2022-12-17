# Making Churn Prediction Non-Trivial

## Data:

The data consists of files obtained from different sources:

- `contract.csv` — contract information
- `personal.csv` — the client's personal data
- `internet.csv` — information about Internet services
- `phone.csv` — information about telephone services

In each file, the column `customerID` contains a unique code assigned to each client. The contract information is valid as of February 1, 2020.

## Goal:

In this project, we dig deeper into refining the feature set and move from a two- to a three-class model based on the results of running UMAP to better grasp the dataset.

## Libraries used:

numpy |
pandas | 
matplotlib | 
seaborn |
plotly | 
re |
Datetime |
sklearn |
scipy |
keras |
umap

## Contents

Intro

Decomposition

Data Upload, Pre-processing and Analysis

EDA summary, open questions and further steps

Intermediate discussion results

Final data preprocessing

Modeling - two-class classification
- Dummy Classifier
- Modeling functions
- Logistic Regression
- Random Forest
- Keras implementation of Logistic Regression
- Fine-tuning the feature set
- HistGradientBoosting Classifier
- Re-running the models
- ML intermediate summary

UMAP

Constructing a better neural network for the two-class case

Simple three-class neural network

Best model discussion

Conclusions


## Summary

### Dataset

As part of EDA, we demonstrated that the most churn happened during the first three months of the contract and with the customers with higher monthly charges, while the customers can be devided not only into those who churned and those who not, but into three classes, when churned customers are grouped as those who churned in the first 6 months of the contract and those who churned after a longer period.

In addition, we discovered the following issues with the source data:
- partial data on internet and phone services usage as compared to the number of customer IDs covered by the contract and personal information; no indication on the months customers had singed up for additional services;
- the data cover the churn period of four months, from Oct 2019 to Jan 2020;
- there were more new customers and more churn in the last five months of the period.

### ML

Together with the team leader, we ended up with the following plan:
- try modeling with two datasets, a "basic" one, which includes data from `contract.csv` and from `perdonal.csv`, and an "augmented" one, which uses the data of all the source files;
- consider the task as being a two-class classification problem and a three-class classification problem;
- apply Logistic Regression as a base model, then try Random Forest, and some variation of a neural network.

The need to prevent data leakage (any `EndDate` based variables are target) and avoid multicollinearity while encoding was stressed.

The modeling itself included:
- finalization of the dataset preprocessing (correct data types assignment, datasets join, categorical variables encoding (see the note on encoding);
- dataset split into training and testing subsets (see the note on splitting); validation subset was extracted for weighted class NN models;
- scaling;
- training a number of models with cross-validation;
- the need of class balancing was studied separately;
- comparing the models by validation scores, choice of the best model and checking its performance on the test subset.

Based on the intermediate results of two-class classification, we concluded that there was a need in a deeper analysis of the non-linear relationships between the variables. To this end, we employed the UMAP approach, and demonstrated that the feature set can be reduced to two-dimensional embeddings, which revealed that the observations split into three classes; thus, developing a three-claa classification model was suggested. With a simple three-class NN, we demonstrated the potential to harvest a good result in term of `roc_auc` score. Based on the learning curves, we chose to run this model with 200 epoch and ended up with a score of 0.86.

![image](https://user-images.githubusercontent.com/78222587/208249020-34dc2438-86b6-47af-94ff-538a34674ceb.png)

## Challenges

### Two-class classification

During the two-class modeling:
1. We established a baseline by running a Dummy Classifier model; trained a basic Logistic Regression, checked confusion tables and established the balancing procedure for the classes.
2. We trained 10+ models with class weightening and cross-validation: Logistic Regression (without and with hyperparameter tuning), Random Forest (without and with hyperparameter tuning) and the `keras` implementation of the Logistic Regression for two combinations of the preprocessed data: "basic" and "augmented".
3. We build additional `keras` implementation of Logistic Regression for the basic feature set, using `validation_data` option, instead of the `validation_split` parameter, and showed that it results in much higher scores. 
4. Then we tuned the feature dataset, by omitting less important features, as revealed during the `EDA` (multiple phone line service, gender, and streaming services); the initial version of this feature set contained missing values for the internet services usage variables, since only part of the customer ID's are mentioned in the source file.
5. With the "tuned" feature set we trained `HistGradientBoosting`, without dropping the missing values (`HistGradientBoosting` Classifier handles them), which showed better result than the balanced Random Forest model on the "basic" feature set after hyperparameter tuning.
6. At the next step, we performed several checks and assumed that the missing values in the "tuned" feature set can be filled with zeros, thus, preserving observations, as oposed to the "augmented" feature set.
7. We re-run the Logistic Regression and the Random Forest models without and with hyperparameter tuning, as well as the `keras` implementation of the Logistic Regression and Histogram-based Gradient Boosting Classification Tree.

### Class-balancing

As mentioned above, the need of class balancing was studied separately; the models were modified to include class weights (for weighted class NN models, it was shown that a better result can be achieved when a validation subset is split from the trainign set and explicitely passed to the `.fit()` method of the model).

### Non-linear dependency between variables

The poor results of the `keras` version of the Logistic Regression suggested that we might have been actually dealing with a complex non-linear dependency between the variables. We used [`Uniform Manifold Approximation (UMAP)` approach for non-linear dimension reduction](https://umap-learn.readthedocs.io/en/latest/index.html), mainly as additional exploration tool.

The UMAP exploration resulted in two conclusions:
- the "tuned" feature dataset (see paragraph 4 above) with filled missing values comprises a more prominent option.
- a three-class model will possibly bring good results.

In what followed, based on the "tuned" feature set, we constructed:
- 2 additional neural networks for the two-class classification case, which resulted in lower validation `roc_auc` score than the `keras` implementation of logistic regression.
- a simple three-class neural network; the initial idea was to demonstrate the potential, while actually, even this simple model, harvested a good result in term of `roc_auc` score, and we decided to pic it as the final model.
