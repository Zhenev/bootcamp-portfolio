# Looking for  Grains of Gold

## Data:

We were provided with three files:

- `gold_recovery_train.csv`,
- `gold_recovery_test.csv`,
- `gold_recovery_full.csv`.

The `full` file supposedly contained the data both from the `train` and the `test` files. The  documentation to the data mentioned the naming convention for the variables.

## Goal:

In this project, we build a model to predict amount of gold recovered from gold ore. The model is optimized by a custom, business need driven, model quality metric, a weightes sMAPE.

## Libraries used:

pandas | 
numpy |
re |
matplotlib.pyplot |
plotly.express |
seaborn |
sklearn |
scipy

Note: the notebook should be trusted to allow correct display of plots.

## Contents

* Introduction
* Technological Process Overview
* Dataset
* Data overview and pre-processing
* ML
* Conclusions

## Summary

### Dataset

1. We uploaded the three datasets, `train`, `test`, and `full`; there are 22715 data points in the raw full dataset, all columns and data types were correct, except the `date` variable; the full dataset did not have duplicated data points, though there were missing values for some variables, which, based on the time series nature of the data again, we filled with lineary interpolated data.
2. We checked the time dependent nature of the data through applying time series visualization. The test period consists of two chunks of time: September to December, 2016 and September to December, 2018; the training set consist of the rest of the data points between January, 2016 and August, 2018; i.e. it can be the case that the model should evolve with the time; thus, in case a static model, we found it being worth comparing two training and testing subsets - for 2016 and 2017, correspondingly.

### EDA and pre-processing
1. Correlation analysis did not result in any quick conclusions, the dependencies in the ore processing seem to be pretty complex with some non-linear dependency on particle sizes of the rogher input. We employed the Wilcoxon-Mann-Whitney Nonparametric Test and showed that the hypothesis of equal distributions of the particle sizes for the train and the test datasets should be rejected; moreover, the distributions of the particle sizes turned to be different for each period of time within both the training and the testing datasets;
2. We showed, both on the Au case and for the total values, that the concentration values in the train dataset could inherit some measurement errors; thus, to avoid any impact on the model we suggested to keep only observations which exhibited the expected growth in the concentration values along the purification process.
3. We showed that the `rougher.output.recovery` in the train dataset is correctly calculated; additionaly, we noticed that there are ~320 observation points with zero or near-zero Au concentrations in the flotation output.

**Two main conclusions were made:**

1. To avoid ambiguous results while evaluating the models, we decided to employ cross-validation to help us make the distribution of each validation subset as close to the original data as possible, and hopefully build a more robustic model, which will score decently on the test dataset as well.
2. Additionally, we saw that the test set contains a limited set of the features, `input` and `state` variables only and no calculated data, which is reasonable since we assume that no a posteriory data can be available at the start of the process and we want to make the predictions at the start. That means than we could use only this set of features when we train the model.

We prepared the data to train models (`X_train`, `Y_train`, `X_test`, and `Y_test`) based on these two concludions.


### Modeling

Before proceeding with the modeling, we applied `.make_scorer()` function to create a scorer, based on the weighted sMAPE formula, as discussed above; we took into account the fact that this scorer works as a loss function, the less is better.

The modelling part included the following steps:

1. Sanity check - the dummy regression, which uses `mean` values for making predictions, resulted in 9.8% of weighted sMAPE score in case of the clean dataset and 10.3% in case of the raw dataset (the one with seemingly inconsistent values of the total concentrations).
2. We compared three linear regression models, including basic versions of Lasso and Ridge, and a random forest model; we applied cross-validation to score the models; in all models we used `k=10` cross-validation subsets. We also applied RandomizedSearchCV to finetune the hyperparameters of the Random Forest model, although the result was still unacceptable.
3. We compared the results received on the clean train subset and on the raw train dataset, when the observations which exhibit inconsistent dynamics of the total concentrations along the purification process are kept; the final model was just a Linear Regression on the raw dataset, although the test weighted sMAPE score of that model was 0.3% lower than that of a dummy model.

We can made the overall conclusion that building a more complex model, which will also be constantly updated with new data, could be a better option, since the production conditions seem to be constantly changing. Additionaly, the weights in the scoring function can be adjusted.

## Challenges

### Technological Process and choosing a Metric fitting the business need

This projects necessitates some domain knowledge on gold ore purification. Mined ore undergoes primary processing to get the ore mixture or rougher feed, which is the raw material for flotation (also known as the rougher process). After flotation, the material is sent to two-stage purification. Thus, the whole process includes the following steps:

1. Ore pre-processing into rougher feed (ore mixture);
2. Processing:

        2.1. Flotation;
        2.2. First purification;
        2.3. Second purification;
5. Post-processing of the final AU concentrate.

At each of the processing stages residues with a lower concentration of valuable metals are created (rougher tails). The `Flotation` is highly volatile: its stability is affected by the high variability of the physicochemical state of the flotation pulp (a mixture of solid particles and liquid).

As part of the gold ore process modeling, the following naming convention was adopted for the variables: `[stage].[parameter_type].[parameter_name]`, i.g. `rougher.input.feed_ag`.

- Possible values for `[stage]`:
  - `rougher` — flotation
  - `primary_cleaner` — primary purification
  - `secondary_cleaner` — secondary purification
  - `final` — final characteristics

- Possible values for `[parameter_type]`:
  - `input` — raw material parameters
  - `output` — product parameters
  - `state`  — parameters characterizing the current state of the stage

- Possible values for `[parameter_name]` can be either input parameters, process state parameters, physicochemical state characteristics, or characteristics which are calculated later on during the production process.

### Customized model quality metric

Following the business logic of the gold ore purification process, specific model quality assessment metric was proposed, which we called weighted sMAPE (symmetric Mean Absolute Percentage Error). The reason for that was the presence of two target variables:

- rougher concentrate recovery - `rougher.output.recovery`;
- final concentrate recovery - `final.output.recovery`.

sMAPE itself is similar to MAE, but is expressed in relative values instead of absolute ones, and it equally takes into account the scale of both the target and the prediction. The weighted metric included 25% of rougher sMAPE and 75% of final sMAPE.



