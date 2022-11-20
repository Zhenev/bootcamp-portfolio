# Best Place for a New Well

## Data:

We were provided with geological exploration data for three regions stored in separate files:

- `geo_data_0.csv.`
- `geo_data_1.csv.`
- `geo_data_2.csv.`
- `id` — unique oil well identifier
- `f0`, `f1`, `f2` — three features of points
- `product` — volume of reserves in the oil well (thousand barrels).

In addition, the documentation to the data mentioned the following information:

1. Only linear regression is suitable for model training (other mmodels were not able to demonstrate sufficient level of prediction quality).
2. When exploring the region, a study of 500 points is carried with picking the best 200 points for the profit calculation.
3. The budget for development of 200 oil wells is 100 USD million.
4. One barrel of raw materials brings 4.5 USD of revenue The revenue from one unit of product is 4,500 dollars (as mention above, the volume of reserves is measured in thousand barrels).
5. After the risk evaluation, only the regions with the risk of losses lower than 2.5% should be kept. From the ones that fit the criteria, the region with the highest average profit should be selected.
6. The data is synthetic: contract details and well characteristics are not disclosed.
7. The specific meaning of the features is unimportant, but the features themselves are significant.

## Goal:

In this project, we build a model to predict the best place for a new well for a mining company and analyse potential profit distribution using the bootstraping technique.

## Libraries used:

pandas | 
numpy |
matplotlib.pyplot |
plotly.express |
seaborn |
sklearn |
scipy

Note: the notebook should be trusted to allow correct display of plots.

## Contents

* Introduction
* Dataset
* Data overview and pre-processing
* Well profitability analysis
* Conclusions

## Summary

### Dataset
1. We uploaded the three datasets, of 100000 data points each, all columns and data types were correct, no missing values, no duplicates.
2. The regions differed in some meaningful way by the features and volumes of reserves in the wells with the first and the third region demonstrating some minor similarity, on the `f2` feature and `product` only; all three regions have similar standart deviation of the `product` values, though the second region has 50 lower average volume and is more skewed due to outliers.
3. In addition, the `product` variable, the target, seemed to be of a different scale, as compared to the three features of the well points.

No data pre-processing needed for the given dataset and no need in feature scaling procedure as a prerequisite for the model training (features already scaled).

### Modeling

The modelling part included the following steps:

1. Split the data into a training and validation subsets (we have a basic linear regression here, no regularization and other optimizations; hence, no need to split into training, validation and testing).
2. Train the model and make predictions for the test set.
3. Save the predictions and correct answers for the test set.
4. Find the average volume of predicted reserves and model RMSE.
5. Analyze the results.

### Well profitability analysis

Backed by the distribution of the actual profit of 200 best wells, we concluded that the second region would have the average profit between USD5.0 mln and USD5.3 mln with 95% probability and it also imposed the lowest risk of the losses, 1% only compared to 6%-6.4% in the first and the third regions.

## Challenges

### Modeling

The results of the first run of a linear regression looked strange in terms of the model quality with the second region demonstrated perfect predictive capability of the linear regression, while the first and the third regions demonstarted similarity, again, in the poor model quality. One of the explanations could be the synthetic nature of the data, but as a check up, tohelp in understanding the reasons for that difference, we visualized the data points as a flow of feature values ([inspired by this article](https://towardsdatascience.com/histogram-on-function-space-4a710241f026)).
In this way, we demonstrated that the three regions had different characteristics, while the `f2` feature for the second region even had discrete values.

### Potential profit calculations

Profit calculation itself requers understanding of the well economy; it includes a pre-defined formula for the minimum volume of reserves sufficient for developing a new well without losses.

The calculations involved a pretty sophisticated procedure (see Appendix 1).

To better understand the quality of the models, we performed calculations for three cases:
1. When 200 best predictions are chosen out of 500 randomly sampled data points.
2. The actual total of those wells chosen in case 1.
3. When 200 largest wells are chosen by the actual value of the `product` variable out of 500 randomly sampled data points

The sampling was performed from the same validation subset for the target variable.

By performing this comparison, we learned that the linear regression model for the third region resulted in overestimated values and the one for the second region resulted in underestimated values; the ranking of the regions, if the actual reserve volumes are considered for the wells with highest predictions, changes, leaving us with the second region being the best candidate.

### Potential profit distribution analysis

To calculate the confidence level and estimate the risk of losses, we applied the bootstrapping technique for assessing confidence intervals of ML models.

In this case, bootstrapping refers to resampling data and instead of fitting the model to the original test features and target variable, fitting the model to resampled versions of them multiple times, 1000 in our case. Since we already computed the predictions, instead fitting the model again, we were able to sample the original target test points and take corresponding predictions instead. For every run, after extracting the 200 top model predictions, we used the actual `product` values of those data points to calculate the actual profit. Then we used the 1000 of such estimates to derive the distribution of actual profits (we want to estimate the distribution of actual profits, not predicted profits).

### Domain knowledge

As it mentioned above, a better grasp of the oil sector is a prerequsite for this kind of projects. We suggested consulting with industry professionals, e.g. to learn whether the risk level of 1% is acceptable: given the high capital-intensity of well development and relatively low ROI, why to drill and incure all the costs and environmental demage if there is any chance of losses. If oil prices go up, then the second region will definitely become the most attractive.

## Appendix 1. Profit calculation procedure

1. Prepare for profit calculation:

- Calculate the volume of reserves sufficient for developing a new well without losses.
- Compare the obtained value with the average volume of reserves in each region.
- Derive preliminary observations.

2. Calculate profit from a set of selected oil wells and model predictions:
- Pick the wells with the highest values of predictions.
- Summarize the target volume of reserves in accordance with these predictions.
- Calculate the profit for the obtained volume of reserves.

3. Calculate risks and profit for each region:
- Use the bootstrapping technique with 1000 samples to find the distribution of profit.
- Find average profit, 95% confidence interval.
- Find risk of losses (loss is negative profit, we calculated it as a probability and then expressed as a percentage).

