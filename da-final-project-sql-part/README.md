# Digging for a Value Proposition with SQL

## Data:

At our disposal, we have a database with five tables:

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

The ultimate goal of this project is analysing the dataset from multiple angles to build a base for a value proposition for a book reading app.

## Libraries used:

sqlalchemy |
pandas |

## Contents

* Introduction
* Dataset
* Data overview
* Running research queries
* Summary and conclusions
