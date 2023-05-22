# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Classifying Reddit Posts from Trump and Biden Subreddits using NLP Classification

## Overview
The United States is preparing for an upcoming presidential election, which will determine who will be the next leader of the country. The current Trump administration, led by President Trump, is in the process of hiring new employees. The selection process is based on interviews and projects, which are used to evaluate the candidates' skills and abilities.

As part of the hiring process, we have been tasked with completing a project that involves using natural language processing (NLP) classification to determine if a given post is from the Trump or Biden subreddit. This project will require us to analyze and categorize large amounts of text data from both subreddits, using NLP techniques to identify patterns and features that distinguish between the two.

## Contents:
- [Problem Statement](#Problem-Statement)
- [Data dictionary](#Data-dictionary)
- [Conclusions](#Conclusions)
- [Future Work](#Future-Work)

## Problem Statement
The goal of this project is to use NLP classification techniques to accurately identify whether a given post is from the Trump or Biden subreddit. This task is important because it will help to shed light on the political preferences and attitudes of the users of these subreddits, which may be indicative of broader trends in American politics. Additionally, the project will help to evaluate our skills in NLP and machine learning.

For our project, we will only be focusing on the metric accuracy. This is because the goals for project is simply to determine whether the post is classified correctly.

## Data dictionary
|Feature|Type|Description|
|---|---|---|
|subreddit|int|0,1 values, 0 if Biden, 1 if Trump|
|selftext|object|Reddit poster's body text|
|title|object|Reddit poster's title text|
|combined|object|Reddit poster's selftext+title|

## Conclusions
For the specific subreddits we scrapped, we ran into problems with overfitting of the train data. We have tried 3 methods (1 of them is not shown) to deal with the overfitting problem. We started by reducing the criterion (number of characters) from 80 to 60 for `title` to be deemed to "contain" enough words to be substituted/added to `selftext`. This helped to increase the number of documents we have by close to double, it did little to reduce overfitting though.

Next, we tried to remove words that occur frequently but do not have high coefficients. That is, we have specified those words as noise and have tried to remove them. Our attempts did not seem to be too successful as both the train and test scores were dropping with the overfitting problem not improving.\
On hindsight, our method for selecting the words to remove seems too rudimentary, where we only considered if the words occur with high frequency and if they are in the top 50 words with large coefficients. We could instead have specified that for some limiting frequency f, and some limiting coefficient c, if frequency of of word > f and coefficient of word < c, then we drop that word.

Lastly, we used `modeling_worker` to "guess" which combination of estimator and transformer would give a good score that is also not overfitted. We then proceeded to tune that particular combination in an attempt to produce a good model. It turns out, either we were tuning the model in the wrong way, or using such a method to choose which combination to use is equivalent to trying to predict the lottery. 

At the end of all the methods that we have attempted, we find that the model produced in the very first iteration is the "best", not in terms of least underfitting but taking into account the trade off between being overfitted but still being able to predict well.

To conclude, we will talk about the top words and their coefficients. In particular, for the method where we were removing words, by the third iteration, it seems that the coefficients for the words became very significant, but recall that the scores have also dropped as well, meaning that even though the words are very significant, they are only able to explain a smaller part of the variance as compared to if we had more words. I.e., it is not very helpful to talk about the coefficients of a word if the model score was very low in the first place. 

## Future Work
In an extension of our work, once we obtain a reliable model, we could generate posts that would look like they are from a certain subreddit based on the top words, perform sentiment analysis to determine if it is a positive or negative post, and use it accordingly to skew public opinions. In such a case however, we would probably want to use a different metric from accuracy, as we would not want to accidentally post a negative sentiment post on Trump's reddit.