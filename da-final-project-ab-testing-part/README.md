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
