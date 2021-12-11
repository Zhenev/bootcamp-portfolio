## Data:

- `children` - the number of children in the family
- `days_employed` - work experience in days
- `dob_years` - client's age in years
- `education` - client's education
- `education_id` - education identifier
- `family_status` - marital status
- `family_status_id` - marital status identifier
- `gender` - gender of the client
- `income_type` - type of employment
- `debt` - was there any debt on loan repayment
- `total_income` - monthly income
- `purpose` - the purpose of obtaining a loan

## Goal:

This is a model project aiming to demonstrate the ability to thoroughly explore and prepare the data (check for artifacts, reveal and fill in missing values in a robust maner, check for and exclude possible duplicates, and categorize), and conduct basic hypotheses tesing (in this case, formulate and test hypotheses to figure out what customers' characteristics have a major impact on the probability of not paying a loan and should be considered in building a credit score model).

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats

## Contents

* Introduction

* Stage 1. Data Overview
    * General Information About the Data
    * Data Exploration
    * Intermediate Conclusions
* Stage 2. Data Transformation
    * Investigating and Filling In Missing Values
    * Intermediate conclusions on the nature of missing values
    * Further investigation of missing values: the role of customers' age
    * Intermediate conclusions on the nature of missing values
    * Further investigation of missing values: the role of loan purpose
    * Final conclusions on the nature of missing values
    * Working with duplicates in individual columns
    * Working with duplicates across the dataset
    * Intermediate conclusions on duplicates
    * Restoring missing values 
    * Final check for missing values for the rest of the variables
    * Data Categorization
* Stage 3. Testing Hypotheses

    * Hypothesis 1: merital status impact
    * Hypothesis 2: number of children impact
    * Hypothesis 3: income impact
    * Hypothesis 3*. number of children for different income levels
    * Hypothesis 4: purpose impact
    * Hypothesis 5: gender impact
    * Hypothesis 6: work experience impact
    * Hypothesis 7: education level impact
    * Hypothesis 8: age impact
* Stage 4. General Conclusion: Findings and Recommendations
