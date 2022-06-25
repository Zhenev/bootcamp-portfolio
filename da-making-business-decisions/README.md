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
