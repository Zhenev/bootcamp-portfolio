# Another Try on Reviews

## Data:

The data is stored in `the imdb_reviews.tsv` file. Selected fields incllude:
- `review`: the review text
- `pos`: the target, '0' for negative and '1' for positive
- `ds_part`: 'train'/'test' for the train/test part of dataset, correspondingly

*The data was provided by Andrew L. Maas, Raymond E. Daly, Peter T. Pham, Dan Huang, Andrew Y. Ng, and Christopher Potts. (2011). Learning Word Vectors for Sentiment Analysis. The 49th Annual Meeting of the Association for Computational Linguistics (ACL 2011).*

## Goal:

In this project, we build a classification model for texts to automatically detect negative reviews.

## Libraries used:

pandas | 
numpy |
matplotlib |
math |
re |
sklearn |
nltk |
spacy |
lightgbm |
torch |
transformers |
tqdm

## Contents

* Introduction
* Data overview and pre-processing
* ML
* Conclusions

## Summary

### Dataset

1. We have uploaded the relevant subset of the source data and have 47331 observations which are equally divided into train and test subsets; no missing values, no duplicates, correct column names.
2. The distributions of the target variable are similar for the two subsets.
3. We have cleaned the `review` column from digits, punctuation and other signs, and transformed the column into lower case.

### ML

We started the modeling part by establishing:

1. An evaluation procedure, which included multiple quality metrics and visualizations.
2. A basic vectorization procedure - the `review` variable vectorized with the basic version of the "bag-of-word" method.
3. A baseline model: a [`DummyClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.dummy.DummyClassifier.html).

We proceeded with:
1. Applying the `LogisticClassifier()` for binary classification and Light GBMclassifier.
2. Performing a Grid Search for Logistic Regression hyperparameters tuning.
3. Running the Logistic Regression model on lemmatized data.
4. Applying BERT language representation by using a pre-trained model called bert-base-uncased (trained on lowercased English texts) and `transformers` library. The latter implements BERT and other language representation models.
5. Testing the final model (Logistic Regression model, applied to TF-IDF weighted bag-of-words for cleaned text data)

To demonstrate how the final model performs, we applied it to our own reviews and printed out the predicted probabilities of a review being positive:

![image](https://user-images.githubusercontent.com/78222587/207990347-cfcb626e-b0fd-47cc-bcb7-af558a713499.png)


## Challenges

### Vectorization

The raw word count of the basic "bag-of-words" approach are not very informative: the feature vectors of most texts in the same language will be very similar, while what we are interested in words which distinguish the most a positive review from a negative, i.e. possess the most meaning: words that occur often in positive reviews are expected to occur less often in the corpus, in general, and in the negative reviews, in particular, and vice versa. To assign more weight to these more meaningful words, we applied the [TF-IDF score](https://en.wikipedia.org/wiki/Tf%E2%80%93idf), which counts the number of times every token appears in a text and divides it by (the logarithm of) the percentage of corpus documents that contain that token. This weighting is performed by Scikit-learn's `TfidfTransformer`. To obtain the weighted feature vectors, we combine the `CountVectorizer` and `TfidfTransformer` in a `Pipeline`, and fit this pipeline on the training data. We then transform both the training texts and the test texts to a collection of such weighted feature vectors. Scikit-learn also has a `TfidfVectorizer`, which achieves the same result as our pipeline.

Another approach, we applied in this project was `BERT`, which transforms text data points into word embeddings, that is, words turn to be embedded into a vector space that represents a language model. Different areas in that vector space have different meanings from the language perspective. A vector made from a word using word embedding will carry some contextual information about the word and its semantic properties, thus, preserving the conventional definition of a word and its meaning in context.

BERT (Bidirectional Encoder Representations from Transformers) is **a neural network model** created by Google to enhance the relevance of search results and was published in 2018 (the original article can be found at: https://arxiv.org/abs/1810.04805). When processing words, BERT takes into account both immediate neighbors and more distant words. This allows BERT to produce accurate vectors with respect to the natural meaning of words. Existing BERT models are usually applied, that are pre-trained (by Google or other contributors) on large text corpora.

NOTE: running BERT for thousands of texts may take long run on CPU, at least several hours; thus we had to limit the sample size. That seemed to be the main reason why BERT embeddings, unexpectedly, results in worse scoring. Also, in a real life setting we would probably fine-tune BERT model using our data before generating embeddings instead of just using the pretrained model.




