# Where to Sell a Fueling App

## Data:

- `name` — the encrypted name of each gas station chain; each name has been replaced with that of a flower
- `id` — a unique code for each particular gas station
- `date_time` — the time the driver arrived at the gas station, in ISO format;
- `time_spent` — the amount of time spent at the gas station in seconds.

## Goal:

The working hypothesis is that gas station chains with longest refueling times are appropriate to market a new fueling app; thus, the overall goal is to find chains with longest refueling visit times. Having said this, we should be careful and watch for anomalies in the visit time variable values and, to better estimate average visit time per chain, we will have to find those anomalies and address them appropriately.

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats
seaborn

## Contents

* Project stages
* Data overview
* Data preprocessing
* EDA
* Estimating the median visit time per chain
* Summary and conclusions
