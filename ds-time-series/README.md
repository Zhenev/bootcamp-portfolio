# Behind the Time Series

## Data:

We got one file, `taxi.csv`, with the following time series data:

- `datetime` — order timestamps in 10-minute slots
- `num_orders` — number of orders in a given time slot
- 
## Goal:

In this project, we build a predictive model for a time series, while studying and tackling the issues of heteroscedasticity and autocorrelation.

## Libraries used:

pandas | 
numpy |
seaborn |
plotly |
time |
sklearn |
scipy |
statsmodels |
arch

## Contents

* Introduction
* Data overview and pre-processing
* ML
* Conclusions

## Summary

### Dataset

![image](https://user-images.githubusercontent.com/78222587/207796695-d7409621-d4da-4010-9d04-5f039119b0ec.png)


1. As part of the uploading process we checked memory usage and date types for a small subsample, which allowed us to properly upload the data.
2. The raw dataset had ~26500 10-min observation points, no missing values, no duplicates.
3. The data exhibited a trend: the overall number of rides per day grew from 1300 in the beginning of March, 2018, up to 3700 rides per day by the end of August, 2018.
4. The seasonality component demonstrated some interesting pattern:
- The average number of rides diminished on weekends and Tuesdays;
- There were peaks on Mondays;
- The number of orders grew from Tuesday to Thursday.
5. The standard deviation grows with the growing number of rides; it exhibits weekly spikes as well - they start to be more expressed by the end of the period under investigation.

From these results, the need appeared to conduct heteroscedasticity and autocorrelation tests to check our assumtions, before we dive into modeling.

### ML

In the modeling part, we:

1. Engineered the feature set:
  - resampled the source data,
  - added 24 hourly lags,
  - day of week, and hour,
  - 4-hour slot rolling median (to make the model more robust).
2. Split data into training/validation and test subsets (`shuffle=False`)
3. Introduced a block splitter (a custom class) for blocked cross-validation.
4. Run baseline linear regression model.
5. Run Random Forest model with hyperparameter tuning (via Randomized Search).
6. Run ARIMA-GARCH modeling.
7. Re-worked the feature set and the Random Forest model based on the ARIMA-GARCH results:
  - added a rolling standard deviation and several lags of it,
  - re-run hyperparameter search.
  
The cross-validated value of RMSE was 22; on the test set, the model, as expected, performed poorly, with an RMSE value of 44. The following steps were suggested to improve the modeling:
- further investigation of additioanl features,
- trying other models, besides Random Forest,
- further investigation of ARIMA-GARCH variations potential,
- developing a dynamic model.

## Challenges

As it is mentioned above, the EDA results showed that heteroscedasticity and autocorrelation tests should be conducted before the modeling. Heteroscedasticity and autocorrelation makes any model, which assumes that the conditional variance of the independent variable is constant and independent, i.e. the residual error for each prediction is identically (and, in many cases, normally) distributed and, thus, has the same probability distribution, unreliable: standard errors of the model's parameters can become incorrect and misleading when inference should be made with regard to the significance level of the model parameters. One of the intuitive ways to observe this impact is plotting the model residuals versus the target variable values, which should demonstrate some kind of dependency for the former one.

![image](https://user-images.githubusercontent.com/78222587/207846222-e211b6c2-a65c-4882-933d-cf8d3c4737b4.png)

### Heteroscedasticity

There is a family of tests for heteroscedasticity; all of them share the same approach, though employ different functions for modeling the dependence of fluctuations in the data on the target variable (you can find [intro](https://towardsdatascience.com/heteroscedasticity-is-nothing-to-be-afraid-of-730dd3f7ca1f) here). We run the most popular, White's, test on the residuals of the decomposed initial time series.

Note: we used the residuals of the naive decomposition of the daily aggregate data from the previous section, which is sufficient in case the alternative hypothesis (the residuals are not distributed with equal variance) is accepted. We rejected the null-hypothesis (of the residuals distributed with equal variance)

To tackle the problem of heteroscedasticity, the following methods are applicable;
- transforming the target variable (the most common way is taking a log);
- redefine the target variable, e.g. by taking difference as a new dependent variable;
- using regression models which account for heteroscedasticity, like weighted regression (gives smaller weights to data points that have higher variances, which shrinks their squared residuals) or (G)ARCH (Autoregressive Conditional Heteroskedasticity) models.

Taking logs did not help due to infinite values for zero amount of orders for some hours; taking the difference did not work also, as the growing variability kept appearing; thus, we turned to study the possibility of the applying (G)ARCH model.

### Autocorrelation

While studying the autocorrelation of the given dataset, we visualized and analyzed the intraday pattern (based on the 10-min time slots), as well as hourly aggregations. For testing purposes, we used the daily data and employed Ljung-Box test. Ljung-Box test, when applied for 24-hourly lags, showed that all of them are highly correlated. Also, we applied the `plot_acf` function of the `statsmodels` library to visually check the sagnificance of the autocorellation for different lags. This method showed that only part of the lags had statistically significant autocorrelation, namely `t-1`, `t-2`, `t-7`, `t-8`, `t-11`, `t-12`, `t-13`, `t-22`, `t-23`, `t-24`.

Note: we should remember though that ACF only looks at particular lags and tests randomness of each tag, while Ljung-Box test tests whether a group of autocorrelations of a time series is random.

To account for the non-stationary nature of the given time series, we combined the GARCH model (see above) with ARIMA model.

### Feature Engineering

The EDA conclusions should have been taken into account to carefully design the feature set. Thus, we chose the hour and the day of the week feature, the rolling median (should be calculated on shifted data to prevent data-leakage), and lag features.

### Cross-validation

To still take advantage of the cross-validation, we had to introduce blocked cross-validation. Blocked cross-validation works by adding margins between the training and validation folds in order to prevent the model from observing lag values which are used twice once as a regressor and another as a response. We introduced the blocked splitter explicitely.

### ARIMA-GARCH modeling

We run Grid Search for 200+ sets of the ARIMA model and found the best combination, in terms of time consumption and RMSE score. Neither of the ARIMA models showed higher RMSE value than the cross-validated RMSE of the initial Random Forest model, but our goal was to check whether we can improve the situation by applying a GARCH model to ARIMA residuals. The study showed that the lag variance and lag residual error components can be explanatory variables of statistical significance; however, the GARCH model failed to produced normally distributed residuals as well.

Despite being ambiguous and time-consuming, the results of diving into ARIMA-GARCH modeling suggested that re-working the initial feature set by including additional features, namely rolling standard deviation (a proxy to variance) and a number of its lags, can benefit the model.
