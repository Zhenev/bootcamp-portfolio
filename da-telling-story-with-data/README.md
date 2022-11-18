# What Makes a Robot-Run Cafe Chain Successful

## Data:

We will use open-source data aggregated in the `rest_data` table:

- `object_name` — establishment name
- `chain` — chain establishment (TRUE/FALSE)
- `object_type` — establishment type
- `address` — address
- `number` — number of seats
 
## Goal:

The ultimate goal stated for the project is, based on the open-source data on restaurants in LA, to hypothesize on a potentially successful scaling strategy for a robot-run cafe chain.

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats |
seaborn |
math |
re |
datetime |
cufflinks |
plotly |

NOTE: make sure the Jupyter Notebook is trusted to see the interactive plots.

## Contents

* Introduction
* Dataset
* Data preprocessing
* Data Analysis
* Conclusions

## Summary

Me and my partners decided to open a small robot-run cafe in Los Angeles. The project was promising but expensive, so we wanted attract investors. They were interested in the current market conditions and whether we would be able to maintain your success after the novelty of robot waiters wears off. So the task was to prepare a market research. To this end, we employed open-source data on restaurants in LA.

While preprocessing the initial data:

* A quick check of missing values was performed, 3 data points fixed.
* The chain variable was casted into boolean.
* We investigated implicit duplicates using stemming and dropped 57 objects at 47 addresses to fix the issue (still there can be some cases of similar titles with variations at the beginning of the title).
* The resulting datasets contained data on 9594 eating places.
* We took a look at the distribution of the seat number and learned that it is non-normal with long tail above 50 seats.
* Additionally, while performing the data analysis, we extracted the street names into a separate columns to study street-wise trends.

The analysis included the following aspects:

* Proportions of the various types of establishments.
* Proportions of chain and nonchain establishments. Learn which type of establishment is typically a chain.
* Characteristics of chains: many establishments with a small number of seats or a few establishments with a lot of seats?
* Number of seats for each type of establishment.
* TOP10 streets by number of restaurants.
* Share of streets that only have one restaurant.
* Trends for streets with a lot of restaurants.


The main conclusions included:

1. Appr. 75% of all the establishments are restaurants;
2. For some reason all the backeries work in chains. Than the majority of about 60% of cafes and fast food establishments worked in chains. Pretty reasonably, bars are the most independent establishments with less than 30% of them working in chains, while appr. 30% of the restaurants were part of chains;
3. Chains tended to have many more objects with smaller number of seats, 50% of the objects had 25 seats and less, while restaurants had more seats in general;
4. Independent bars and restaurants had the greatest number of seats, "on average", around 30;
5. The top10 streets with the largest amount of establishments in general were the same top10 streets with the largest amount of restaurants;
6. Appr. 86% of the streets had only one restaurant on them;
7. On the popular streets (with 10 and more restaurants) the distribution of the restaurants resembeled that of the general dataset; nevertheless, the restaurants on the popular streets tended to be smaller (50% of them possessed 24 and less seats vs. 29-30 for the general dataset), though large restaurants could have even more seats than those on the streets contained less restaurants.
8. The most striking difference detected was in the proportion of chain cafes, general fast food objects, and even pizza establishments; those chain objects, especially chain pizzas, had higher number of seats than the general dataset; unfortunatelly, data on only small number of such objects was available for popular streets; however, it gave a good value proposition idea on possible format: to focus on the niche of fast-food like objects.

The overall recommendation on the restaurant type and number of seats would be to open a medium-size independent fast-food like object of 30 to 40 seats in one of the popular (to draw upon the traffic and raise interest at as many people as possible) streets.

A PDF file was included to the project folder presenting this research to investors.

## Challenges

The main challenge of this project seems to be beyond the data analysis per se and it resides in the amount of assumtions to make in view of the novelty and the natural lack of data in what supposedly should be a promising niche. Thus, the real estate and the traffic issues, as well as the general readiness to accept the idea of robo-service in food, should be investigated additionally. The team has to proving the concept, and after gathering some statistics and testing the hypotheses with regard to the target audience, menu and favorable conditions for a robo-restaurant to thrive, further developing of a chain can be assessed. Ultimately, the idea can become a new standard of high quality and time-efficient eating.
