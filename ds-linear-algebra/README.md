# The Pillar of Machine Learning

## Data:

We got one file, `insurance_us.csv`, with the following data:

- Features: insured person's gender, age, salary, and number of family members.
- Target: number of insurance benefits received by an insured person over the last five years.

## Goal:

In this project, we show case a masking technique that makes it hard to recover personal information, though does not impact the ML results.

## Libraries used:

pandas | 
numpy |
seaborn |
typing |
sklearn 

## Contents

* Introduction
* Data overview and pre-processing
* ML
* Conclusions

## Summary

### Dataset

1. The data types were correct, the dataset did not look like the most memory-intensive for that number of variables.
2. The sample included almost equal number of men and women; only customers aged 40+ seemed to receive insurance benefits.
3. Among numerical variables, only the salary variable seemed to be normally distributed. No outliers.

### ML

In this part we performed three basic tasks and established:
1. A procedure to find customers who are similar to a given customer:
    - We applied two distance metrics, Euclidian and Manhatten;
    - We showed that scaling significantly affects the neighbor search. 
2. A classification model to predict whether a new customer is likely to receive an insurance benefit:
    - A baseline dummy model scored maximum 0.2 for the F1-score when assigning success in 100% of cases.
    - As expected, the KNN model performed much better for the scaled data.
    - When running for the n_neighbors equal 1 to 10, the best score (0.96) was achieved with k=1.
3. A linear model to predict the number of insurance benefits a new customer is likely to receive using a linear regression model.
    - We used a scoring function, which uses two metrics, RMSE and R2.
    - We run the model for two cases, raw and scaled data.
    - As expected, scaling of the features results in different magnitude of the linear regression coefficients.

**The last model was also checked on transfromed, i.e. masked, data; the same quality of the model had been received as expected.**

## Challenges

Analysis of customer data must follow strict requirements of preserving personal data. To this end, a masking technique should be applied. In this project:
  - We showed how the raw customer data can be masked by multiplying on an invertible matrix.
  - We established why it is possible to mask the data by multiplying the numerical features by an invertible matrix.
  - We showed why the invertability property is important.
