# Time-for-Training

## Data:

We got one file, `car_data.csv`, with the following data.

Features:

- `DateCrawled` — date profile was downloaded from the database
- `VehicleType` — vehicle body type
- `RegistrationYear` — vehicle registration year
- `Gearbox` — gearbox type
- `Power` — power (hp)
- `Model` — vehicle model
- `Mileage` — mileage (measured in km due to dataset's regional specifics)
- `RegistrationMonth` — vehicle registration month
- `FuelType` — fuel type
- `Brand` — vehicle brand
- `NotRepaired` — vehicle repaired or not
- `DateCreated` — date of profile creation
- `NumberOfPictures` — number of vehicle pictures
- `PostalCode` — postal code of profile owner (user)
- `LastSeen` — date of the last activity of the user

Target:

- `Price` — price (Euro)

## Goal:

In this project, we study the balance between quality, speed and time required to train a model of predicting the car price.

## Libraries used:

pandas | 
numpy |
seaborn |
plotly |
time |
sklearn |
scipy |
lightgbm |
catboost |
xgboost

## Contents

* Introduction
* Data overview and pre-processing
* ML
* Conclusions

## Summary

### Dataset

1. As part of the uploading process we checked memory usage and date types for a small subsample, which allowed us to avoid significant excessive use of the memory.
2. The raw dataset included ~K355 observations.
3. We found ~260 fully duplicated entries and K21+ implicite duplicates, which essentially looked like the same cars (including miliage, the postal code of the car and the price) crawled and published twice or more, so the listing would have different last time seen. To avoid any biases, we dropped those entries.
4. We performed extensive work on filling missing values (see the Challenges part):
- some variables were lacking between 5% to 20% of values;
- one variable, `power` had significant amount of unexpectedly low values, including zeros and values below 30 HP; in both cases we treated those values as missing.
- we preserved ~14% of the dataset as it appeared after dropping duplicates.
5. Finally, we dropped irrelevant variables, and ended up with a clean dataset of 10 variables with ~K295 observations.

### ML

In the modeling part, we:

1. Encoded the categorical variables with `LabelEncoder()`;
2. Split the data;
3. Defined the score function, based on RMSE (the score was evaluated by cross-validation, for which `cross_val_score()` was employed);
4. Made a sanity check with a pipeline based on `DummyRegressor()`;
5. Built a RandomForest model with hyperparameter tuning (with Random Serach);
6. Applied XGBoost with hyperparameter tuning (with Grid Search);
7. Applied LightGBM with hyperparameter tuning (with Grid Search).

LightGBM model with tuned hyperparameters turned out to be the fastest and resulted in the best score (Linear Regression was the only model faster than LightGBM, but it scored the worst among the basic models), while we failed in finding better hyperparameter combination for Random Forest and for XGBoost, while the search took significant amount of time. The summary of the scores and the run time is presented in the table below:

![image](https://user-images.githubusercontent.com/78222587/207647960-46b00e18-1bc8-4ee7-9a12-c940845260b1.png)

`LightGBM_basic*` is a LightGBM model without explicit categorical features definition (as we remember we used Label encoding).

## Challenges

### Filling in missing values

During the missing variables pre-processing, we explored part of the variables and suggested using `RandomForestClassifier` to predict the missing values for categorical variables, like `fuel_type`, `gearbox`, and `vehicle_type`, and `RandomForestRegressor` to predict reasonable values instead zeros and near-zero values for the `power` variable.

Thus, we restored missing variables in `fuel_type`, `model`, `gearbox`, `vehicle_type`, and `power` variables (the model considered only car manufacturing characteristics and included the `brand` variable as well), and then in `not_repaired` variable (we modeled it through `car_age` and `mileage`).

To accomplish the task of filling in missing values, we developed the following procedure:
- extract the dataset (variables) for modeling (from the initial dataset `data_raw`);
- extract a clean subset, without missing values, to use for modeling;
- choose a variable to fill in missing values and apply modeling:
    - extract the subset with missing values for the variable of interest;
    - create a model with that variable as a target to fill in the missing values;
    - use the model to predict missing values;
    - concatinate the observations with the predicted missing values with the initial version of the clean subset and, thus, update it;
- choose the next variable to predict and repeat the modeling for it.

While implementing the procedre above, we automated most of its parts and ended up with one line function, `iterative_missing_values_modeling` applicable both for categorical and for numerical variables, for different subsets of related variables.
