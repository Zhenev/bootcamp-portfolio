
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

