# Improving Customer Service through Tweet Classification

## Abstract

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

This will be achieved through a model which, enables tweets to be categorised according to:

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

### Model Data

### Interpret Results


## Summary of Results


## Future Work


### Labelling

### Tweets & Replies

## Technical Requirements

## Footnotes
1. Figures taken from 2019 report published by Statista: https://www.statista.com/statistics/375986/market-share-held-by-mobile-phone-operators-united-kingdom-uk/

2. Results from the Which Survey can be found here: https://www.simplybusiness.co.uk/knowledge/articles/2019/05/best-and-worst-mobile-networks-in-the-uk-2019/

3.  https://blog.twitter.com/en_gb/a/en-gb/2016/customer-service-on-twitter-and-the-impact-on-brands.html
