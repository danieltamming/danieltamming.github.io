## Data Exploration and Modeling
### Public Transit Travel Optimization for Greater Toronto Area
#### Outcome 
Finds the optimal meetup location for a pedestrian and driver that have differing starting locations but the same target location. The command line interface provides the optimal meeting location anywhere within range of Toronto public transit. 
#### Data
A set of csv files that includes relevant data regarding the 9155 stops and 213 routes is made publicly available on the Toronto OpenData website. 
#### Approach
By constructing a networkx graph using publicly Toronto Transit Commission (TTC) data, we call the Google Maps API only once per use. 

### Interview Transcripts - Contrasting and Classifying Hockey Players vs Coaches
#### Outcome
An analysis of NHL Stanley Cup Final interview transcripts. Players vs coaches and successful teams vs unsucessful teams are contrasted. A simple model classifying speech as from a coach or from a player is trained and achieves an F1 score of 0.969 on the test set. 
#### Data
See "National Hockey League Interview Transcripts" in the Data Collection and Cleaning section of this page.
#### Approach
Speech is studied along two axes: sentiment and selfishness. Sentiment is classified using the Afinn sentiment lexicon, where each word is assigned a sentiment score between -5 and +5. Selfishness is scored using the same approach but with a simple lexicon of my creation, where first person singular and first person plural pronouns are assigned scores of +1 and -1, respectively. The player vs coach classifier is a logistic regression model with TF-IDF features as input. 

### Examining The Assumptions of Linear Regression


## Machine Learning - Model Training and Deployment
### HockeyBot, The Facebook Messenger Chatbot
#### Outcome
Upon receiving a message that could begin a hockey player's interview response (e.g. "Well you know"), it responds with a 5 setence continuation of that message. Active for over 4 months, it has maintained a 100% response rate within 30 seconds of receiving a message.
#### Data
See "National Hockey League Interview Transcripts" in the Data Collection and Cleaning section of this page.
#### Approach
A unidirectional recurrent neural network is trained on the interview transcript data. The training set is comprised of each 6 contiguous words/symbols, where the first 5 are the input example and the 6th is the label. Having learned a probability distribution over the possible choices for the next word/token, this distribution is sampled to generate new text. This model is then deployed to a Heroku server that responsds to all messages at any time. 

## Data Collection and Cleaning
### National Hockey League Interview Transcripts
#### Outcome 
A csv file with columns team1 and team2 (the two teams in the Stanley Cup Final), the date, the interviewee name and job type (player, coach, other), and the interview transcripts.
#### Approach
The website is formatted as sport -> year -> date -> interview page. BeautifulSoup is used to crawl across each of the bottom three levels and gather the relevant information as it goes. A script is used to settle naming inconsistencies (such as Mike Babcock vs Coach Babcock) and determine job type. If job type or name cannot be inferred by the data alone, the script uses user input to solve it. 
### Medical Mask Donation Hubs
#### Outcome
A csv file with all the fields on the  (state, city, location, address, instructions, accepting, Open packages?)

## Python Programming Projects
### Web Browser Activity Tracker
### Motion Detection

## Master's Research (In Progress)

## Blog Posts
Medium posts explaining elements of the above projects.
### NHL Player Chatbot
### A Quantitative Study of NHL Interviews

## Honorable Mentions
### Kaggle House Prices
### Titanic Classification

## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/danitamm/danitamm.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/danitamm/danitamm.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
