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

## Summary

The client, a navigation app, asked to find out more about customer behavior at gas stations, specifically, they wanted to figure out how much time, on average, drivers spend filling up at each gas station chain.

We were provided with data on individual gas station visits. The data covered 317104 data points within the week of April 2, 2018 to April 9, 2018 on 45 gas chains, with 471 stations in total, roughly 100 visits per gas station per day, which we estimated as comprising around 10% of the total refueling traffic (those are customers with a navigation app open as the source of data). The data indicated that two chains grabbed more than 50% of the market, with 5 more chains taking additional 35%.

The data did not contain missing values, only the `date_time` variable was of the string type with dates in ISO UTC format, so we took care of the date and time relevant issues straight away:
* converted this column into datetime type;
* added three columns - weekday, local_time, date_hour, to find out how gas station visiting times depend on the days of the week and on the time of the day.

When studying the `time_spent` variable, we came up with the following facts:
* the average visit time drops gradually (which looked fine), with the last chain having average visit time slightly above 50 seconds (which looked suspicious);
* when medians were checked, ~40% of all visits were shorter than 60 seconds, appr. 25% shorter than 20 seconds (there were also stations in our dataset where 60% and more of visits were shorter than 60 seconds);
* there were visits in our dataset that are abnormally long (these extreme durations probably reflect drivers who stop for additional arrangements, other then refueling);

To clean the data (until we have additional inputs from the client), we applied three rules:
* More than 15 min long visits are excluded from the analysis.
* Gas stations where 60% or more of visits are shorter than 60 seconds are excluded from the analysis.

Before moving to the estimates, we checked whether they could be still impacted by proportions of abnormal visits. We found that we significantly reduced the correlation between median `time_spent` by gas station with the visits shorter than 60 seconds in the filtered dataset compared to the raw dataset. A meaningful difference in median times appeared due to clean up of some gas stations and of the `too_fast` visits in general; most gas stations had median `time_spent` between 120 and 300 seconds. i.e. between 2 and 5 minutes.

The main finding was pointing at specific chains which seemed to be the first ones to partner with, with median refueling time above 200 seconds and decent amount of stations. We illustrated this conclusion with a graph showing the median refueling time and the number of gas stations by chain.

## Main challenges

### Time spent

While taking a closer look at `time_spent` variable we found massive concentration of near-zero visiting times (appr. 25% of all visits had time_spent value below 20 seconds; more than 42% lasted less than 60 seconds. In addition, appr. 2.4% of the visits had duration above 15 minutes with some visits ending up being 8 hour long. Both too short and too long visit times took place at various stations.

Speculating on the reasons of the latter fact, we suggested that hour long visits can be attributed to gas station workers on the shift with navigation app open all the time and drivers doing something else, e.g., buying drinks, food or other supplies, eating, washing the car, sleeping and more. The mean was almost 90% higher than the median, and those extreme outliers are at least one reason for that. With regard to the short visits, those which were 19 seconds and less long can be cars passing by. In genreal, visits below 60 seconds, look suspicious and after some short analysis we built a case to communicate the issue to the client to get more details on possible reasons.

Next, we took a closer look at the gas station with most of the ultra-long visits and did not find any differences in the shape of the distribution of the `time_spent` variable relative to the rest of the sample. We assumed, that a visit aiming refueling should last less than 15 minutes; after checking the share of visits longer than 15 minutes, we found that for the majority of gas stations the overall share of long visits did not exceed 5%, with 50% of them having less than 1.4% of long visits. After checking that visits longer than 15 minutes can be safely dropped, we proceeded to investigating the too short and ultra-short visits, i.e. those shorter than 60 and 20 seconds, correspondingly.

The amount of short visits did not depend on the day time (the overall number of visits diminished at night and during the afternoon as expected). We found no particular correlation between visiting time and the time of the day, which makes sense actually. By calculating the percentage of fast visits per gas station, we found that there were stations with more than 60% of visits shorter than 60 seconds and some of them had 100% of short visits. Different gas stations had various shares of visits less than 20 seconds long with appr. 20% on average, while 50% had up to 16% of them and 75% of the stations had up to 23.5% of such visits. Specifically, at 31 gas stations (out of 471) 50% and more of all visits were shorter than 20 seconds.

Based on the scatter matrix, we found a nearly direct correlation between mean visit time and the share of visits shorter than 60 seconds when this share is above 60%. Thus, the conclusion was to drop those visits all together. We estimated the "losses" and found that the share of visit below 60 seconds at stations where 60% and more of the visits are this long is appr. 25% of the original dataset.

### Calculating median in a robust way

To have a more robust estimate (which would be less sensitive to problems with individual gas stations, whether physical or on the data extraction level) of how much time drivers typically spend at gas stations of a particular chain, we applied **the aggregate median function to the medians for individual gas stations** to calculate the median for each chain. We checked and found that with less visits the dispersion of median visit times grows, i.e. the estimate of the median visit time is less robust for stations with small number of visits; therefore, to account for high variability of individual estimates when based on small samples, we dropped gas stations with less than 30 visits for the 7-day period under investigation. We also treated chains with 10 and less gas stations as one group (we called it "Others"), assuming that the client will be less interested in investing their effort into those chains at this stage of the deployment, and got final estimates for 10 chains including "Others". The distributions of visit times were roughly similar, so the estimates are comparable across the chains.
