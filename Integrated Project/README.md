# What Makes a Video Game Successful

## Data:

- Name
- Platform
- Year_of_Release
- Genre
- NA_sales (North American sales in USD million)
- EU_sales (sales in Europe in USD million)
- JP_sales (sales in Japan in USD million)
- Other_sales (sales in other countries in USD million)
- Critic_Score (maximum of 100)
- User_Score (maximum of 10)
- Rating ([ESRB](https://www.esrb.org/ratings-guide/), the Entertainment Software Rating Board evaluates a game's content and assigns an age rating such as Teen or Mature)

## Goal:

The ultimate goal stated for the project is, based on the previous years sales data, to determine the most successful platforms/genres/games and make recommendations with regard to the hotspots and the target audience of the next year's advertising campaign.

## Libraries used:

pandas | 
matplotlib.pyplot |
scipy.stats |
seaborn |
scipy.neighbors |

## Contents

* Introduction
* Dataset
* Data overview
* Data preprocessing
* EDA - revealing user patterns
* SDA - testing user preferences
* Summary and conclusions

## Summary

We were provided with historical data on games for various platforms, their genres, user and expert ratings, ESRB ratings and sales across different regions. Initital data overview showed, that:

* At our disposal is a dataset on about 16700 games.
* The games' release years spanned over 37 years.
* These are games in 13 genres for 31 platform.
* For appr. 50% of the dataset the critic rating was lacking, and for appr. 40% the user and the ESRB score was lacking.
* Sales data on 2016 seemed to be incomplete.
* There were minor issues with column names (uppercase) and duplicates.

At the data preprocessing step, we:

* Fixed the column names and converted the relevant columns to more appropriate types.
* Explored the data for potential errors and artifacts other than missing values, such as duplicates, outdated platforms; in addition, we revealed the three generations of video games platforms.
* Added the total_sales variable, for the sake of further analysis.
* Tried several methods to fix the missing values and chose the one which seemed the most appropriate.

The exploratory data analysis showed, that:

* Video games platforms come and go in generations; it takes 4 to 5 years for a platfrom to grow and 5 to 7 years to fade.
* In 2010th, 5 platfroms were launched and, as we have revealed, two of them, PS4 and XOne, were growing faster than others; actually, in 2015, the total sales for PS4 are double of those for XOne and together they brought in two thirds of the overall sales (USD 180 mln) for the Ice store.
* The analysis across regions revealed that the lists of leading genres, in terms of sales, were similar (action, sports, shooter), except for Europe, where the list was completely different: role-playing became the leading genre, action games appeared at the second place and miscellaneous at the third; sports moved to the 4th place.

Before running the hypotheses testing at the statistical data analysis step, we made additional **check of our basic assumtion of the normality of the user rating distributions** (we rejected this one).

Then, we tested two hypotheses:

* Average user ratings of the XOne and PS4 platforms are equal;
* Average user ratings for the Action and Sports genres are equal.

For each hypothesis about the mean, we tested the **hypothesis of the equality of the variances** (this one could not be rejected). Then we chose **one-sided alternative hypotheses**, so that we could show that, on average, the games for PS4 receive higher scores from users than games for XOne; the same is valid for Action - Sports games pair.

Based on these observations, our conclusion with regard to the advertising campaing for 2017 would be:

* Focus on the games for users playing on the PS4 and XOne platforms;
* Among them to focus on Action and Sport genres (the popularity of the Shooter genre can have psychological roots and it seems to be self-propagating; besides, promoting shooter games can be against someone's values as promoting violence);
* For the new games, choose those with higher critic ratings (in contrast to the user ratings, critic ratings have higher degree of correlation with sales and, in case of sport games, relatively high correlation with the user ratings themselves);
* Remember that the largets markets for the company are Japan and North America.

## Challenges

### Treating missing values

We applied several techniques to treat the missing values:

* First, we applied multi-category sub-grouping and corresponding sub-group median estimates to restore missing values, but ended up with obviously distorted distribution.
* Then we developed another approach, generated additional values from the sample distribution of corresponding variable without missing values, category-wise as well.
* Still, we saw some difference in the distributions of resulting constinuous variables, critic_score and user_score, in the final dataframe compared to the initial games dataframe, which urged us to explore multiple models for their probabiltiy density estimation: in addition to gaussian kde kernel with different scalar values of bandwidth, Scott's Rule and Silverman’s Rule for bandwidth estimation have been applied as well, with the gaussian kernel, as well as tophat kde kernel with different scalar values of bandwidth. The final choice of the model ('tophat' with bandwidth=1) was guided mostly by a visual estimate and the search for the least change in the sample median values.

Unfortunately, the resulting distributions of the `critic_score`, `user_score`, and `rating` variables are unexceptable, so we could not employ the full dataset when any of these three variables is included in the analysis; nevertheless, we employ the resulting "final" dataset, when possible, to not exclude the relevant sales data.

### "Counterintuitive" facts

It is worth noting that the analysis of correlations between total sales and ratings for PS4 demonstrated some degree of correlation between `critic_score` and `total_sales` and near-zero correlation between `user_score` and `total_sales`. When run against different genres, the correlation matrix for action games revealed some positive correlation between `user_score` and `total_sales`, while the correlation matrix for sports games showed high correlation between the ratings themselves, but near-zero correlation between the `total_sales` and `user_rating`. Shooters demonstrated even negative correlation between `user_score` and `total_sales`, which might be something worth to ask an axpert about.
