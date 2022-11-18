# Best Conditions to Propose a Ride

## Data:

1. neighborhoods table: data on city neighborhoods
- `name`: name of the neighborhood
- `neighborhood_id`: neighborhood code

2. cabs table: data on taxis
- `cab_id`: vehicle code
- `vehicle_id`: the vehicle's technical ID
- `company_name`: the company that owns the vehicle

3. trips table: data on rides
- `trip_id`: ride code
- `cab_id`: code of the vehicle operating the ride
- `start_ts`: date and time of the beginning of the ride (time rounded to the hour)
- `end_ts`: date and time of the end of the ride (time rounded to the hour)
- `duration_seconds`: ride duration in seconds
- `distance_miles`: ride distance in miles
- `pickup_location_id`: pickup neighborhood code
- `dropoff_location_id`: dropoff neighborhood code

4. weather data are retrieved from a web site and stored as additional table in the SQL database at hand:
- `date_and_time`
- `temperature`
- `description`: short discription of weather conditions at that hour


## Goal:

The ultimate goal stated for the project is to determine how weather conditions impact the taxi ride length.

## Libraries used:

requests |
BeatifulSoup|
json |
pandas | 
matplotlib.pyplot |
scipy.stats |
seaborn

## Contents

* Introduction
* Dataset
* Data overview
* Data preprocessing
* EDA
* SDA
* Summary and conclusions

## Summary

We have loaded and investigated 3 datasets: the number of rides for each taxi company on November 15-16, 2017;  the average number of rides by the neighbourhood they ended in, for in November 2017;  data on rides from the Loop to O'Hare International Airport with the weather conditions at the moment of their start. The datasets comprise results of specific queries to a database.

During the check up of the data loaded, it turned out that two issues should be fixed: duplicates in the weather data and the datetime type of the `start_ts` variable.

The exploratory analysis showed that:

* At our disposal is data on appr. 137 thousands rides for the period of November 15-16, 2017 and 56 thosands dropoffs on average in that month.
* Top ten destinations cover appr. 76% of dropoffs; there is a long tail of 84 neighbourhoods making the rest of the dropoffs.
* Top 10 taxi companies serve appr. 72% of the rides, with Flash Cab and Taxi Affiliation Services taking appr. 20% of all rides; there is a long tail of 54 companies making the rest of the rides.
* The Loop neighbourhood is leading in the amount of dropoffs (unfortunately, the csv file does not contain the data on the pick-up locations to compare with), with River North, Streeterville and West Loop making up the absolute leaders with above 5000 dropoffs (the next most popular destination, O'Hare airport makes appr. 2.5 thousands).

Statistical analysis was possible for the third dataset, which contained data on ride duration and weather conditios for the ride. We have performed **Levene test for non-normal distributions** and showed that the sample distributions of the ride durations during bad and during good weather conditions have the same sample variance; then we performed **two-sided and one-sided t-test for equal sample means** and found that during the bad weather conditions the rides tend to take more time.
