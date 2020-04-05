<link rel="stylesheet" href="https://danitamm.github.io/assets/css/style.css">

# Daniel Tamming's Data Science Portfolio
This website summarizes several of the technical projects that I've worked on in the last couple of years. Projects range from practical (a transit routing algorithm, a traffic congestion predictor) to entertaining (a Messenger [chatbot](https://www.messenger.com/t/102447081166159) that talks like a hockey player in a post-game interview), and everything in between. 

Table of Contents:
1. [Research](#research)
2. [Machine Learning](#machine-learning)
3. [Data Exploration and Modeling](#data-exploration-and-modeling)
4. [Data Collection and Cleaning](#data-collection-and-cleaning)
5. [Statistics](#statistics)
6. [Python Programing Projects](#python-programming-projects)
7. [Blogging](#blogging)
8. [Honorable Mentions](#honorable-mentions)
9. [Recommended Links](#recommended-links)

Contact:
- [LinkedIn](https://www.linkedin.com/in/daniel-tamming/)
- [GitHub](https://github.com/danitamm)
- [Kaggle](https://www.kaggle.com/dtamming)
- [Medium](https://medium.com/@danieltamming)

# Research
## Artificial Data Generation for Natural Language Processing Tasks - University of Waterloo Master's Thesis (In Progress)
A detailed study of various synthetic data generation techniques that provide multi-percentage point accuracy increases to state-of-the-art text classification models. The data generation methods range from rules-based to deep-learning-based approaches. 

### Relevant Links (Coming Soon)
 - GitHub repo will be made public upon completion

### Libraries Used
 - pytorch, nltk, spacy, huggingface transformers, gensim

# Machine Learning
## HockeyBot, The Facebook Messenger Chatbot
### Product
Upon receiving a message that could plausibly begin a hockey player's interview response (e.g. "Well you know"), the chatbot responds with a 5 sentence continuation of that message. Active for over 4 months, it has maintained a 100% response rate within 30 seconds of receiving a message.
### Data
See "National Hockey League Interview Transcripts" in the Data Collection and Cleaning section of this page.
### Approach
A unidirectional recurrent neural network is trained on the interview transcript data. Each string of 6 contiguous words forms a training example, where the first 5 are the input and the 6th is the label. Having learned a probability distribution over the possible choices for the next word, this distribution is sampled to generate new text. This model is then deployed to a Heroku server that responds to all messages at any time. 
### Relevant Links
 - [Facebook Messenger link](https://www.messenger.com/t/102447081166159) to interact with the bot
 - [Bot training GitHub repository](https://github.com/danitamm/hockey-bot)
 - [Bot Heroku deployment GitHub repository](https://github.com/danitamm/HockeyBotProduction)

### Libraries Used
 - pytorch

# Data Exploration and Modeling
## Traffic Congestion Prediction (Geotab Challenge)
### Product
A program that predicts the traffic congestion at more than 10,000 intersections in 4 major American cities. It is estimated that the model would achieve results in the 25th percentile of the Kaggle leaderboard. Please see the [modelling notebook](https://www.kaggle.com/dtamming/geotab-modelling/) for an explanation of this estimate. 
### Data
The 20th, 50th, and 80th percentiles of two traffic congestion metrics: 

1. The total time spent waiting at an intersection 
2. The distance between the center of the intersection and the place at which the vehicle first stops. 

These metrics are aggregated by city, intersection, month, hour of the day, entrance and exit orientation, and several other variables. 
### Goal
Given a training set with all variables and a test set with only the aggregation variables, predict each of the six metric-percentile pairs on the test set.
### Approach
Weakly predictive numerical variables such as hour and coordinates are used to engineer strongly predictive variables such as "time to rush hour" and "distance from city center". It is observed that dummy encoding of over 10,000 unique intersections would not fit into memory. This important feature is kept in the training set by using a [gradient boosted trees library](https://catboost.ai/) that has a novel [encoding method](https://catboost.ai/docs/concepts/algorithm-main-stages_cat-to-numberic.html) for high-cardinality variables. Hyperparameter tuning and early stopping are used to ensure that the model fits the data well.
### Relevant Links
 - [Data Exploration Notebook](https://www.kaggle.com/dtamming/geotab-exploration/)
 - [Modelling Notebook](https://www.kaggle.com/dtamming/geotab-modelling/)
 - [Dataset](https://www.kaggle.com/c/bigquery-geotab-intersection-congestion/data)

### Libraries Used
 - catboost, scikit-learn, seaborn, pandas, numpy


## Public Transit Travel Optimization For The Greater Toronto Area
### Product 
A program that calculates within 10 seconds the best place for a pedestrian to meet a driver. The command line interface finds the optimal meeting location for any valid query (anywhere within range of Toronto public transit).
### Data
Extracted relevant data from a set of 7 csv files that is made publicly available on the Toronto OpenData website. 
### Approach
By constructing a graphical representation of the 9155 stops and 213 routes, we restrict our Google API use to only once per query. Relevant data gathered from the Toronto Open Data website and is organized into 2 csv files that are saved to memory and loaded as Pandas dataframes. 
### Relevant Links
 - [Toronto Open Data](http://opendata.toronto.ca)
 - [Project GitHub repository](https://github.com/danitamm/mapping)

### Libraries Used
 - networkx, pandas, numpy

## Contrasting Hockey Players' and Coaches' Speech Patterns
### Findings and Product
1. The difference between coaches' and players' average sentiment is not statistically significant. 
2. The difference between coaches' and players' average selfishness is not statistically significant. 
3. The variance in both sentiment and selfishness is greater for players than it is for coaches.
4. A model classifying speech as from a coach or from a player is trained and achieves an F1 score of 0.969 on the test set.  

### Data
See "National Hockey League Interview Transcripts" in the Data Collection and Cleaning section of this page.
### Approach
Sentiment is classified using the Afinn sentiment lexicon, where each word is assigned a sentiment score between -5 and +5. Selfishness is scored using the same approach but with a simple lexicon of my creation, where first person singular and first person plural pronouns are assigned scores of +1 and -1, respectively. Sentiment and selfishness are both normalized by the number of words in the interview response. The player vs coach classifier is a logistic regression model with TF-IDF features as input. 
### Relevant Links
 - [ASAP Sports](http://www.asapsports.com/), the sports interview aggregation site
 - [Kaggle Kernel](https://www.kaggle.com/dtamming/starter-kernel-nhl-interviews), published alongside my dataset

### Libraries Used
 - scikit-learn, afinn, nltk, seaborn, matplotlib, numpy, pandas
 
# Data Collection and Cleaning
## National Hockey League Interview Transcripts
### Product 
A csv file with columns team1 and team2 (the two teams in the Stanley Cup Final), the date, the interviewee name and job type (player, coach, other), and the interview transcripts.
### Approach
The website is formatted as sport -> year -> date -> interview page. BeautifulSoup is used to crawl across each of the bottom three levels and gather the relevant information as it goes. A data cleaning script is written to settle naming inconsistencies (such as Mike Babcock vs Coach Babcock) and determine job type. If job type or name cannot be inferred by the data alone, the script asks the user for the relevant input.
### Relevant Links
 - [ASAP Sports](http://www.asapsports.com/), the sports interview aggregation site
 - [Project GitHub repository](https://github.com/danitamm/interview-analysis)

### Libaries Used
 - beautifulsoup, pandas, nltk

## Medical Mask Donation Hubs
### Product
A web scraping script that creates a csv file with rows for each entry and columns for each of the fields on a medical mask donation hub aggreagation website. The fields describe the hub locations and the type of donations they are accepting. The program was completed as part of an [Upwork](https://www.upwork.com/) project proposal. 
### Relevant Links
 - [Aggregation website](https://findthemasks.com/give.html)
 - [Project GitHub repository](https://github.com/danitamm/webscrape-mask-donation)

### Libraries Used
 - beautifulsoup, selenium, numpy, pandas

# Statistics
## Inference On The Boston Housing Dataset Using Linear Regression
### Findings
1. We can say with 95% confident that for a fixed percentage of the population that is lower status, an increase of 1 in the average number of rooms per house effects between a 5.44 and 13.20 percent increase in the median house value.
2. We can say with 95% confident that for a fixed average number of rooms per house, an increase of 1 percent in the percentage of the population that is lower status effects between a 0.47 and 0.56 percentage decrease in the median house value.
3. Even given the effects due to the percentage of the population that is lower status, the average number of rooms has a statistically significant effect on the log of the median house value.
4. Even given the effects due to the average number of rooms, the logarithm of the percentage of the population that is lower status has a statistically significant effect on the log of the median house value.

### Data
The Boston Housing dataset contains medians, means, and proportions of various attributes of 506 different segments of the city of Boston. We use the median house value as the target variable.
### Approach
After validating linear regression's assumptions we construct confidence intervals and test hypotheses regarding the predictive variables' relationships with the target. 
### Relevant Links
 - [GitHub repository](https://github.com/danitamm/boston-housing-linear-regression)

### Libraries Used
 - scipy, seaborn, matplotlib, pandas, numpy

# Python Programming Projects
## Web Browser Activity Tracker
### Product
A command line program that tracks daily Firefox browser use, saving the information to a json file. An accompanying program reads the saved json files and organizes it into a Pandas dataframe. It currently plots a bar graph of the use metric per website, but can be used to analyze the data in any desired way. 
### Data
As Firefox runs it stores the current session's information a lz4 compressed json file. The file is updated roughly every 5 seconds, allowing the program to closely track internet use.
### Libraries Used
 - numpy, pandas, seaborn, matplotlib

## Motion Activated Camera
### Product
A security camera that begins recording video once motion is detected. Used to investigate my housemates' suspicions that a cat has been sneaking into our house in the early morning via our front door mail slot (the results are inconclusive).
### Approach
Motion is detected by background subtraction, in which the difference between each consecutive frame is calculated and studied. The frames are converted to grayscale, blurred with a Gaussian filter, and the element-wise absolute difference between frames is computed. The threshold is applied to the result to map each value to 0 or 255. The resultant image is dilated for display purposes. When motion is detected for 5 consecutive frames, it begins saving the video feeds. These are saved until 30 seconds have passed without any movement. The videos are saved to timestamped folders, and the camera feed video is annotated with the time and the status (whether motion is detected at each given moment). 
### Libraries Used
 - opencv, numpy

# Blogging
Medium posts explaining elements of the above projects. 
## NHL Player Chatbot
 - Selected by Medium curators for distribution in the site's AI and Machine Learning sections
 - Published on Analytics Vidhya's Medium page
 - [Medium link](https://medium.com/analytics-vidhya/nhl-player-chatbot-5c882e330fb7)

## A Quantitative Study of NHL Interviews
 - Published on Analytics Vidhya's Medium page
 - [Medium link](https://medium.com/analytics-vidhya/nhl-player-chatbot-5c882e330fb7)

# Honorable Mentions
Data wrangling, feature engineering, and model fine-tuning with two classic Kaggle datasets. As of March 30, 2020, the resultant models achieved results in the following percentiles of the competitions: 
## Kaggle House Prices
 - 21st percentile (first of two submissions)
 - [notebook Kaggle link](https://www.kaggle.com/dtamming/house-prices)

## Titanic Classification
 - 18th percentile (first and only submission)
 - [notebook Kaggle link](https://www.kaggle.com/dtamming/titanic-classification)

# Recommended Links
If you'll forgive the digression, here are some things that I find interesting and/or thought-provoking: 
 - Alan Perlis' ["Epigrams in Programming"](https://www.cs.yale.edu/homes/perlis-alan/quotes.html). Numbers 7 and 16 are my personal favorites.
 - [FiveThirtyEight](https://fivethirtyeight.com/) and its Founder, Nate Silver, [in conversation](https://www.youtube.com/watch?time_continue=45&v=_tMA7F4FA94&feature=emb_logo) with the polymath academic Tyler Cowen
 - [Datasets Subreddit](https://www.reddit.com/r/datasets/)
 - [Our World In Data](https://ourworldindata.org/)
