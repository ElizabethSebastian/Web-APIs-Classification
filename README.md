# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Web APIs & Classification

## Contents:
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Data Dictionary](#Data-Dictionary)
- [Conclusions](#Conclusions)
- [Limitations and Recommendations](#Limitations-and-Recommendations)


## Problem Statement
To classify Reddit posts from r/Premier league and r/NBA using Natural Language Processing (NLP). The model evaluated and selected based on Accuracy score is expected to predict to which subreddit a given post belongs to.

The model should then help Reddit data science team to advise their advertisers on spending forecast for targeted marketing campaigns of products and services based on the predicted subreddit. Reddit data science team is the primary stake holder and advertising companies are the secondary stakeholders.

**Objective**<br>
The objective is to find out the attributes of each group and their online behaviour:<br>With the information gathered, business can<br>
        1) Decide how much advertising dollars to spend and on which group to obtain better ROI.<br>
        2) Types of products and services to be advertised.<br>
        3) Frequency of advertisement.<br>
        4) Types of advertisement.<br>
        5) KPI expected from Reddit.

## Executive Summary

### Gather Data
 1) Extract posts from the 2 subreddits that records football league system. -Premier League and NBA.<br>
 2) Attempt to extract 75 pages of posts equivalent to approx. 2000 unique posts per subreddit.<br>
 3) Write them to individual .csv files for recording purposes.<br>
 4) Read these .csv files to separate dataframes and examine data extracted.<br>


### Data Cleaning
1) Removed uncessary columns.<br>
2) Found many rows with null values in the column selftext. Created a new colum feature_var that combines title and selftext and used this colum to gather words.<br>
3) Removed duplicated rows.<br>
4) After cleaning the invalid/ unnecessary rows, combine/concat the cleaned dataframes from both subreddits into 1<br>
5) After combining, column feature_var that contains all the texts from the posts is passed to text_processor function that does the following:<br>
    - removes HTML tags using Beautiful Soup
    - extracts words with letters and replaces punctuations and special characters with blank space
    - convert all characters to lower case
    - lammetize the extracted words
    - removes english stop words and extracts meaningful words from the above list of words


### Exploratory Data Analysis
 
1) The subreddits vary hugely in terms of the number of members. NBA has way more followers with 
   3.8m memebers than Premier League with 324k members.<br>
2) Number of unique posts per subreddit is fairly comparable between the 2 subreddits. <br>
3) The number of comments per posts is significantly higher for NBA subreddit, which is not surprising, given that NBA subreddit has much larger members that Premiew League. <br>
4) Another interesting fact is that the average length per post is way higher for NBA subreddit compared 
    to Premier League subreddit. <br>
5) There are quite a number of words that are commonly seen in both subreddits. not surprising as both 
    subreddits are professional baseketball league. Words like 'team','play', 'game' and 'player' are some 
    of them.<br>

### Feature Engineering
1. Binarize the target column - Subreddit column


### Preprocessing, Modelling & Evaluation

#### **Preprocessing**
1. Inspect Baseline Score
2. Performed Train-Test-Split   
3. Done EDA on Count Vectorizer and TD-IDF Vectorizer.

#### **Modeling**
1. Worked with 2 classification models - Multinomial Naive Bayes and Logistic Regression, coupled with Count Vectorizer and TD-IDF Vectorizer
2. Employed Pipeline and Grid Search for all combinations of Vectorizers and Model
    - To tune hyperparameters for both vectorizers and models
    - Identify best score and best parameters for each of the model combinations
    - Fit the train data 
    - Score both train and test data to see model performance

#### **Evaluation & Selection**
1. Getting back to the problem statement, the objective of the project is to classify accurately the subreddit a given post belongs to. The model then helps advertisers market their products and services applicable to the 2 subreddit members based on the nature of the posts within these subreddits.Accuracy metric is a good metrics in classifying the posts. <br>
2. Thus the preliminary basis of model selection is Accuracy metric and TD-IDF with Multinomial Naive Base scored the best for Accuracy on unseen/test data.
3. The variance between training score and test score is also lower for TD-IDF with Naive Base model. 
4. When Accuracy is used as a metric to evaluate models, the following criteria is also checked.<br>
    - Datasets are symmetric.<br>
    - The values of false positive and false negatives should be almost the same.
5. While the model selection is primarily based on Accuracy scores, confusion matrix and ROC-AUC scores were also examined for each of the model combinations to ensure these scores of the model selected doesn't vary significantly with scores of other models. Plotting and inspecting ROC Curve & AUC as:
    - It represents the degree or measure of separability of the model into the 2 classes and is an important 
metric in classification problems. <br>
    - Higher the AUC, the better the model is at distinguishing between the 2 subreddits in this case.

Thus the model selected is **Multinomial Naive model with TD-IDF.**


### **Data Analysis post modelling**
1. Identifying words that appears in the corpus most number of times.<br>
2. Examine the coefficients of features/ words to determine predictive words per subreddit. 




## Data Dictionary

Data dictionary for the final set of features.

| Feature           | Type  | Description                                                                                    |
|-------------------|-------|------------------------------------------------------------------------------------------------|
| subreddit       | object | label or name of the subreddit forum                                                                    |
|title     | object   | title text of the user post  
|selftext       | object   | raw text of the user post                    | 
|name        | object| unique Id of the parent post                               |   
| num_comments    | int | number of cmments per post                                   
| author      | object  | author of the post|



## Conclusions
1) The first thing we can do is to look at the coefficients for every word, sorted 
negative to positive. <br>
2) In this model, we have assigned 'Premier League' comments to class 0 and NBA comments to class 1. So 
the words with the strongest positive coefficients in our model are the words that are 
most predictive of NBA (they push the outcome towards 1 the most). <br>
3) And the words with the strongest negative coefficients are the most predictive of 
Premier League (they push the outcome toward 0). <br>
4) Moreover, the words with the most influential coefficients either positive or negative don't just 
represent the words that are most likely to appear in posts within a given subreddit. <br>
5) They are also unlikely to appear in posts of the other subreddit. In short, the model is looking for words that are highly frequent in one subreddit relative to the other.<br>
6) These predictive words can be potentially employed in targeted marketing campaigns to improve the click- rate of a particular ad to the target subreddit member. <br>
7. Business should get data directly from Reddit using API or RESTful API which is
significantly faster than scraping (limit on posts).<br>
8. Business may want to spend more advertising dollars on the NBA group as higher chance to convert 3.8m users to customers; more bang for the buck

## Limitations and Recommendations
1) The analysis and modeling is based on ~1000 posts per subreddit which may not be a good representation of 
the subreddit itself.<br>
2) Comments were not scraped or included in modeling. <br>
3) The hyper-parameters of the model has been tuned to include features/ words as many as 2500 to reduce 
the variance between testing and training scores<br>
4) But in reality, it may make sense to reduce the number of words<br>
5) The model can be further enhanced :
    - To eliminate synonyms, post agnostic words, common words
    - To include phrases instead of just words for prediction.
    - To include comments as part of post for analysis and modeling<br>
6) The dataset of that the model is trained is likely not representative of the content of the subreddit in the long run. This is because a subreddit like Premier League and NBA changes with current events. As such, there needs to be a way for the model to recognize the type of content in the subreddit apart from individual words.
