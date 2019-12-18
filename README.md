# Improving Customer Service through Tweet Classification

## Abstract

Evidence suggests that customers are dissatisfied with the service provided by the "big four" mobile network operators (EE, O2, Three and Vodafone).  Meanwhile, across multiple industries, customers are increasingly turning to Twitter for support.  Combined, these two observations are the motivating force behind this project.  This project aims to provide mobile network operators with a tool to help them exploit the potential of Twitter, and thereby increase their standing in the eyes of the customer. 

This is achieved through a Natural Language Processing (NLP) model which, categorises tweets according to subject (e.g. network, device, customer service etc.) and sentiment (e.g. positive, neutral, negative).  The objective of this model is to help businesses efficiently handle pre-categorised tweets, access an on-the-pulse measure of customer satisfaction and identify key areas for service improvement.  


## Background

### Mobile Operators & Customer Satisfaction 

In the UK, there are only 4 mobile operators that own and operate network infrastructure.  These are:

- EE
- O2
- Three
- Vodafone

As of 2018, these giants accounted for 87% of all subscribers<sup>1</sup>.  With the remaing 13% subscribing to a variety of different virtual networks that 'piggyback' off the infrastructure of the major players.  These include companies like  GiffGaff, Plusnet and Tesco Mobile.

However, in spite of their dominance, the 'big four' rate poorly in relation to the smaller virtual network operators.  A recent survey by Which?<sup>2</sup> asked customers to rate mobile providers on four criteria:

- Customer Service
- Ease of Contacting
- Value for Money
- Incentives

The results revealed that O2, EE and Vodafone occupied the bottom three places, whilst Three ranked a mediocre 8 out of 13.

|Rank   |Mobile Network Provider |
| :---: | :--------------------  |
| 1     | Giffgaff               |
| 2     | Utility Warehouse      |
| 3     | Plusnet Mobile         |
| 4     | Tesco Mobile           |
| 5     | Asda Mobile            |
| 6     | Sky Mobile             |
| 7     | ID Mobile              |
| 8     | Three                  |
| 9     | BT Mobile              |
| 10    | Virgin Mobile          |
| 11    | O2                     |
| 12    | EE                     |
| 13    | Vodafone               |

It is therefore clear that the major networks need to take action to improve the level of satisfaction amongst their customers.  Any complacency will likely weaken their position in the market. 


### Twitter as a Platform for Customer Support

Increasingly, customers are turning to Twitter to for support.  The chart below plots this phenomenon.  

![Customer Service on Twitter](https://github.com/isobeldaley/categorising-tweets/blob/master/images/customer_service_on_twitter.png)

This turn to "social customer care" is unsuprising.  Twitter is public.  It facilitates a one-to-one interaction between customer and company, but with the added pressure (to the company) of an audience.  

Twitter therefore provides companies with an amazing opportunity to publicly demonstrate their ability to serve customers well.  A 2016 study by Twitter<sup>3</sup> illustrates the dividends that this can pay.  It found that 96% of users who used Twitter to contact a company for customer service, and then had a positive experience, would buy from the business again.  Moreover, 83% of them would recommend that brand to others.  

The emergence of Twitter as a customer care tool therefore provides the major mobile network operators with an important opportunity to improve their ratings.  

## Business Case

Given the above, the purpose of this project is to provide mobile network operators with a tool to help them exploit the potential of Twitter, and thereby increase their standing in the eyes of the customer.   

This will be achieved through an Natural Language Processing (NLP) model which, enables tweets to be categorised according to:

1. **Subject** (e.g. network, device, customer service etc.)
2. **Sentiment** (e.g. positive, neutral, negative)

This will help businesses to:

- Efficiently handle pre-categorised tweets
- Provide an on-the-pulse measure of customer satisfaction
- Identify key areas for service improvement


## Project Approach

The **OSEMN framework** has been used to structure this project.  This approach guides the data scientist through the five key stages of a data science project:

1. **O**btain Data
2. **S**crub Data
3. **E**xplore Data
4. **M**odel Data
5. i**N**terpret Results

The following five subsections summarise the approach to each stage.  Each section heading is a link to an annotated Jupyter workbook in which more detail can be found. 

### [Obtain Data](https://github.com/isobeldaley/categorising-tweets/blob/master/Step%201%20-%20Obtain%20Data.ipynb)

The Twitter API was used to obtain data.  The API was accessed using Tweepy, a Python library built to facilitate interaction with Twitter.  Using this approach, Tweets directed at each of the four manjor networks were obtained for the preceding 7 days.  This yielded 16,145 tweets.  

To progress with a supervised learning approach, a significant number of these tweets had to be labelled.  As such, the tweets were exported to Excel, where 4,377 of the tweets were categorised, according to their subject, into one of seven groups:

- **Network**: tweets related to signal/coverage
- **Customer Service**: tweets related to level of service received
- **Device**: tweets related to  a device (e.g. phone, tablet, smart watch etc.)
- **Contract**:tweets related to a contract question(e.g. "How do I terminate my contract?", "How can I add data to my allowance?" etc.) 
- **Broadband**:tweets related to Broadband provision by the mobile network operator
- **Promotion**: tweets related to a promotion being run by the operator (or their associates)
- **Other**: tweets that could not be classified into any of the above groups

It is important to note that labelling tweets is not an exact science.  It was not always obvious how to categorise every tweet, and sometimes a close call had to be made.  However, this is to be expected of real world, messy data.  


### [Scrub Data](https://github.com/isobeldaley/categorising-tweets/blob/master/Step%202%20-%20Scrub%20Data.ipynb)

The next step is to 'clean' the data in order to ensure that it is in a standardized format, with no missing or erroneous values. This involved:

- **Handling missing values**: A single missing value (blank tweet) was identified.  Since this represented such a tiny fraction of the overall dataset, and it was not possible to sensibly replace the contents of a blank tweet, it was dropped.  

- **Deleting Retweets**: Retweets can be construed as duplicate data.  These were therefore be dropped from the dataset by identifying all tweets that started with the letters "RT".  

- **Calculating Sentiment using Text Blob**: An important part of our analysis involved understanding the sentiment behind each tweet.  For this task, TextBlob was used.  TextBlob is a simple Python library capable of assigning a sentiment score based on language used.  

- **Remove Stopwords**: Stopwords are very commonly used words that add little meaning to a phrase or sentence (e.g. a, the, we etc.).  As such, to enhance the performance of an NLP model, they were filtered out using the standard list of English stopwords helpd by the NLTK library.

- **Remove Links & Twitter Handles**: Both links and Twitter handles were removed, using the regular expressions library, as they add little meaning to a Tweet.  

- **Tokenize & Lemmatize**: Each tweet was broken into tokens.  Each token (word) was lemmatized.  

- **Split into labelled and unlabelled**: Finally data was split into labelled and unlabelled Tweets, and saved to separate Pandas DataFrames using Pickle.  

### [Explore Data](https://github.com/isobeldaley/categorising-tweets/blob/master/Step%203%20-%20Explore%20Data.ipynb)

To ensure an in-depth understanding of the data, the next step was Exploratory Data Analysis (EDA).  The objective of this task was to understand the distribution of tweets, by subject (e.g. network, customer service etc.) and sentiment.  Exploratory data analysis was performed using the labelled observations only.  

#### Distribution of Tweets by Network

As a starting point, the distribution of tweets across each network was checked.  

![Distribution of Tweets by Network](https://github.com/isobeldaley/categorising-tweets/blob/master/images/number_of_tweets_by_network.png)

As the chart above shows, the number of tweets varies from around 850 (EE) to around 1300 (O2).  It is important to note that every single EE tweet was labelled.   Therefore the low number of tweets attributable to EE is likely a consequence of their customers being less active on Twitter.  In contrast, the Twitter API yielded thousands of tweets for each of the remaining networks.  It was therefore possible to ensure that Threee, O2 and Vodafone each had at least 1000 labelled tweets in the dataset.  

#### Distribution of Sentiment by Network

Next, the distribution of sentiment across the networks was scrutinised.  As explained above, each tweet had been assigned a sentiment rating by TextBlob.  The sentiment rating can assume a value between -1 (negative) and +1 (positive).  

![Distribution of Sentiment by Network](https://github.com/isobeldaley/categorising-tweets/blob/master/images/distribution_of_sentiment_by_network.png)

The chart above shows that the median sentiment for all networks is 0.  However, it is important to note that the lower and upper fence for Vodafone is notably lower than any of the other networks.  This indicates a greater lack of satisfaction with Vodafone on Twitter.  This is consistent with the *Which?* rankings in which Vodafone scored a one-star rating for each of customer service, value for money and technical support.  In contrast, EE and O2 appear to perform better on this measure.

#### Distribution of Subject by Network

The distribution of tweets by subject was also reviewed.  For clarity, a radial plot was used for this task.  

![Distribution of Subject by Network](https://github.com/isobeldaley/categorising-tweets/blob/master/images/proportion_of_tweets_subject.png)

The chart above illustrates the following:

- Vodafone appears to have the greatest proportion of tweets relating to customer service.  
- Network issues are most prevalent with O2 (followed closely by Three)
- A large proportion of tweets are classified as *Other*.  This may suggest that a finer degree of categorisation is required.  This should be considered during further work on this area.

#### 20 Most Common Bigrams by Network

Finally, the 20 most frequently used bigrams (pairs of words) were identified.  These were used to a more detailed indication of the topics customers were tweeting about.  

![20 Most Common Bigrams by Network](https://github.com/isobeldaley/categorising-tweets/blob/master/images/20_most_common_bigrams.png)

It is clear for the above, that for every network, the phrase 'customer service' tops the list of most frequently used bigrams.  However, the frequency with which this phrase is used is highest for Vodafone.   Many of the other bigrams in the Vodafone chart also hint at poor customer service.  These include 'hold hour', 'still waiting' and 'trying get'.  All suggest that Vodafone are perhaps unresponsive. It seems to be a similar story for Three.  

In contrast, the bigrams for EE and O2 suggest that many questions relate to network promotions/benefits (e.g. 'amazon prime', 'swappable benefit' and 'priority ticket'.  

### [Model Data](https://github.com/isobeldaley/categorising-tweets/blob/master/Step%204%20-%20Model%20Data.ipynb)

#### Final Pre-Processing Steps

Before creating models to categorise tweets by subject, the following steps were undertaken:

- Data was split into a training and test set.  This was to ensure that the models do not suffer from overfitting, and can be generalised to new (test) data.
- Data was vectorized in two different ways.  These methods were then compared for performance when running alternative machine learning algorithms:

    - **Count Vectorization**, which simply counts the number of times a word appears in a tweet and uses this as its weight.

    - **TF-IDF Vectorization**, which evaulates how important a specified word is in a tweet.  This works by increasing the importance of a word in proportion to the number of times it appears in a particular tweet, but reducing the importance of that word by the frequency the word appears in the entire dataset of tweets.  In this way, it helps the algorithm determine which words are key to categorising a given tweet.  
    
#### Classification Models

A number of different classification models were assessed for their suitability for the task of predicting the category a given tweet relates to.  These are:

- K Nearest Neighbours
- Naive Multinomial Bayes Classifier
- Multinomial Logistic Regression
- Random Forest Classifier
- XG Boost
- Support Vector Machine (SVM)

Each model was created using the two datasets specified above:

- Dataset transformed by tf-idf vectorization
- Dataset transformed by count vectorization

#### Results

Having run each model, it was determined that the multinomial logistic regression offered the most balanced performance.  That is it exhibited reasonable accuracy, reasonable balance between precision (risk of false positives) and recall (risk of false negatives) and suffered less from overfitting in comparison to a number of the other models.

### [Interpret Results](https://github.com/isobeldaley/categorising-tweets/blob/master/Step%205%20-%20Interpret%20Model.ipynb)

To understand how this model works, a sample of 10 tweets were selected from the test data.  The model was then used to predict the subject category.  The results of this exercise are given below:

![Predicting Subject & Sentiment for Test Data](https://github.com/isobeldaley/categorising-tweets/blob/master/images/pandas_dataframe_test_tweets.png)

The performance of the model appears to be good. On 8 out of 10 occasions the category is correctly predicted.

There are two tweets that are labelled "customer service" where the user (me) has labelled them "contract" and "other". However, upon deeper reading of the tweet, it could be argued that either or both should have been labelled "customer service".

Performance of the sentiment rating is more difficult to judge. The first tweet is clearly negative, and yet has a mildly positive sentiment rating of 0.18. Meanwhile, the majority of tweets have been given a neutral rating. More work may be needed to fine tune this element of the project.

#### On-the-Pulse Measure of Customer Satisfaction

At the outset, it was stated that an objective of this project was to provide mobile networks with an on-the-pulse measure of customer satisfaction.  To understand how this would work, possible customer satisfaction measures are shown benlow, based on the 10 demo tweets considered above:

**Subject**

![On-the-pulse measure of customer satisfcation: Subject](https://github.com/isobeldaley/categorising-tweets/blob/master/images/on_the_pulse_subject.png)

**Sentiment**

![On-the-pulse measure of customer satisfaction: Sentiment](https://github.com/isobeldaley/categorising-tweets/blob/master/images/on_the_pulse_sentiment.png)

## Recommendations

Having demonstrated its potential, we recommend that this model is used in the following ways:

1. To pre-categorize tweets.  This will enable all questions/issues to be directed to the correct person/department and handled efficiently.

2. To provide a measure of the proportion of tweets relating to each subject category.  This will help mobile network operators to quickly identify and address specific (e.g. persistent problems with customer service, or poor network coverage).

3.  To provide an on-the pulse measure of customer satisfaction, by considering the distribution of sentiment ratings.

This approach will enable more effective management of customer communications via social media, rapid assessment of customer sentiment/issues.


## Future Work

There are a number of areas that would merit further investigation in the future.  These are detailed below.

### Labelling & Categorisation

For this project, we had access to 4,377 labelled tweets.  It is highly likely that model performance could be improved if the labelled dataset was significantly expanded.  Given the appropriate time and resources, it is recommended that this is done prior to deploying the model.  

Moreover, it was noted that a large proportion of tweets were categorised as "Other".  This suggests that more detailed categorisation may be needed.  

### Tweets & Replies

This model considered tweets individually.  However, tweets are often part of a longer conversation between a service agent and a customer.  It may be useful to broaden the model to consider original tweets alongside any replies.  This would help operators understand how many tweets and how much time was required to resolve an issue/question.  This is a crucial component of customer service.  

### Sentiment Analysis

Finally, it was noted that the sentiment analysis provided by TextBlob was often inaccurate.  An alternative means of modelling sentiment should be considered to improve performance of this element of the model.  

## Technical Requirements

This section will guide you through the technical requirements that will need to be fulfilled, before running this project on your machine.

Before getting started, you will need to have Python installed on your machine. We recommend using Anaconda for this purpose, as it is delivered with more than 1,500 packages/libraries pre-installed.

### Libraries
To run these projects, you will first need to install the following libraries. If you have chosen to run Python through Anaconda, many of the the libaries will already be available, and there will be no need to install them individualls.  These are highlighted below.

**Libraries available through Anaconda**

- **json**: Required to import user credentials for Twitter API
- **Pandas**: Used for data manipulation and analysis
- **Numpy**: Supports multi-dimensional matrices and arrays, provides a large number of mathematical functions
- **Matplotlib**: A 2D graphical plotting library
- **Seaborn**: Another data visualisation libary, based on Matplotlib
- **Sklearn**: Provides tools for data analysis
- **xgboost**: Enables use of the XG Boost weak learner algorithm
- **pickle**: Allows Python objects to be saved for later use, and retrieved
- **nltk**: A leading Python library for Natural Language Processing (NLP) tasks
- **re**: Used to identify and handle 'regular expressions'

**Libraries that may need to be individually installed**

- **tweepy**: Required to interact with Twitter API
- **TextBlob**: Used for sentiment analysis
- **Plotly**: An alternative data visulalisation library, allowing a greater degree of intetraction with plots

### Forking & Cloning the Repository onto your Local Machine

1. Within Github, click 'Fork'
2. Once Forked, copy the link https://github.com/[YOUR NAME]/categorising-tweets
3. Open the command line and navigate to the folder you wish to save the projec to
4. Type 'git clone https://github.com/[YOUR NAME]/categorising-tweets
5. Open a new command line window, type 'jupyter notebook'
6. A Jupyter notebook will open. Navitage to the folder in which your project is located, open the Jupyter notebook. You are ready to get started.

## Footnotes
1. Figures taken from 2019 report published by Statista: https://www.statista.com/statistics/375986/market-share-held-by-mobile-phone-operators-united-kingdom-uk/

2. Results from the Which Survey can be found here: https://www.simplybusiness.co.uk/knowledge/articles/2019/05/best-and-worst-mobile-networks-in-the-uk-2019/

3. Blog published by Twitter on research into impact of Twitter on customer service: https://blog.twitter.com/en_gb/a/en-gb/2016/customer-service-on-twitter-and-the-impact-on-brands.html
