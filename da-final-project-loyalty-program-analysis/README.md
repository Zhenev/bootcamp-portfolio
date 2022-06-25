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
