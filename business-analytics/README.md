
# Best Conditions to Propose a Ride

## Data:

The visits table (server logs with data on website visits):

- `Uid` — user's unique identifier
- `Device` — user's device
- `Start Ts` — session start date and time
- `End Ts` — session end date and time
- `Source Id` — identifier of the ad source the user came from

All dates in this table are in YYYY-MM-DD format.

The orders table (data on orders):

- `Uid` — unique identifier of the user making an order
- `Buy Ts` — order date and time
- `Revenue` — app's revenue from the order

The costs table (data on marketing expenses):

- `source_id` — ad source identifier
- `dt` — date
- `costs` — expenses on this ad source on this day

## Goal:

The goal stated for the project is to derive conclusions and advise marketing experts how much money to invest and on what channel and raise ideas on necessary investigations for the product team.

## Libraries used:

pandas | 
numpry |
matplotlib.pyplot |
scipy.stats |
seaborn

## Contents

* Introduction
* Dataset
* Data overview
* Data preprocessing
* Data analysis
* Conclusions

## Key conclusions

Based on three summaries, on products, sales and marketing costs, which resulted from our investigation, we were able to propose the following recommendations to the team:

1. Abandon the marketing source number 3 and refocus the marketing spendings onto source number 1, which costed 6 to 7 times less per new buyer and had much better potential to bring revenues higher than costs.
2. Investigate how the average purchase size could be boosted for buyers who come through the source number 1.
3. Study the reasons for 50% to 80% churn in the first month following the first order to boost the retention rates.
4. Pay special attention to users using touch devices and users using both touch devices and desktops; the latter exhibited much higher retention rates.
5. Finally, additional investigation could be performed to find out why the very first cohort in the dataset remained the most loyal and apply the knowledge to nurture other cohorts.

## Challenges

This project involved a very detailed study of three datasets and a variety of assumptions, which assumes a high degree of involvement and constant check ins from the marketing depatment side.

## Detailed summaries on the datasets

### Data upload

1. A quick check of the necessary data types was performed before the upload, to make the memory usage more effective.
2. The resulting datasets contained data on 359400 visits, 50415 orders and 2542 data points of expenses on different marketing sources.
3. No missing values, some column names required fixing.
4. The first visit in the dataset started at 00 hour 01 min on June 01, 2017; the last visit started 1 min before midnight one year later; the last visit to end ended up at 01 hour 26 min on June 01, 2017.
5. There were some outliers, as well as ultra-short visits, and the distribution is in any case non-normal: the median duration is 5 minutes, while the average is above 10 minutes and the maximum duration is around 23.5 hours. The mode has a value of 1 minute.
6. The orders dataframe did not have duplicates; the first payment in the dataset was proceed at 10 minutes past midnight on June 01, 2017, while the last one at 02 minutes past midnight one year later.
7. We had seven marketing sources to analyze, interestingly for most of them there is data only for 363 days out of 365 for the year under consideration, and 364 for the category number 5.

### Product usage

1. The average number of people using the app every day was 900+; on the weekly basis 5700+ people on average used the app; on the monthly basis the average number of users was K23+.
2. Most users used the product through touch devices: the daily average of peole using the app from a touch device only was 550+ out of 900+ daily active users; there were users using the app both from a desktop and from a touch device, their share veried from ~8% of all daily active users to ~6% of all monthly active users.
3. There were on average 987 sessions on the daily basis, 6781 visit on the weekly basis and 29950 on the monthly basis for the period of time under consideration.
4. The average number of sessions per day was equal 1.09, while users who used only desktop devices made less visits and users who visited the app from either a touch device or from a desktop performed 1.17 sessions per day on average. The same trend could be seen for weekly and monthly visits, while on the monthly basis the difference between the average number of sessions for monthly active users in total and the average number of visits for monthly active users using both types of devices was the most pronounced.
4. The most frequent duration is one min and below for all users, desktop sessions tend to be shorter: 50 % of sessions lasted up to 3 minutes on a desktop and up to 5 min on a touch device, while users who used both type of devices tend to spent additional minute on the app.
5. Less than 8% of the users returned to use the up even the month following the month of the first visit and by the end of the period under consideration the things got even worse: only about 4.8% of the first time users returned the month after. During the autumn and winter months the app attracted more users, which supposedly was the consequency of slightly higher marketing spendings.
6. Things got even worse for those users who use only desktop, compared to those who use only touch devices. The retention of those who use both desktops and touch devices was higher.; however, they churned fast anyway: less in the second month, but in the same manner that all the users together in the next months.
7. It could be the case that some campaign during September - November 2017 resulted in much better retention rates, but all the cohorts experienced a steep decline in December 2017. In April 2018 there was another steep decline in returning users.
8. There was a spike in positive churn in March 2018 for the August 2017 cohort, which would be good to investigate in more details.
9. Users from the June 2017 cohort turned the most loyal though.

### Sales

1. The absolute majority of users who ended up placing an order did this close to the date of the first visit; actually, the majority of first orders are performed within 20 minutes from the first visit. Between 5% and 10% of them returned and placed the first order in the following month. Interestingly, the better retention of the users in general in Autumn 2017 resulted in higher number of the first orders as well. There is also a long tail of users who placed their first order long after they visited the app for the first time (usually around 2% to 3% percent).
2. There was high number of orders for the first cohort, June 2017, during their first 10 months (with the maximum of 4.5 order in December 2017) and a local peak in order numbers for June 2017 and September 2017 cohorts in the month of February 2018; October 2017 to December 2017 cohorts experiences some splash in May 2018.
3. The number of regular buyers was actually very small in each cohort, several dozens vs. 2K to 4K of first time buyers.
4. Within the first month after the first order appr. 10% of the buyers would place additional order, thus resulting in 1.1 order in average for the first order month.
5. In general, the average number of orders per buyer in the month of December 2017 was higher for 5 cohorts out of 6. Some sign of a boost in the number of orders could be noticed for the spring cohorts of 2018, but the data is insufficient and additional investigation is necessary to be able to derive conclusions on possible trends.
6. The overall average purchase size across the whole dataset was equal to 7.2 units and was impacted by some outliers.
7. The average size of a purchase within the first order month varies between 3.7 and 5.3 units and could be a bit higher in the following months.
8. There were two interesting developments in September 2017 and December 2017 cohorts: the average purchase size was much higher than the overall average in December 2017 and February 2018 correspondingly and remained to be relativly high for at least four consecutive months.
9. Since the number of returning buyers was low, the lifetime value of the customers, especially compared with the average purchase size, is very low: a simple linear regression model suggested that one could expect the LTV to be around 10 money units even after 11 months. In accordance to the previous observations, the LTV of June 2017 and September 2017 cohorts were slightly higher.

### Marketing expenses

1. We investigated daily data for seven marketing sources. The overall marketing spending for the given period was 329131.6 units, the median daily spending across sources was 77.3 units, the maximum - 1788.3 units. More than 41% of this sum was spent on source 3. Sources 3, 4, and 5 took almost 75% of the overall marketing spending for the given period.
2. The marketing spending starting from September 2017 to March 2018 were slightly higher than in other months.
3. To analyze marketing costs by sources and months, we connected between the `uid`'s in the orders dataset and the `source_id` in the costs dataset through the `source_id` and `uid` in the visit dataset. **Assuming that the last source before a particular order was the one that brought the buyer**, we calculated the minimal time difference between the end of a session in visits and the order timestamp in orders.
4. CAC for different sources varied from 5.0 units per buyer on average for the source 1 up to 19.2 units per buyer on average for the source 3. Source 10 was the next best after 1 and source 9 took the third place. A simple linear regression model suggested that CAC is had been getting lower during the one-year period under consideration.
5. The overall investments in marketing were not worthwhile: the overall CAC was less than LTV only for the very first cohort, from June 2017, and for the September 2017 cohort. Following our previous observations, the buyers from the June 2017 cohort happened to be relatively loyal, while several buyers from the September 2017 cohort spent unexpectedly high amount of money on purchases in winter (chances are with no relation to the grow in the marketing spendings which took place in the following months).
6. There was not enough data to make conclusions about cohorts of 2018 and given the results with regard to LTV in general, the direct comparison between marketing costs and revenues from the orders in the given month was proposed as a way to estimate the marketing source efficiency.
7. In addition to monthly CAC by sources, we derived monthly order numbers and monthly revenues for different marketing sources and compared them to the monthly spendings on the same marketing sources.
8. Finally, we calculated the ratio between the revenue and marketing costs by source and month.
9. The marketing source number 1 had been systematically the most "efficient" one. The least efficient one was the marketing source number 3, which seemed to be even more disturbing since we remember that this source received more than 41% of the marketing budget and was appr. 7 times more costly than 1; the spendings on the source number 1 got appr. 6% of total costs.
9. Source 10, while being the cheapest in terms of new buyers attraction, seemed to bring buyers whose average purchase size was very small as well.
