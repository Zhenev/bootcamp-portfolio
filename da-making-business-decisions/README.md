# Prioritize Hypotheses and Look After the AB

## Data:

Data used in the first part of the project, prioritizing hypotheses, contains nine hypotheses on boosting an online store's revenue with Reach, Impact, Confidence, and Effort specified for each.

**hypotheses_us.csv**
- `Hypotheses` — brief descriptions of the hypotheses
- `Reach` — user reach, on a scale of one to ten
- `Impact` — impact on users, on a scale of one to ten
- `Confidence` — confidence in the hypothesis, on a scale of one to ten
- `Effort` — the resources required to test a hypothesis, on a scale of one to ten. The higher the Effort value, the more resource-intensive the test.

Data used in the second part of the project

**orders_us.csv**

- `transactionId` — order identifier
- `visitorId` — identifier of the user who placed the order
- `date` — of the order
- `revenue` — from the order
- `group` — the A/B test group that the user belongs to

**visits_us.csv**

- `date` — date
- `group` — A/B test group
- `visits` — the number of visits on the date specified in the A/B test group specified

## Goal:

The project has two main goals: apply the ICE and the RICE frameworks for choosing the hypotheses to work on and, based on an AB test results, make a decision to 1. Stop the test, consider one of the groups the leader. 2. Stop the test, conclude that there is no difference between the groups. 3. Continue the test.

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats |
seaborn |
math |
datetime |

## Contents

* Introduction
* Datasets
* Data preprocessing
* Data Analysis
* Conclusions

## Summary

Together with the marketing department, we compiled a list of hypotheses that may help boost revenue. My part was to prioritize the hypotheses and then analyze the results of corresponding A/B test.

We uploaded and preprocessed the data. At this step we found that 58 visitors who ended up being assigned to both A and B group. We had no other option rather than to drop their entries all together. After plotting the cumulative revenue and average order size, we quickly discovered the outliers, which we had to drop as well to be able to make sense of the data at hand.

The hypotheses prioritization part lead to some ambiguity to be resolved, altough we still could say which of the hypotheses seemed to be of the top priority.

Then, we analyzed the difference in conversion rates and the difference in order size for the two groups and used **non-parametric method to test the hypoteses** about the significance of those differences. We conducted the tests both on the raw data (without duplicates) and on the filtered data. Both raw and filtered data revealed statistically significant differences in conversion between the groups. By the end of the month, group B demonstarted 14.4% higher relative gain.

Neither raw nor filtered data revealed any statistically significant differences in the order size (revenue) distributions of the groups; actually, the graph showing the difference in average order size between the groups showed that it can be too early to make conclusions.

Based on these facts and given the goal of the project, to reveal a hypothesis that may help boost the total revenue, we were able to conclude that the given A/B test delivered prominent results on the conversion rates, but it had to be continued until the more steady graph of the average order size for the two groups were achieved as well.

## Challenges

### Hypotheses prioritization

We employed two methods, ICE (includes Impact, Confidence, Ease) and RICE (includes Reach, Impact, Confidence, Effort) to prioritize hypotheses. While the TOP5 hypotheses in both cases were the same, with both methods we have two hypotheses in the TOP3 and one hypothesis on the 4th place; one striking difference was in two hypotheses, one of which took the second place with RICE and only the 5th witn ICE, and another one, which showed the same result, but with ICE. The only difference is that in RICE the reach factor is taken into account, the estimate of how many users will be affected by the update. Anyway, drawing upon the two results, we could recommend re-check the scores and to recalculate the ratings.
