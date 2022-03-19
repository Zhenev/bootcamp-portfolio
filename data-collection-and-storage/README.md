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

The ultimate goal stated for the project is to determine how weather conditions impact the raxi ride length.

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

