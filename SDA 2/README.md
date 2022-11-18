# What Brings More Revenue

## Data:

1. `users` table (data on users):

- `user_id` — unique user identifier
- `first_name` — user's name
- `last_name` — user's last name
- `age` — user's age (years)
- `reg_date` — subscription date (dd, mm, yy)
- `churn_date` — the date the user stopped using the service (if the value is missing, the calling plan was being used when this data was retrieved)
- `city` — user's city of residence
- `plan` — calling plan name

2. `calls` table (data on calls):

- `id` — unique call identifier
- `call_date` — call date
- `duration` — call duration (in minutes)
- `user_id` — the identifier of the user making the call

3. `messages` table (data on texts):

- `id` — unique text message identifier
- `message_date` — text message date
- `user_id` — the identifier of the user sending the text

4. `internet` table (data on web sessions):

- `id` — unique session identifier
- `mb_used` — the volume of data spent during the session (in megabytes)
- `session_date` — web session date
- `user_id` — user identifier

5. `plans` table (data on the plans):

- `plan_name` — calling plan name
- `usd_monthly_fee` — monthly charge in US dollars
- `minutes_included` — monthly minute allowance
- `messages_included` — monthly text allowance
- `mb_per_month_included` — data volume allowance (in megabytes)
- `usd_per_minute` — price per minute after exceeding the package limits (e.g., if the package includes 100 minutes, the 101st minute will be charged)
- `usd_per_message` — price per text after exceeding the package limits
- `usd_per_gb` — price per extra gigabyte of data after exceeding the package limits (1 GB = 1024 megabytes)

## Goal:

The ultimate goal stated for the project is to determine which prepaid plan brings in more revenue for the company to optimaze the marketing effort; thus, we are going to analyze clients' behavior and determine which prepaid plan generates more revenue.

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
* SDA
* Summary and conclusions

## Summary

Initital data overview showed, that:

* At our disposal was data on 137735 calls, 76051 messages, and 104825 internet sessions for 500 unique users in 73 MSAs.
* The traffic dataset span over 50 weeks, namely 351 day from Jan 15, 2018 to Dec 31, 2018.
* The traffic data covered the users who registered starting from Jan 01, 2018.
* Some of the users started churning as early as on Jul 31, 2018.
* No issues were found about the data, except for missing values in churn_date variable; according to the note above this fact meant that the calling plan was being used at the moment the data was retrieved; from that we have learned that 6.8% of the users in the sample stopped using the corresponding plan.

At the data preprocessing step, we fixed data types, treated observations with zero duration and zero internet usage, checked the data for trends in service consumption, explored user behaviour, and calculated the monthly revenue from users.

The exploratory data analysis showed, that:

* Overall, the users of the both plans mostly used less than 500 minutes of calls per month and sent less than 50 messages, with ultimate users sending appr. 6 messages more each month.
* Both the `surf` users and the `ultimate` users used 17-18 Gb of the internet traffic per month on average, which resulted in significantly higher (as compared to the monthly basic price) extra cost of the internet traffic for the `surf` users: on average they were billed appr. USD 40 of extra payments per month.
* There were too few users in the beginning of 2018 in the data sample; thus, we had to drop the first three months of mobile traffic observations to get more robust estimates for monthly stats (the sample included 23 surf users in March and 21 ultimate users in April).
* Despite the fact that on average the `surf` users still paid less than the `ultimate` users, the `surf` users had experienced a steep growth in the monthly payments: by the end of 2018, they were billed as much as the `ultimate` users, which was due to the growth in the extra payment for the mobile internet traffic.

Before running the hypotheses testing at the statistical data analysis step, we made additional **check of our basic assumtion of the normality of the revenue distributions** (we rejected this one).

Then, we tested three hypotheses of different character:

* `surf` and `ultimate` samples have equal total_payment means;
* `surf` and `ultimate` samples have equal total_payment means for December 2018;
* NY-NJ-PA MSA sample of users has total_payment mean equal to that of the users from the rest of MSAs, for each of the payment plans.

For each hypothesis about the mean, we tested the **hypothesis of the equality of the variances** (this one was rejected as well).

On one hand, with regard to the difference between two payment plans, the first hypothesis was rejected; on the other hand, as we have seen, the average payment for the `surf` users started to be similar to that of the `ultimate` users in December 2018. Indeed, when tested, the second hypothesis could not be rejected. Given that the `surf` users had been steadily using more internet traffic, the conclusion would be that the two plans generate the same average revenue when users consume 19 GB of the internet traffic and the `surf` plan was supposed to generate even more revenue on average per user if more users continued expanding their mobile internet usage. 

With regard to the NY-NJ-PA MSA, there was not enough evidence that the revenue per user in this MSA was different from that in other MSAs, i.e. the hypothesis of equal means was accepted.

And finaly, we illustarted the fact, that due to the higher number of the `surf` users, this payment plan had always brought in more total revenue.

## Challenges

The data preprocessing part demanded a high level of attention:
* We converted the date columns into the date type and added a month column to each one of the traffic-related dataframe.
* We have spotted zero duration calls, which comprised 19.4% of the initial dataset on calls and which essentially mean missed calls; we have dropped them to reveal the actual distribution of the call duration.
* We have spotted zero usage internet sessions, which comprised 13.1% of the initial dataset on internet sessions and which essentially can mean either rounding up artifact or unsuccessful connections; we have dropped them to reveal the actual distribution of the internet traffic usage.
* For the user sample at hand, we explored the daily growth of the "user base" for the two payment plans of interest (by the end of 2018, there wre 316 active `surf` users and 150 active `ultimate` users in the sample) and revealed upward trends in the overall mobile services consumption for each type of service (calls, messages, internet); the trends looked unexpectedly (we have added plan-wise viz on them during the EDA stage).
* Finally, we explored individual monthly mobile service consumption and the total cost of it for the users: we explored the behavior, i.e. the number of calls made and minutes used per month, the number of text messages sent per month, the volume of data consumed per month, and calculated the monthly revenue from each user, including the extra costs incurred for the extra traffic, besides the volumes included in the plans.
