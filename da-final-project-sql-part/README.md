# Digging for a Value Proposition with SQL

## Data:

At our disposal, we had a database with five tables:

- **books** - contains data on books:

    - `book_id`
    - `author_id`
    - `title`
    - `num_pages` 
    - `publication_date`
    - `publisher_id`

- **authors** - contains data on authors:

    - `author_id`
    - `author`

- **publishers** - contains data on publishers:

    - `publisher_id`
    - `publisher`

- **ratings** - contains data on user ratings:

    - `rating_id`
    - `book_id`
    - `username` - the name of the user who rated the book
    - `rating`
    
- **reviews** - contains data on customer reviews:

    - `review_id`
    - `book_id`
    - `username` - the name of the user who reviewed the book
    - `text` - the text of the review

## Goal:

In this project, we analysed a dataset of four tables residing in a database; we aspired to look at the data from multiple angles to build a sound basis for a value proposition for a book reading app. Technically, the project represents a collection of **SQL queries**, from the simpliest ones to more sophisticated. 

## Libraries used:

sqlalchemy |
pandas |

## Contents

* Introduction
* Dataset
* Data overview
* Running research queries
* Summary and conclusions

## Summary

In the first part of the project, we investigated the dataset. below are our main conclusions.

#### Books
1. There are 1000 items in the books table.
2. The earliest book was published in December 1952 and is called A Streetcar Named Desire; the latest one is called A Quick Bite (Argeneau #1) and was published in March 2020; there are no nulls; thus every book has data on the publication_date.
3. The table seems to have one duplicate entry.
4. The duplicated book is Memoirs of a Geisha. Let's see wheter there is a reason why this book entered the dataset twice; this book has two entries since there are two publishing houses published it.
5. The 999 books in the books table come from 340 publishers with the leading one having 42 books on the list.
6. The 999 books in the books table come from 636 authors with the leading one having 15 books on the list.

#### Authors
1. There are 636 persons in the authors table (we have assumed that there are no intrinsic duplicates in the name list).
2. No duplicated entries in the authors table.
3. Terry Pratchett is the most effective writer!

#### Publishers
1. There are 340 entries and no duplicates in the publishers table.
2. Penguin Books are the leaders and have their sister Penguin Classics among the TOP5 publishers as well.

#### Ratings and Reviews
1. Every book in the dataset has rating at least from one user.
2. It turned out that Memoirs of Geisha is also the most reviewed book.
3. Only 6 books do not have reviews.
4. The user with the username susan85 is the top reviewer with almost 30 reviews.

In the second part of the analysis, we conducted a more sophisticated investigation of the data as a whole. We arrived to the conclusions below.

#### Complex queries
1. 821 book was published after January 1, 2000.
2. For each book, we created a dataframe with the number of user reviews and the average rating (avg_rating_with_review_count_per_book).
3. Penguin Books is the publisher that has released the greatest number of books with more than 50 pages; Penguin Classics is the publisher that has released the greatest number of books with more than 500 pages.
4. J.K. Rowling (!) has the highest average rating between authors with more than 50 ratings in total.
5. 6 users in the dataset rated more than 50 books; these users created 24.3 reviews per user on average.

### Value Proposition
The ultimate goal of a book reading app would be to maximize the engagement time with the app; thus, for a value proposition, it might be a good idea to put more focus on recommending the books which raise higher emotional attachment, like those which get higher ratings and more reviews, and build a feeling of a quality list and a club. To this end, in turn, it may be of a high value to build a community of reviewers around the app and encouarage more users to contribute. Of course, the need for personalization by genre preferences and personal goals should be taken into account from the very beginning.
