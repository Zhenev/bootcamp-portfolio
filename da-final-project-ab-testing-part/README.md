# Getting it All from an A/B test

## Data:

Data for the task include:

**ab_project_marketing_events_us.csv** -  the calendar of marketing events for 2020

- `name` — the name of the marketing event
- `regions` — regions where the ad campaign will be held
- `start_dt` — campaign start date
- `finish_dt` — campaign end date

**final_ab_new_users_upd_us.csv** - all users who signed up in the online store from December 7 to 21, 2020

- `user_id`
- `first_date` — sign-up date
- `region`
- `device` — device used to sign up

**final_ab_events_upd_us.csv** - all events of the new users within the period from December 7, 2020 through January 1, 2021

- `user_id`
- `event_dt` — event date and time
- `event_name` — event type name
- `details` — additional data on the event (for instance, the order total in USD for purchase events)

**final_ab_participants_upd_us.csv** - table containing test participants

- `user_id`
- `ab_test` — test name
- `group` — the test group the user belonged to

## Goal:

The ultimate goal of this project is deriving a conclusion with regard to the degree of correctness the test was carried out with.

## Libraries used:

pandas |
numpy |
matplotlib.pyplot |
seaborn |
scipy.stats |
math |
datetime |
plotly |

NOTE: make sure the Jupyter Notebook is trusted to see the interactive plots.

## Contents

* Introduction
* Dataset
* Data preprocessing
* A/B test data analysis
* Conclusions

## Summary

After the initial investigation of the data and pre-rpocessing, we were left with a sample of 2788 users who participated in the recommender system A/B test. In addition, we singled out EU-based customers.

The initial analysis of the conversion rates looked ambiguous, different options of funnels resulted in different numbers, including more purchase events than product carts; the basic funnel resulted in approximately 13% conversion from `login` to `purchase`; we applied the **z-criteria** to compare overall conversions of the two groups and the result **showed that the two distributions are essentially identical**, except for the proportions betweem `login` to `product_page` count. Then, we got to a conclusion that when users who registered before Dec 15, 2020, i.e. during the first week of the test, are filtered, the distrbutions of events by dates look more coherent; thus, we ended up with investigating the results for two subsets, both for unfiltered and filtered data.

Due to the caveats of the A/B test setting, the test, although resulted in some steady and statistically significant results for unfiltered data, showed only partially significant for filtered data:
* 10% increase in overall conversion from `product_page` to `purchase` for group B for unfiltered data.
* 13% lower average purchase size for those users from group B who placed orders compared to group A for unfiltered data; nevertheless, due to better conversion, group B had relatively higher sales per user than group A.
* Statistically insignificant increase in the overall conversion rate from `product_page` to `puchase` for group B on filtered data.

## Challenges

### Artifacts of the dataset

Initially, about 6000 declared test participants were expecte, while the initial data had multiple issues:
* The users in the new user dataset signed up between Dec 07, 2020 and Dec 22, 2020, not 21, 2020, while the events of the new users span within the period from December 7, 2020 through Dec 30, 2020, not Jan 01, 2021 as it was initially communicated.
* 43% of the all events are login, 28.% are visits to product pages, 14.23% are purchases, 14.18% are [presumably adding to] product cart.
* All the purchase events have money amounts assigned to them. Interestingly, the amounts have a discrete distribution, when appr. 2% of the purchase volumes equal USD 499.99, another 9% are of USD 99.99; since it did not look like some random amount, we have left it as is and will consider to be something to take into consideration later down the analysis process.
* Although we were updated that the purpose of the test was testing changes related to the introduction of an improved recommendation system, the `ab_participants` dataset covers additional test, called `interface_eu_test`.
* There were 887 users participated in two tests; since we did not have any prelimenary information on the dates the `interface_eu_test` test was run, to move forward we had to drop those `user_id`'s alltogether, in addition to dropping those who participated in the EU interface test; thus, we were left with 2788 users who participated in the recommender system A/B test, well below the initially declared expected number of test participants: 6000.

We proposed cross-checking the datase for possible artifacts with data engineers on the team.

### Test setting

The following data exploration of the test setting showed:
* Initially , we had 2082 participants in group A and 706 participants in group B; the users of both testing groups were found to be 93% from the EU region; since the test was said to focus on the EU region, we had to clean up our A/B test user list; less than 15% of the new EU users ended up being covered by the A/B test; we have been left with 1939 participants in group A and 655 participants in group B.
* Group B had appr. 30% more users with one event than group A and appr. 20% less users with three events than group A. The share of users with two events was slightly higher and with 4 events was slightly lower in group B than in group A; thus, the number of events per user in group A ended up being slightly higher then in group B, let's check it out.
* When we checked how the events are distributed by the experiment dates, we encountered another interesting issue: the distributions of events by dates had differences for group A and group B and the first week of the experiment looked not so comparable for the two groups, so we decided to check the things out:
* The user intake pattern over the perios differs between groups A and B, though in neither case can be called constant;
* When users who registered before Dec 15, 2020 are filtered, the distrbutions of events by dates look more coherent; however, the difference is still meaningful; on the other hand we loose 36% users of group A and 54% users of group B;
* When average count of events per user is checked, the user behaviour seems to remain stable over time, so given that we should study the conversion, we can tolerate the differences in the user intake or filter out events by the date only;
* When only events before Dec 15, 2020 are filtered, we are left with 1799 participants in group A and 552 participants in group B (9% less);

Eventually, we decided to leave the groups as is for the basic funnel analysis and use filtered by users data for cumulative metrics. With regard to the details parameter, there were no meaningful differences for the groups; we have left it as is, considering the cases with the value USD 499.99 as another peculiarity of the dataset.
