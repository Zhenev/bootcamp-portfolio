# Predicting Best Plan Based on User Behaviour

## Data:

Every observation in the dataset contains monthly behavior information about one user. The information given is as follows:

- `сalls` — number of calls,
- `minutes` — total call duration in minutes,
- `messages` — number of text messages,
- `mb_used` — Internet traffic used in MB,
- `is_ultra` — plan for the current month (Ultra - 1, Smart - 0).

## Goal:

The ultimate goal stated for the project is to resolve a  a classification task and to develop a model with the highest possible accuracy.

## Libraries used:

pandas | 
numpy |
matplotlib.pyplot |
seaborn |
sklearn |
scipy |

## Contents

* Introduction
* Dataset
* Data overview
* ML
* Conclusions

## Summary

### Dataset
1. We uploaded the dataset, updated column names and variable types, and checked the data for duplicates and missing variables; the dataset at hand, did not have missing values, duplicates, as well as outliers.
2. Out of 3214 users we had a group of 985 who have use the Ultra plan.
3. Users of the Ultra plan seemed to make more calls, talk more minutes, send more messages, and even used more internet.
Results interpretation and model adjustment

### Metrics
For this particular project, using the accuracy score was suggested. Accuracy works good when classes are balanced, i.e. when objects are distributed almost evenly between the classes, approximately 50/50. In our dataset, the classes are not completely balanced. For further improvements, one can consider class balancing, as well as using precision and ROC-AUC as leading metrics:
* Precision looks at the share of correct answers in the predictions deemed belonging to the target class by the model (marked as "1");
* AUC-ROC tries to measure if the rank ordering of classifications is correct.

### Model training
1. We employed Logistic Regression and Random Forest Classifier models; we also used a Dummy Classifier for a sanity check;
2. We used standardization as well, to avoid possible differences in the feature scales;
3. The logistic regression resulted in less promissing accuracy value; other metrics showed insufficient results as well;
4. We focused on tuning the hyperparameters of the random forest model.
5. We achieved better accuracy for some of the hyperparameters, when considering them individually.
6. We tried to employ both grid search and random search to find a better combination of hyperparameter values; the first one requires more computing resources as compared to the latter one, but neither of them resulted any meaningful improvement of the model in terms of accuracy score.
7. The best accuracy score for the final model on the test subset was 0.81. The sanity check showed better performance of the final model as compared to a dummy classifier.
