<link rel="stylesheet" href="https://danitamm.github.io/assets/css/style.css">

This website summarizes several of the technical projects that I've worked on in the last few years. From a transit routing algorithm and a traffic congestion predictor to a Messenger [chatbot](https://www.messenger.com/t/102447081166159) that talks like a hockey player in an interview, projects range from practical and informative to entertaining.  They all served as an opportunity to deepen my understanding of a certain method or technology while tackling an interesting problem.

With the exception of the Camera-Based Cycling Cadence Tracker, all projects were completed before August 2020.

### **My Contact and Other Work**
- [LinkedIn](https://www.linkedin.com/in/daniel-tamming/)
- [GitHub](https://github.com/danieltamming)
- [Kaggle](https://www.kaggle.com/dtamming)
- [Medium](https://medium.com/@danieltamming)

### **Table of Contents**
1. [Research](#1-research)
2. [Machine Learning](#2-machine-learning)
3. [Computer Vision](#3-computer-vision)
4. [Data Exploration and Modelling](#4-data-exploration-and-modelling)
5. [Data Collection and Cleaning](#5-data-collection-and-cleaning)
6. [Statistics](#6-statistics)
7. [Blogging](#7-blogging)
8. [Honorable Mentions](#8-honorable-mentions)
9. [Recommended Links](#9-recommended-links)

## **1. Research**
### 1.1. Data Augmentation For Text Classification Tasks - University of Waterloo Master's Thesis
A detailed study of various synthetic data generation techniques that provide multi-percentage point accuracy increases to state-of-the-art text classification models. The data generation methods range from rules-based to deep-learning-based approaches.

#### Relevant Links
 - [GitHub repository](https://github.com/danieltamming/thesis)
 - [Thesis paper](https://uwspace.uwaterloo.ca/handle/10012/16113)

#### Libraries Used
 - PyTorch, PyTorch-Transformers, NLTK, Gensim, spaCy

## **2. Machine Learning**
### 2.1. HockeyBot - The Facebook Messenger Chatbot
#### Product
Upon receiving a message that could plausibly begin a hockey player's interview response (e.g. "Well you know"), the chatbot responds with a 5 sentence continuation of that message. Active for over 4 months, it has maintained a 100% response rate within 30 seconds of receiving a message.
#### Data
The dataset described in the [National Hockey League Interview Transcripts](#42-national-hockey-league-interview-transcripts) project.
#### Approach
A unidirectional recurrent neural network is trained on the interview transcript data. Each string of 6 contiguous words forms a training example, where the first 5 are the input and the 6th is the label. Having learned a probability distribution over the possible choices for the next word, this distribution is sampled to generate new text. This model is then deployed to a Heroku server that responds to all messages at any time. 
#### Relevant Links
 - [Facebook Messenger link](https://www.messenger.com/t/102447081166159) to interact with the bot
 - [Bot training GitHub repository](https://github.com/danitamm/hockey-bot)
 - [Bot Heroku deployment GitHub repository](https://github.com/danitamm/HockeyBotProduction)

#### Libraries Used
 - PyTorch

## **3. Computer Vision**
### 3.1 Camera-Based Cycling Cadence Tracker
#### Product
A stationary bike cadence (i.e., RPM) tracker that runs in real-time on a MacBook Pro. Current and average speed are also estimated from the RPMs and two bike-specific parameters. 
#### Approach
The algorithm proposed in [this paper](https://www.researchgate.net/profile/Ross_Cutler/publication/3813403_Real-time_periodic_motion_detection_analysis_and_applications/links/5969c77baca2728ca6803943/Real-time-periodic-motion-detection-analysis-and-applications.pdf) is implemented. The frame-by-frame pixel-level intensity difference is calculated, compressed, and reshaped to a vector. The frequency of the periodic motion is determined via Fourier analysis of the resultant vector.
#### Relevant Links
 - [GitHub repository](https://github.com/danieltamming/cycling-tracker/tree/master)
 - [Research Paper](https://www.researchgate.net/profile/Ross_Cutler/publication/3813403_Real-time_periodic_motion_detection_analysis_and_applications/links/5969c77baca2728ca6803943/Real-time-periodic-motion-detection-analysis-and-applications.pdf)
#### Libraries Used
 - SciPy, NumPy

### 3.2. Motion Activated Camera
#### Product
A security camera that begins recording video once motion is detected. Used to investigate my housemates' suspicions that a cat has been sneaking into our house in the early morning via our front door mail slot (the results are inconclusive).
#### Approach
Motion is detected by background subtraction, in which the difference between each consecutive frame is calculated and studied. The frames are converted to grayscale, blurred with a Gaussian filter, and the element-wise absolute difference between frames is computed. The threshold is applied to the result to map each value to 0 or 255. The resultant image is dilated for display purposes. When motion is detected for 5 consecutive frames, it begins saving the video feeds. These are saved until 30 seconds have passed without any movement. The videos are saved to timestamped folders, and the camera feed video is annotated with the time and the status (whether motion is detected at each given moment). 
#### Relevant Links
 - [GitHub repository](https://github.com/danieltamming/motion-detection)

#### Libraries Used
 - OpenCV, NumPy

## **4. Data Exploration and Modelling**
### 4.1. Traffic Congestion Prediction
#### Product
A program that predicts the traffic congestion at more than 10,000 intersections in 4 major American cities. It is estimated that the model would achieve results in the 25th percentile of the Kaggle leaderboard. Please see the [modelling notebook](https://www.kaggle.com/dtamming/geotab-modelling/) for an explanation of this estimate. 
#### Data
The 20th, 50th, and 80th percentiles of the following two traffic congestion metrics were used: 

1. The total time spent waiting at an intersection 
2. The distance between the center of the intersection and the place at which the vehicle first stops. 

These metrics are aggregated by city, intersection, month, hour of the day, entrance and exit orientation, and several other variables. 
#### Goal
Given a training set with all variables and a test set with only the aggregation variables, predict each of the six metric-percentile pairs on the test set.
#### Approach
Weakly predictive variables such as coordinates, entrance and exit orientation are used to engineer strongly predictive variables such as distance from city center and direction of turn. It is observed that dummy encoding of over 10,000 unique intersections would not fit into memory. This important feature is kept in the training set by using a [gradient boosted trees library](https://catboost.ai/) that has a novel [encoding method](https://catboost.ai/docs/concepts/algorithm-main-stages_cat-to-numberic.html) for high-cardinality variables. Hyperparameter tuning and early stopping are used to ensure that the model fits the data well.
#### Relevant Links
 - [Data Exploration Notebook](https://www.kaggle.com/dtamming/geotab-exploration/)
 - [Modelling Notebook](https://www.kaggle.com/dtamming/geotab-modelling/)
 - [Dataset](https://www.kaggle.com/c/bigquery-geotab-intersection-congestion/data)

#### Libraries Used
 - CatBoost, Pandas, NumPy, scikit-learn, seaborn

### 4.2. Public Transit Optimization for the Greater Toronto Area
#### The Problem
Imagine Jack and Jill, starting from different locations, have a shared destination. Jill is planning to drive, and agrees to pick Jack up at a transit stop on the condition that she does not have to go any further out of her way than strictly necessary. Which stop should Jack travel to? 
#### The Solution
This program calculates within 10 seconds the answer to the above question. It gives the correct answer as long as Jack's starting location is within range of the Toronto public transit system.
#### Data
Extracted relevant data from a set of 7 csv files that is made publicly available on the Toronto OpenData website. 
#### Approach
The data is organized into Pandas dataframes and used to create a NetworkX graph. By constructing a graphical representation of the 9155 stops and 213 routes, Google API use is limited to only once per query. 
#### Relevant Links
 - [Toronto Open Data](http://opendata.toronto.ca)
 - [Project GitHub repository](https://github.com/danitamm/mapping)

#### Libraries Used
 - NetworkX, Pandas, NumPy

### 4.3. Contrasting Hockey Players' and Coaches' Speech Patterns
#### Findings and Product
1. The average sentiment of players is more positive than that of coaches, and this difference is statistically significant. 
2. The difference between coaches' and players' average selfishness is not statistically significant. 
3. The variance in both sentiment and selfishness is greater for players than it is for coaches.
4. A model classifying speech as from a coach or from a player is trained and achieves an F1 score of 0.969 on the test set.  

#### Data
See "National Hockey League Interview Transcripts" in the Data Collection and Cleaning section of this page.
#### Approach
Sentiment is classified using the Afinn sentiment lexicon, where each word is assigned a sentiment score between -5 and +5. Selfishness is scored using the same approach but with a simple lexicon of my creation, where first person singular and first person plural pronouns are assigned scores of +1 and -1, respectively. Sentiment and selfishness are both normalized by the number of words in the interview response. The player vs coach classifier is a logistic regression model with TF-IDF features as input. 
#### Relevant Links
 - [ASAP Sports](http://www.asapsports.com/), the sports interview aggregation site
 - [Kaggle Kernel](https://www.kaggle.com/dtamming/analysis-and-modelling), published alongside my dataset

#### Libraries Used
 - AFINN, NLTK, Pandas, NumPy, seaborn, matplotlib, scikit-learn
 
## **5. Data Collection and Cleaning**
### 5.1. Web Browser Activity Tracker
#### Product
A python script that tracks daily Firefox browser use, saving the information to a json file. An accompanying program reads the saved json file and organizes it into a Pandas dataframe. It currently plots a bar graph of a use metric for each website, but can be used to analyze the data in any desired way. The following use metrics are currently supported:
1. Number of visits to each website
2. Length of time that each webpage was open

#### Data
As Firefox runs, it stores the current session's information (e.g. each open tab's title and url) in an lz4 compressed json file. This file does not contain any historical data - it only contains information pertaining to the open tabs at the time of update. The frequency at which the file is updated can vary, but it is usually every 5 seconds. 
#### Approach
The python script reads the lz4 file at a predefined frequency, 1 Hz by default. It compares the current reading to the previous reading, noting the current time and the webpages that have appeared or disappeared. In doing so it compiles a history of the time at which a user opened and closed each visited webpage. 

#### Libraries Used
 - Pandas, NumPy, seaborn, matplotlib

### 5.2. National Hockey League Interview Transcripts
#### Product 
A csv file with columns team1 and team2 (the two teams in the Stanley Cup Final), the date, the interviewee name and job type (player, coach, other), and the interview transcripts.
#### Approach
The website is formatted as sport -> year -> date -> interview page. BeautifulSoup is used to crawl across each of the bottom three levels and gather the relevant information as it goes. A data cleaning script is written to settle naming inconsistencies (such as Mike Babcock vs Coach Babcock) and determine job type. If job type or name cannot be inferred by the data alone, the script asks the user for the relevant input.
#### Relevant Links
 - [ASAP Sports](http://www.asapsports.com/), the sports interview aggregation site
 - [Project GitHub repository](https://github.com/danitamm/interview-analysis)

#### Libaries Used
 - Beautiful Soup, NLTK, Pandas

### 5.3. Medical Mask Donation Hubs
#### Product
A web scraping script that creates a csv file with rows for each entry and columns for each of the fields on a medical mask donation hub aggregation website. The fields describe the hub locations and the type of donations they are accepting. The program was completed as part of an [Upwork](https://www.upwork.com/) project proposal. 
#### Relevant Links
 - [Aggregation website](https://findthemasks.com/give.html)
 - [Project GitHub repository](https://github.com/danitamm/webscrape-mask-donation)

#### Libraries Used
 - Beautiful Soup, Selenium, Pandas, NumPy

## **6. Statistics**
### 6.1. Inference on the Boston Housing Dataset Using Linear Regression
#### Findings
1. One can say with 95% confidence that for a fixed percentage of the population that is lower status, an increase of 1 in the average number of rooms per house results in an increase of between 5.44% and 13.20% in the median house value.
2. One can say with 95% confidence that for a fixed average number of rooms per house, an increase of 1 percent in the percentage of the population that is lower status results in a decease of betweeen 0.47% and 0.56% in the median house value.
3. Given the effects due to the percentage of the population that is lower status, the average number of rooms has a statistically significant effect on the log of the median house value.
4. Given the effects due to the average number of rooms, the logarithm of the percentage of the population that is lower status has a statistically significant effect on the log of the median house value.

#### Data
The Boston Housing Dataset contains medians, means, and proportions of various attributes of 506 different segments of the city of Boston. The median house value is used as the target variable.
#### Approach
Linear regression's assumptions are validated, allowing the enumerated confidence intervals to be created and hypothesis tests to be conducted. 
#### Relevant Links
 - [GitHub repository](https://github.com/danitamm/boston-housing-linear-regression)

#### Libraries Used
 - SciPy, Pandas, NumPy, seaborn, matplotlib

## **7. Blogging**
Medium posts explaining elements of certain projects. 
### 7.1. NHL Player Chatbot
 - Selected by Medium curators for distribution in the site's AI and Machine Learning sections
 - Published on Analytics Vidhya's Medium page
 - [Medium link](https://medium.com/analytics-vidhya/nhl-player-chatbot-5c882e330fb7)

### 7.2. A Quantitative Study of NHL Interviews
 - Published on Analytics Vidhya's Medium page
 - [Medium link](https://medium.com/analytics-vidhya/nhl-player-chatbot-5c882e330fb7)

## **8. Honorable Mentions**
Data wrangling, feature engineering, and model fine-tuning with two classic Kaggle datasets.
### 8.1. Kaggle House Prices
 - [notebook Kaggle link](https://www.kaggle.com/dtamming/house-prices)

### 8.2. Titanic Classification
 - [notebook Kaggle link](https://www.kaggle.com/dtamming/titanic-classification)

## **9. Recommended Links**
 - Alan Perlis' ["Epigrams in Programming"](https://www.cs.yale.edu/homes/perlis-alan/quotes.html)
 - [Datasets Subreddit](https://www.reddit.com/r/datasets/)
 - [Our World In Data](https://ourworldindata.org/)
