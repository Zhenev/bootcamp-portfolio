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

This is a model project aiming to demonstrate the ability to thoroughly explore and prepare the data (check for artifacts, reveal and fill in missing values in a robust maner, check for and exclude possible duplicates, and categorize), as well as conduct basic hypotheses tesing.

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

## Summary

To make the right decisions on the structure of a future credit scoring model, a bank requested to reveal sound assumptions with regard to customers' characteristics which have the most impact on the probability of a particular customer to payout the loan. To this purpose, we formulated a number of starting hypotheses to test on the existing data provided by the loan division of the bank, which we performed after ensuring the highest possible data quality.

Based on their experience, the loan division of the bank suggested that the merital status and the number of children should have considerable impact. Thus, our initial list of hypotheses included:

* Marital status has an impact on whether the client will default on a loan (one of the initial assumption provided by the loan division).
* Number of children has an impact on whether the client will default on a loan (additional assumption provided by the loan division).
* Other variables have impact as well.

First, we explored the quality of the data, revealed its issues and elaborated on the most appropriate ways to fix them. Then, during the data preprocessing stage, we filled in the missing values, checked for duplicates and other artifacts, and categorized the data to enable further hypotheses testing.

At the hypotheses testing stage we calculated the default rates for different sub-groups of cutomers and conducted a number of Welch's t-test to test selected hypotheses. We concluded that the following customers' characteristics do have impact on the probability of a particular customer to payout the loan (presumably, from the highest to lowest):
* Gender;
* Working experience;
* Children (yes/no);
* Age group;
* Family status;
* Income level.

Despite the very fact of having children had a tangible impact on the probability for a customer to payout the loan, we had to reject the hypothesis considering the impact of the number of children, both in the general sample and in the two lowest income sub-samples. The impact of the loan purpose and education level should be considered as questionable. Further investigation is needed.

With regard to the development of the credit score model, our recommendation so far would be to make sure that the six variables above are included into the model. In addition, we could discuss with the loan division the issues around the data, which would hopefully result in the data of higher quality.

## Main challenges

While conducting the initial overview of the data, we found that the data sample consisted of 21525 data points. It did not have any specific issues with column names and the data types looked appropriate. Three significant issues with the data quality have been immediatelly revealed:

* Negative values for days_employed variable and value(s) with what seems to be impossibly high number of days to work;
* Implicit duplicates / variaty of options in `education` and `purpose` column (treated by grouping or renaming some categories);
* 10% of the rows, which comprises a considerably large piece of data, missed values for the `days_employed` and `total_income` variables.

After detailed comparison of the value distributions for each variable in two subsets of the initial data, one where the values for `days_employed` and `total_income` were both missing and another one, with complete set of data, we did not find any specific pattern which could explain the missing values. Thus, we concluded that missing values were missing randomly. Possible reasons for their appearance can be technical issues during data entry or, later on, during data retrival from the bank's database, like incorrect SQL query. This would be my first assumtion to address to the loan division. Deeper in, lack of integrety in the bank's database could be also the case.

It should be noted that we were not provided with unique identifiers for the customer entries also. Theorethically, this can lead to some entries from the subset with missing values be duplicating entries from the data subset with complete set of values. While the columns with complete data cannot and should not define a customer specifically enough (we do look for groups of people with similar characteristics after all), we had to assume that those are unique customers for whom `days_employed` and `total_income` values were missing.

To restore values in `total_income` we analyzed the income distribution in our "complete" data subset and did this for age groups (constructed beforehand), gender, and education. We drew upon the already common knowledge that those are the variables having the most impact on the income level: years of education are thought to have positive impact, women are still paid less, even in high-tech, and there are different periods of life when people can have more income or less, depending in which period they find themselves. We checked our assumtion that the median is a better indicator for `total_income` than the mean (income typically has more issues with outliers). Then we created special function to extract the median `total_income` values for each of the resulting sub-categories and to replace the missing values for which we used the `.fillna()` method.

While restoring values in `days_employed`, we saw a whole subset of strange day of experience values (with the mean of 365000 days) which was entirely about retiree customers, while the "regular" subset did not have retiree customers at all. To properly process it, we added an `income_type` variable to the list of columns to group by and included this column into constructer of customized customer sub-groups to have a higher precision of the median values used to fill in the missing values.

To wrap up the data preparation, we conducted additional check for any minor issues left for other variables bedore proceeding with the hypotheses testing.
