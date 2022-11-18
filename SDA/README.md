# What Sells a Car

## Data:

- `price`
- `model_year`
- `model`
- `condition`
- `cylinders`
- `fuel` — gas, diesel, etc.
- `odometer` — the vehicle's mileage when the ad was published
- `transmission`
- `paint_color`
- `is_4wd` — whether the vehicle has 4-wheel drive (Boolean type)
- `date_posted` — the date the ad was published
- `days_listed` — from publication to removal

## Goal:

The ultimate goal stated for the project is to determine which factors influence the price of a vehicle; thus, we are going to reveal sound assumptions with regard to cars' characteristics which have the most impact the price of a particular vehicle.

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats
seaborn

## Contents

* Introduction
* Dataset
* Data overview
* Data preprocessing
* EDA
* Testing hypotheses
* Summary and conclusions

## Summary

This project's focused on revealing which factors influence the price of a vehicle. We were not been provided with any preliminary hypothesis or information regarding the data quality; therefore, we had to carefully study the data, evaluate its quality, and see how significant its issues were and what are appropriate ways to fix them.

Initial dataset included 51525 vehicle sale postings over 354 days starting from May 1, 2018 to April 19, 2019, which makes approximately 145 postings a day on 100 vehicle models when the five most popular vehicle types are SUV, truck, sedan, pickup, and coupe. A regular issue of wrong types (of the `model_year`, `cylinders`, `is_4wd`, and `date_posted` variables) was treated, then a more challenging issue of missing values (in the `model_year`, `cylinders`, `odometer`, `paint_color`, and `is_4wd` variables) had to be taken care of to make the dataset more suitable for the required analysis:

The following steps were performed at the Data Preprocessing step to fix the data types:

* the type of `date_posted` was fixed to datetime;
* the data types of `model_year` and `cylinders` were fixed to integer;
* the data type of 'is_4wd' was fixed to boolean.

After treating the missing variables, we dropped duplicates based on the vehicle parameters + `date_posted` variable. Finally, we enriched the data by adding `model_age`, `yearly_mileage`, and numerical form of condition variable.

In the EDA part we:

* studied of outliers by `price`, `odometer`, and `yearly_mileage`;
* fixed the outliers by limiting the variables above: `price` less than USD100K, `mileage` less than 0.5 mln, `yearly_mileage` less than 140K;
* fixed the near-zero price values by modelling the lower bound price levels depending on the `model_age`;
* dropped or merged categories that did not have enough data points (50);
* employed correlation matrix to study the correlations between `price` and other numerical variables;
* employed box plotting to study the correlations between price and categorical variables.

The last stage included several hypothesis testing excercises to support the finding from the EDA stage.

The main findings include:

* vehicle manufacturing year (`model_year`) or vehicle age at the date of ad publication (`model_age`) and having 4wd (`is_4wd`) are among numerical factors having the most correlation with the price variable;
* the total mileage (`odometer`), `condition_num` (numerical expression of condition) and average yearly mileage (`yearly_mileage`) are more correlated with the manufacturing year, while `cylinders` has higher correlation with vehicle having 4wd or not (PCA should be applied to explore whether any of these four variables is a necessary dimension in the presence of `model_year`);
* vehicle type and color (`paint_color`) are among categorical factors having the most correlation with the price variable.

## Challenges

Filling in the missing variables, was the most time-consuming part of the data preprocessing. To accomplish the task, we employed several techniques:
* we concluded that missing values in `is_4wd` could be filled in with nulls;
* missing values in `paint_color` were checked for randomness and filled in with 'unknown' value;
* observations with missing values in `model_year` were dropped due to the lack of any obvious way to restore them;
* missing values in `odometer` were restored by sub-grouping by `model_age` and `is_4wd` variables and conducting median estimates;
* missing values in `cylinders` were restored by random sampling from the existing distibution.
