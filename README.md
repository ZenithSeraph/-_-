# Predicting Myers-Briggs Personality Types
## Classification of Personality Types From Twitter Postings
### Presented by Rebecca Wright

<br><br>

## Overview

We wanted to create a NLP classifier to determine a person's Myers-Briggs personality type based on their informal social media postings.  An individual can be scored as one of 16 Myers-Briggs personality types.  These 16 individual types are derived from 4 main personality trait categories: Introversion vs Extroversion, Feeling vs Thinking, Judging vs Perceiving, and Intuitive vs Sensing.

### Scope

A [Myers-Briggs Personality Type Database](https://www.kaggle.com/datasnaek/mbti-type) was sourced from [Kaggle](https://www.kaggle.com/datasnaek/mbti-type).

The dataset contained over 8600 rows of individual user data containing:
* Personality Type (a 4 letter MBTI code/type, eg. INTP)
* A block of the last 50 postings made by the user (separated by "|||" (3 pipe characters))

### Questions Considered For Analysis:
* Can correlations be seen between certain MBTI types and any of the following:
    - Sentiment analysis score
    - Avg word count per post
    - Part of speech text composition percentages
    - General word usage


## File Directory / Table of Contents

1. Data folder
    - original_data
        * super_scrape.csv
        * truth_scrape.csv
    - cleaned_data
        * clean_data.csv
        * selftext_custom_preprocessed.csv
        * selftext_preprocessed.csv
2. Code folder
    - 01_data_collection
        * subreddit_scrape_Super.csv
        * subreddit_scrape_Truth.csv
    - 02_data_cleaning
        * combine_subreddits.ipynb
    - 03_eda
        * eda_truth.ipynb
    - 04_modeling
        * 01_refined_models.ipynb
        * 02_custom_preprocessed_base_model.ipynb
3. Presentation folder
    - Reddit_presentation.pdf
4. Images folder
    - contains image files displayed by README.md
4. Scratch folder
    - Files contained are non-deliverables and not to be used for aassignment evaluation.

<br><br>

## Data Section: 

### Initial Sentiment Analysis Performed
* utilized vader sentiment analysis for use as engineered features

### Data Cleaning Performed
* cast to lowercase
* removal of any string containing a digit
* removal of websites
* removal of special characters
* removal of stopwords

### Part-of-Speech Tagging 
* utilized nltk part-of-speech tagger
* calculated percantages of text comprised of each pos_tag as engineered features

<br><br>

## Methods Section: 

### Null Predictive Models

Null models, creating classification predictions based on *most frequent* classification of each binary personality trait had the following results:
* Comparing E vs I:  **77%**
    - I : 0.769568
    - E : 0.230432


* Comparing F vs T:  **54%**
    - F : 0.541095
    - T : 0.458905


* Comparing N vs S:  **86%**
    - N : 0.862017
    - S : 0.137983


* Comparing J vs P:  **60%**
    - J : 0.60415
    - P : 0.39585

### Recurrent Neural Networking Model
A recurrent neural network model was developed that took advantage of the keras tokenizer and word embedder functionalities.  These functions were applied to the cleaned post text, which was then passed through a series of hidden layers as outlined in [the final keras model notebook](models/final_keras_model.ipynb).

When considering if an individual was one or the other of the two personality options considered under each of the four personality types, for example whether a person was an Introvert (I) or an Extrovert(E), the final model was able to classify an individual's personality type within each category at accuracy levels exceeding those of the null model.


![png](kmodel_ei.png)

![png](kmodel_ft.png)

![png](kmodel_ns.png)

![png](kmodel_jp.png)


### Summarized Results

While the final model was able to perform with better accuracy than the null models, a caveat must be added as to the final corpus used by the neural network.  In previous models, a custom_stopword collection had been created to strip all self-references to a users personality type that might be found within the post text content.  It is likely that the classification predictions performed here are influenced by the existence of these personality terms, and further work should be done to reimplement these models on text stripped of the full custom_stopword collection.

### Revisiting The Earlier Additional Questions:
* Can correlations be seen between certain MBTI types and any of the following:
    - *Avg word count per post* **A calculated range of 20 avg words per post was seen between individual personality types of the 16 unique types.  This does not connect to an obvious difference between the averages of the higher level personality trait, but the area warrants further exploration.**
    - *Part of speech text composition percentages* **No dramatic differences were seen when comparing the 4 main personality types internally.**    
    - *Sentiment analysis score* **Features have been extracted and warrant application in future models**
