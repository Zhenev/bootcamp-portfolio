# What Makes a Fitness Chain Successful

## Data:

The dataset includes the following fields:

- `Churn` — the fact of churn for the month in question
- User data for the preceding month:
    - `gender`
    - `Near_Location` — whether the user lives or works in the neighborhood where the gym is located
    - `Partner` — whether the user is an employee of a partner company (the gym has partner companies whose employees get discounts; in those cases the gym stores information on customers' employers)
    - `Promo_friends` — whether the user originally signed up through a "bring a friend" offer (they used a friend's promo code when paying for their first membership)
    - `Phone` — whether the user provided their phone number
    - `Age`
    - `Lifetime` — the time (in months) since the customer first came to the gym

- Data from the log of visits and purchases and data on current membership status:
    - `Contract_period` — 1 month, 3 months, 6 months, or 1 year
    - `Month_to_end_contract` — the months remaining until the contract expires
    - `Group_visits` — whether the user takes part in group sessions
    - `Avg_class_frequency_total` — average frequency of visits per week over the customer's lifetime
    - `Avg_class_frequency_current_month` — average frequency of visits per week over the preceding month
    - `Avg_additional_charges_total` — the total amount of money spent on other gym services: cafe, athletic goods, cosmetics, massages, etc.

## Goal:

The goal stated for the project was, based on the churn data, to identify target groups, suggest measures to cut churn, and describe additional patterns with respect to interaction with customers.

## Libraries used:

pandas | 
numpy |
matplotlib.pyplot |
seaborn |
plotly |
sklearn |
scipy |

NOTE: make sure the Jupyter Notebook is trusted to see the interactive plots.

## Contents

* Introduction
* Dataset
* Data preprocessing and exploration
* ML
* Conclusions

## Summary

### General observations

1. The exploratory data analysis part showed that among the variables with higher (negative) correlation with churn are class frequency for the current month, lifetime, the number of months to the contract end, age and contract type by its lenght.
2. We trained several models of binary classification and the best result achieved was with a Logistic Regression, with `recall` of 0.86.
3. We run cluster analysis on fo 5 clusters. They are relatively equally populated, besides one, which share is only about 11% of the sample. Out of 5, four clusters had pretty distinguishable characteristics.

### Cluster analysis
|Cluster |  Possible interaction and retention strategy |
|---|---|
|A cluster with 70% churn rate where the customers who had 1 month to the end of the contract, worked out only once per week and were the youngest group of the customers; they had good proximity on average to the fitness studio (maybe that was what brought them to Model Fitness); they seemed to lack friends to work out together, thus they have less insentives to keep up. | This cluster was suggested as to be targeted in the first place. "Get full involvement", to encourage them to continue to work out, socialize with peers, to get them involved 5 months+ when they can start being used to go twice a week for a work out and hang out with friends.|
|The next cluster was the smallest one, but with 50% of churn rate; this was the only group of customers living far from the fitness studio. | It looks like the partner program should be refreshed/redesigned to attract more employers around the studio by the possibility to grant their far living employees opportunity to work out closer to the office, encouraging them to work out and to socialize more.|
|Another cluster, with the least churn rate of about 2%, of customers with long contracts, who worked out twice per week, were reffered by friends and came through partners; they also were intensive users of group lessons. |This group was recommended to be seen as ideal customers. It can be a good idea to learn the "success factors" of this cluster and to try to get the customers from other clusters get used to similar behavioral patterns.|
|A cluster with the least obvious characteristics, with about 12% churn rate, seemed to have customers with 6 to 12-month contracts who had pretty much time till the end of the contract and worked out almost 2 times on average per week; they had high level of referrals and participance in partner program and group lessons; about 15% of them lived far from the studio. | Given the relatively low churn rate, similar to the share of customers leaving far from the studio, this cluster could considered as having an objective reason for the churn. This assumption could be investigated further in case of a meaningful rise in the churn rate. |
|The last cluster, of 8% churn rate, had customers who worked out more than twice on average per week and their lifetime with Model Fitness was 5 months and more; nevertheless they also had 1 month left to the end of the contract. It can mean that those are customers who, if leave, change the studio (they were working out intensively, so it looks like they would not just stop doing this). |Maybe it would be a nice strategy to follow those customers who work out a lot and have their contracts coming to the end to have a conversation with them about extending the contract and the things they like/don't like in the studio (keep in mind that 5% of these, core, customers can churn just due to changing life circumstances).|

## Challenges

For the churn tasks, a proper metric should be used to put a higher weight on False Negatives. As a default option, `recall` ia a metric of higherimportance as compared to `accuracy` and `precision`. The main issue of this particular dataset was achieving convergence of the Logistic Regression and better value of `recall`. The Random Forest algorithm results were weaker. For a real business task of that kind futher search for a model with better performance should be considered given the potential improvement of the financial results from prediction of near-to-churn customers and their retention. Different models can be developed for different clusters of customers.
