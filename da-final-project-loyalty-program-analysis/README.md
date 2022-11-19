# What Makes a Loyalty Program a Success

## Data:

The main dataset at our disposal, `retail_dataset_us.csv`, contains data on purchases made at the building-material retailer Home World:

- `purchaseId`
- `item_ID`
- `purchasedate`
- `Quantity` — the number of items in the purchase
- `CustomerID`
- `ShopID`
- `loyalty_program` — whether the customer is a member of the loyalty program

There is additional file, `product_codes_us.csv`, which contains:

- `productID`
- `price_per_one`

## Goal:

The initial goal of the project was stated "to analyze the loyalty program"; after the decomposition we have clarified the goal and defined it as "to analyze how much effective is the loyalty program in expanding the total customer value of existing customer base and find ways to boost its effectiveness".

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats |
seaborn |
math |
datetime |
plotly |

NOTE: make sure the Jupyter Notebook is trusted to see the interactive plots.

## Contents

* Decomposition
* Dataset
* Data preprocessing
* Data analysis
    - Period under investigation
    - Number and share of members and non-members of the loyalty program
    - Distribution of cheque sizes for the two groups
    - Total sales to both groups
    - Cohort analysis
        - The date of the first purchase;
        - The date of joining the loyalty program;
        - Number of buyers by cohort lifetime for members and non-members;
        - Number of purchases per customer by cohort lifetime for members and non-members;
        - Average purchase size by cohort lifetime for members and non-members;
        - Initial cohort sizes and total revenue by cohort for members and non-members (to calculate the LTV);
* Hypothesis testing
    - Choose the hypothesis for testing;
    - Explain the set up and the test to be employed;
    - Formulate the null and the alternative hypothesis;
    - Implement testing;
    - Elaborate on the results;
* Basic Dashboard
* Conclusions

## Summary

#### The main conclusions from the cohort analysis were:

* Only for the cohort of the first week of December 2016 and the next cohort 10%-12% of the customers returned every week, for the rest of the cohorts the results were worse; the non-member group showed slightly better retention, higher average number of purchases per customer_id.
* Besides one spike (after the return from the winter holidays), the mean revenue was lower for the member group, than for the non-member group.
* LTV of the non-member group seemed to be higher than for the member group in general, while by the end of a three month period one could expect the LTV of 300+ money units and about 400 money units for the member and non-member group correspondingly.

#### We have performed testing of three hypotheses:

* The average cheque is higher for program members;
* The retention rate is higher for the program members;
* There is difference in the LTV for the two group.

#### The hypotheses testing supported the conclusions from the cohort analysis:

* The average cheque size was shown to be significantly higher for the non-member group than for the member.
* The retention rate did not show any statistically significant difference for members and non-members.
* The hypothesis of equal LTV's for the member and the non-member groups was rejected.

#### Thus, the only recommendation here can be conducting a deep review of the loyalty program terms with a major makeover following it.

#### Based on the analysis above, starting with the following dashboard could benefit the marketing department in their monitoring of loyalty programs dynamics:
* A diagram showing the absolute number of purchases (unique purchase_id's) per day, including an indicator of the number of active customers.
* A diagram showing the share of the two groups in purchases per day.
* A diagram showing the distribution of active customers by purchase week.
* A diagram showing the total revenue by purchase week.
* Applicable filters: store ID, loyalty program membership.

Comment 1: In Tableau we should filter out Null values for purchase_id (Tableau replaces "C-purchases" with Nulls, since the letter at the beginning turns them into strings) and customer_id.

Comment 2: The revenue diagram requires building relationship between the retail_dataset and the product_codes tables.

The [link](https://public.tableau.com/views/DA-FinalProjectScatch-v3/HomeWorldLoyaltytrends?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link) to the dashboard on Tableau Public.

## Challenges

The current study revealed multiple issues with the data and with regard to the results of the loyalty program under consideration.

For testing the average cheque size and LTV's, we applied both the **Wilcoxon-Mann-Whitney non-parametric test** and the **Welch's modification of the t-test**; testing for equal variances with the **Levene test was performed before applying the t-test**; for testing the retention rates, the **z-test for equal shares** was applied for the first 4 weeks of the observations after a customer joined the customer base.

## Appendix 1. EDA overview
1. The purchases dataset covered a period of three months between Dec 01, 2016 and Feb 28, 2017; naturally, there is a gap in the data between Dec 24, 2016 and Jan 3, 2017.
2. Before the cheque size study:
* The two groups, the member and the non-member, turned to consist of different customers, meaning that every one was only in one status through out the period under investigation and did not performed transition from one of the groups to the second one.
* All the purchases through the loyalty program were performed by 587 customers who made 1344 purchases out of 4146 purchases in total in the dataset.
* For the subset of non-member purchases, we had data on 1162 customers who made 2802 purchases in total; thus, the share of the loyalty program members was appr. 33.6% and they are responsible for 32.4% of the purchases.
3. During the cheque size study:
* We revealed cheques with positive, zero, and negative values of the size; the cheques with negative cheque values were marked with the letter "C" at the beginning og the purchase_id.
* Further investigation showed that 310 items on the price list had zero prices from the very beginning, while 1849 items had non-zero price, but entered some purchases with zero quantities; 17 item_id's were revealed that enterd the purchases with negatives quantities only.
* We learned that "positive cheques" contained only positive or zero item_id_total's, negative cheques contained only negative or zero item_id_total's, while all shops have cases of negative cheques.
* It could be that the letter C stands for returns; however, we also revealed that with that assumption, customers would return items before purchasing them and to the shops different from those they ended up with regular ("positive") cheque size values. Thus, we could not conclude univocally that those "C-purchases" "behave" like returns and dropped them until we receive additional information from the sales department.
4. After dropping the misterious purchase_id's, we were left with 3490 purchases, performed by 1688 customers; the proportions of customers and purchases remained similar - 33.6% of them members of the loyalty program and their share in total purchases is 33.3%.
5. After getting back to the cheque size distribution, we found 175 outliers above 775 money units, while the median was at 140; those purchases were perfrmed by 44 customers; we filtered them out and ended up having 558 customer_id's for the member group and 1086 customer_id's for the non-member group with average cheque size higher by 20% for the non-member group and the same proportion of ~33.9% for the member headcount and purchase count.

## Appendix 2. Overall observations from the cohort analysis
1. The cohort counts study showed that, in general, only for the cohort of the first week of December 2016 and the next cohort 10%-12% of the customers returned every week, for the rest of the cohorts the results were worse.
2. The churning rate study revealed that the two groups exhibited similar behaviour with 80%-90% churning in the first two weeks; the return from holidays in the first week of January 2017 looked like a whole campaign, but it never helped; the first cohort was the most stable, while pointing at even better retention for non-members.
3. The retention rate analysis revealed that the retention rate was only slightly better in the non-member group for almost every week in the first cohort.
4. The first cohort was the most active for the both groups in terms of purchase counts.
5. In terms of average number of purchases per customer_id, the non-member group showed slightly better result; in general, the average number of purchases per customer_id values were between 1.0 and 2.0 in a week.
6. Average purchase size did not follow any particular pattern; in general, the average purchase size was higher for the first two weeks and for the first three cohorts; the reopening "campaign" in January 2017 resulted in much higher avergae purchase size in the member group, followed by a meaningful drop just afterwards.
7. In the first weeks the mean revenue was lower for the member group, than for the non-member group; there was a spike in the mean revenue by week for the member group at the week after the winter holidays, while the mean revenue from the non-member group grew steadily.
8. The analysis of the lifetime value metric showed that, for the member group, one could expect the LTV to be sligthly higher than 300 money units by the end of three months; for the non-member group, one could expect the LTV to be closer to 400 money units by the end of three months and the LTV of the non-member group seemed to be higher than for the member group in general.
