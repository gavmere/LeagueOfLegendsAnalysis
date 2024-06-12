# League of Legends - Red Side or Blue Side?

This League of Legends project was developed as the final project for DSC 80 at UC San Diego. This project involved stepping through the various stages of the Data Science Life Cycle; the steps involved in reaching the final conclusions of the project included exploratory data analysis, data cleaning, assessment for missingness, hypothesis testing, the creation of a baseline model and an improved final model, and a fairness analysis for the model. The overall goal of the project was to determine whether either one of the sides in League of Legends has a significant advantage over the other side.

## Introduction

### What is League of Legends?

League of Legends, also known as LoL, is a multiplayer online battle arena video game developed by Riot Games; the game launched in 2009, and has over 150 million registered players as of May 2024. The game has been especially dominant in the e-sports industry, which is the basis for the dataset utilized in this project.

### The Dataset

The professional dataset utilized in this project was developed by Oracle's Elixir. The dataset is composed of information taken from various e-sports matches throughout 2022; this information includes gameplay statistics and outcomes representing player behavior and team dynamics.

The dataset contains 148,992 rows. From our understanding, this is how the dataset is structured: Each game takes up 12 rows. 5 of the rows are for players on the Blue Side, 5 of the rows are for players on the Red Side, 1 row is summary statistics for the Blue Side team, and 1 row is summary statistics for the Red Side team.

### Our Question

A commonly agreed-upon theory amongst many League of Legends players is that the Blue Side tends to have a significant advantage over the Red Side due to easier access to Rift Herald and Rift Baron, which make it easier to attain objectives. Red Side typically has easier access to Dragons, but Dragons aren't as helpful when it comes to attaining objectives. As a result, the Blue Side typically sees a higher win rate. We will be exploring this theory throughout the project.

**Question: Does the Blue Side have a significant advantage over Red Side in League of Legends?**

After attempting to answer this question, important insights can be gained into the accuracy of this theory. This may serve to benefit players' decision-making processes in matches, elevate the e-sports entertainment experience, and enhance the overall gameplay experience.

### Relevant Columns

To address our question, we narrowed down the dataset to only include these relevant columns:

- gameid: This string column represents a unique identifier for each individual match in the dataset. This is useful when trying to distinguish between different matches.
  
- side: This string column represents the affilitation of the player or team. The only two values in this column are "Red" and "Blue", representing the Red Side and Blue Side respectively.
  
- result: This Boolean column represents whether the player or team won or lost the corresponding match. A "1" represents a win, and a "0" represents a loss.
  
- kills: This integer column represents the number of enemy champions a player or team eliminated during the respective match.
  
- deaths: This integer column represents the number of times the player or team was eliminated by enemy champions during the respective match.
  
- assists: This integer column represents the number of times a player or team was credited with assisting with a kill. In other words, the player or team didn't make the kill themselves, but was credited for helping another player or team eliminate an enemy champion.
  
- firstdragon: This Boolean column represents whether a player or team gets the first dragon or assists with getting the first dragon. A "1" means they got the first dragon or assisted with getting the first dragon, while a "0" means they didn't get the first dragon or didn't assist with getting the first dragon.
  
- dragons: This float column represents the number of dragons the player or team attained during the match.
  
- firstherald: This Boolean column represents whether a player or team gets the first herald or assists with getting the first herald. A "1" means they got the first herald or assisted with getting the first herald, while a "0" means they didn't get the first herald or didn't assist with getting the first herald.
  
- heralds: This float column represents the number of heralds the player or team attained during the match.
  
- firstbaron: This Boolean column represents whether a player or team gets the first baron or assists with getting the first baron. A "1" means they got the first baron or assisted with getting the first baron, while a "0" means they didn't get the first baron or didn't assist with getting the first baron.
  
- barons: This float column represents the number of barons the player or team attained during the match.
  
- position: This string column represents the position of the particular player the row corresponds to. The possible positions include "top", "jungle", "mid", "bot", and "support". If the row corresponds to a team, the position column simply contains the word "Team".
  
- gamelength: This integer column represents the length of the match, in seconds.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The first step of our data cleaning process involved removing the irrelevant columns. Out of the 131 columns in the original dataset, we only kept 14 of the columns, which are described in more detail above. These 14 columns are all of the columns that were essential for our analysis; including additional columns would've led to unnecessary clutter and space.

Second, we decided to only keep the "team" rows of the dataset. Our question above only concerns comparisons between the two teams, so the player rows would be irrelevant. Keeping only the team rows also kept our visualizations simple, understandable, and uncluttered.

Finally, the columns "result", "firstdragon", "firstherald", and "firstbaron" should've been of type Boolean, not int or float. These columns were therefore converted appropriately. It's important to note that although these columns were converted to Boolean, any missing values were preserved for the section to follow.

After performing the steps above, here's what our dataset looks like:

| gameid                | side   | result   |   kills |   deaths |   assists |   firstdragon |   dragons |   firstherald |   heralds |   firstbaron |   barons | position   |   gamelength |
|:----------------------|:-------|:---------|--------:|---------:|----------:|--------------:|----------:|--------------:|----------:|-------------:|---------:|:-----------|-------------:|
| ESPORTSTMNT01_2690210 | Blue   | False    |       9 |       19 |        19 |             0 |         1 |             1 |         2 |            0 |        0 | team       |         1713 |
| ESPORTSTMNT01_2690210 | Red    | True     |      19 |        9 |        62 |             1 |         3 |             0 |         0 |            0 |        0 | team       |         1713 |
| ESPORTSTMNT01_2690219 | Blue   | False    |       3 |       16 |         7 |             0 |         1 |             1 |         1 |            0 |        0 | team       |         2114 |
| ESPORTSTMNT01_2690219 | Red    | True     |      16 |        3 |        39 |             1 |         4 |             0 |         1 |            1 |        2 | team       |         2114 |
| 8401-8401_game_1      | Blue   | True     |      13 |        6 |        35 |           nan |         2 |           nan |       nan |          nan |        1 | team       |         1365 |

*Note: The DataFrame above is not the full DataFrame. Only the first 5 rows are shown for reference.

### Univariate Analysis

To find some interesting trends for singular variables, we performed a univariate analysis on our data by generating a few plots.g

First, let's look at the distribution of the "kills" variable.

<iframe
  src="assets/chart-1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It looks like the distribution of kills is mostly normal, but slightly skewed to the right, meaning the data is well-behaved. The distribution seems to reflect real life, as most players hover around a similar level, whereas a select few are very good, and are thus able to get more kills than normal.

What about the distribution of deaths?

<iframe
  src="assets/chart-2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of deaths is the exact same as the distribution of kills! This is expected, as the kills committed by one team contribute to the deaths of the other team. We might be able to ignore one of these two variables later in the project, as it would be repetitive to utilize both.

How about assists?

<iframe
  src="assets/chart-3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It looks like the distribution of assists is similar to the distributions of kills and deaths; namely, it looks mostly normal, but with a slight right skew. This suggests there are a few teams with a large number of assists, while the remaining teams have a normal number of assists.

And, finally, what about game length?

<iframe
  src="assets/chart-4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of game length looks almost perfectly normal, which makes sense; naturally, we would expect most games to last about 30 minutes, while fewer games last fewer than 30 minutes and more than 30 minutes.

After looking at the distributions of the variables above, we were able to safely conclude that they all could be used as factors in the prediction process.

### Bivariate Analysis

In addition to the univariate analysis above, we performed a bivariate analysis of our data to find any notable relationships between multiple variables.

Generally, people say the Blue Side wins more than the Red Side. Let's see if that's true! We can group by the "side" column, and take the mean of the "result" column to get the proportion of wins for each side.

<iframe
  src="assets/chart-5.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It looks like people are right! According to the 2022 data, the Blue Side won more matches than the Red Side, on average.

What about game length vs. kills? Naturally, we would expect the number of kills to increase as the game length increases. Let's see if that's true.

<iframe
  src="assets/chart-6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It looks like that isn't necessarily true! There is no clear linear relationship; instead, most of the data is concentrated in one area in a "blob" shape, and there are quite a few data points surrounding the "blob". Even some games of shorter length seem to have a lot of kills, and some games of longer length seem to have very few kills.

It's said that the Blue Side tends to have a higher win rate due to easier access to Rift Herald and Rift Baron, which make it easier to attain objectives. The Red Side typically has easier access to Dragons, but Dragons aren't as helpful when it comes to attaining objectives.

<iframe
  src="assets/chart-7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Indeed, it looks like the above is true! Looking at the bar chart, the Blue side has a higher frequency of firstherald and firstbaron, while the Red side has a higher frequency of firstdragon.

### Interesting Aggregates

As mentioned above, let's look at a table comparing each side's counts for firstdragon, firstherald, and firstbaron, but with the number of wins for each team this time.

| side   |   firstdragon |   firstherald |   firstbaron |
|:-------|--------------:|--------------:|-------------:|
| Blue   |          4305 |          6120 |         5070 |
| Red    |          6290 |          4474 |         4970 |

| side   |   result |
|:-------|---------:|
| Blue   |     6509 |
| Red    |     5905 |

The above aggregated tables reinforce the conclusion we arrived at with the charts above; the Blue Side has more wins due to better access to firstherald and firstbaron, while the Red Side has less wins, but better acccess to firstdragon.

We can also find any patterns associated with the result of a match and the average number of kills, deaths, assists, etc.

| Win?   |    kills |   deaths |   assists |   gamelength |
|:-------|---------:|---------:|----------:|-------------:|
| False  |  9.36785 | 19.6123  |   20.0719 |      1895.94 |
| True   | 19.5965  |  9.40672 |   44.5698 |      1896.17 |

It looks like teams that won have a higher number of kills and a lower number of deaths, on average, than teams that lost. Also, the teams that won have more than twice as many assists, on average, than teams that lost. The game lengths, though, are about the same, on average, regardless of the result.

## Assessment of Missingness

### NMAR Analysis

Looking at all the columns in the dataset, it looks like the columns "ban1", "ban2", "ban3", "ban4", and "ban5" are NMAR, or not missing at random. In these columns, the fields are only populated if an indiviudal player decides to ban someone, and are NaN otherwise. Thus, the missingness of these values depends on the actual missing values themselves, making the column NMAR. However, we can't conclude this by simply looking at the columns. A more thorough inspection is necessary. It also looks like these 5 columns would be considered MAR, or missing at random, if the "all_banned" column was included, as the missing values in "ban1", "ban2", "ban3", "ban4", and "ban5" would depend on "all_banned". The "all_banned" column contains values of 0 and 1; 1 if all players banned an enemy champion, and 0 if at least one player didn't ban an enemy champion. Again, we can't conclude this by simply looking at the columns, and a more thorough inspection is needed.

### Missingness Dependency

To test for missingness dependency in our dataset, we picked the "firstherald" column to analyze further. We checked whether "firstherald" depended on "kills" and "assists". The details of our two permutation tests are below.

**Test 1:** Does the missingness of "firstherald" depend on "kills"?

**Null Hypothesis:** The distribution of "kills" when "firstherald" is missing is the same as the distribution of "kills" when "firstherald" is not missing.

**Alternative Hypothesis:** The distribution of "kills" when "firstherald" is missing is NOT the same as the distribution of "kills" when "firstherald" is not missing.

Test Statistic: Absolute Difference in Group Means

Significance Level: 0.01

After performing our permutation test, we got a p-value of 0.0. An empirical distribution of our test statistics, along with the observed statistic, is shown below.

<iframe
  src="assets/chart-8.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since our p-value is less than the significance level of 0.01, we reject the null hypothesis. This suggests that the distribution of "kills" when "firstherald" is missing is NOT the same as the distribution of "kills" when "firstherald" is not missing, meaning the missingness of "firstherald" depends on "kills".

**Test 2:** Does the missingness of "firstherald" depend on "assists"?

**Null Hypothesis:** The distribution of "assists" when "firstherald" is missing is the same as the distribution of "assists" when "firstherald" is not missing.

**Alternative Hypothesis:** The distribution of "assists" when "firstherald" is missing is NOT the same as the distribution of "assists" when "firstherald" is not missing.

Test Statistic: Absolute Difference in Group Means

Significance Level: 0.01

After performing our permutation test, we got a p-value of 0.023. An empirical distribution of our test statistics, along with the observed statistic, is shown below.

<iframe
  src="assets/chart-9.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since our p-value is greater than the significance level of 0.01, we fail to reject the null hypothesis. This suggests that the distribution of "assists" when "firstherald" is missing is the same as the distribution of "assists" when "firstherald" is not missing, meaning the missingness of "firstherald" does not depend on "assists".

##Baseline Model

for our base line we decide to use the following colums we will try to predict which side a team was on based on the chosen features

**To Predict:**
We will be predicting which side a player is on, so for this we will binaraize the sides 1 being Blue and 0 being Red 

**Features**
For our baseline model features we will use the following (grouped by transformation performed)
**Feature(s):** kills, deaths, assists, dragons, heralds, barons, and gamelength
**Type of Variable:** Since these columns are all intgers, including gamelength, it is impossible for these features to take on decimal values and they are sortable, which makes them Discrete
**Transformation:** We will be using the standardScaler transformer 
**Reason:** Because we have discrete data it's always a good idea to scale it to normalize the data

**Feature(s):** firstdragon, first herald, firstbaron
Type of Variable: These columns simply tells us if a team got the first baron in their game, therefore these columns are categorical
Transformation: For these variables we use one hot encoding
reason: We chose to OHE instead of keeping it binary because we do not want to assume that taking these objectives first is "better".

Feature(s): result
Type of Variable: This column tells us if a team has win or lost their game
Transformation: For these variables we Binarize it, with 1 being a win and 0 being a loss
reason: We chose to make it a binary feature beacuse if we are guessing off a team the values of our result would be 0 or 6 and although it would be around the same as the 6 acts as a one, we decided to make it binary to make it more cohesive.

Using these variables we use a default descion tree classifier and place it into a pipeline. We then evaulate the model using accuracy, precision, recall, and f1-score.

Our results are as follows:
| Metric           | Value |
|------------------|-------|
| Accuracy         | 0.54  |
| Precision (blue) | 0.55  |
| Recall (blue)    | 0.54  |
| F1-score (blue)  | 0.54  |

These scores look pretty bad. The accuracy of .54 signifies to me that the model is slightly better then flipping a coin on unseen data. Although it does not tell me if it underfits or overfits as percision and recall are pretty much the same, we can see that there *is* something to learn about these features. Overall, we determine the model is bad because we are just barely beating out a coin flip. 

### Final Model
For our final model we really need to look deep into our data. In pursuit of this, we not only added features, but dropped/changed many as well.

**Feature Engineering:**
The final features we seattled on are:

Assits, gamelength, Dragons Per second, geralds Per second, Barons Per second, K/D, Dragons, Heralds, Barons, numFirsts, result

#### **The ones we added**

We added the features Dragons Per second, heralds Per second, Barons Per second, K/D, Objectives, numFirsts, firstDragon, firstBaron, firstHerald 

**Feature(s):**Dragons Per second, geralds Per second, Barons Per second

**Why:** We believe these features increased our accuracy because instead of looking at the amount that a certain team took, we instead look out how often a team took said objective. This is more important because in the game you may be putting of getting an objectives to do other things. This should result in high values as high dragon/baron/herald frequency

**Feature(s):**K/D

**Why:**We wanted to combine the kills and deaths columns to create a kill-death ratio column because a team could have lots of kills either because they actually had a lot of kills or it was an extended game where there were a lot of kills. That is to say that more kills does not mean a team is in a better position or not. To mitigate this, we divide by the amount of deaths a team had which should result in a better indicator of a teams position.

**Feature(s):**Dragons, Heralds, Barons

**Why:**: Instead of our intial idea of standard scaling these columns, we instead keep these columns as is. We believe this improved our accuracy because it adds pretty much turns these features as more of an ordinal column rather than a discrete one.

**Feature(s):**numFirsts

**Why:** We created this feature by summing up each of the "first" columns row wise. This should result in a better indicator of how many "firsts" a team gotten. We want to show that each of the "Firsts" are equally as important as the other. 

#### **The ones we changed**

**Feature(s):**Dragons, Heralds, Barons

**Why:**: Instead of our intial idea of standard scaling these columns, we instead keep these columns as is. We believe this improved our accuracy because it adds pretty much turns these features as more of an ordinal column rather than a discrete one.


**Feature(s):**firstDragon, firstBaron, firstHerald 

**Why:** Instead of keeping these binary, we decieded to turn it to a different encoding called "Frequency Encoding". Frequency encoding is a way to encode categorical features where instead of having a binary encoding, you encode them with the proportion they are of the data. This is to have more of a "weight" to these features. One may ask "Is it just .5 for all columns?", this is only true if there was a dragon/baron/herald in every game, which is not always the case. In fact, for almost all of our splits the proportions are around 60/40.

### Hyperparameter search

To chose hyperparameters, we used _RandomizedSearchCV_ instead of gridsearching. We prefer this method because it allows us to search a range of values instead of picking specific values. Our procedure of parameters were as follows:

1. Chose a hyperparameter to search.
2. Do some research on the expect range our optimal parameter would be in we looked at several examples online of successful random forest classification on sites such as kaggle.
3. Perform random gridsearch with a high iteration count to ensure that we are searching a wide range of different hyperparameters. (Note that RandomizedSearchCV also uses k-fold cross validation, we used a value of 5 as our dataset is relatively small, we wanted to ensure we had enough data for our model to train on)
6. For the model we picked the "best" model based on accuracy on the validation sets

The hyperparameters we picked to tune in our random forest were
- classifier__n_estimators = range(1, 500)

we chose this range because it covers having just a single descion tree as well as having a very large amount of descion trees. Having different amounts of descion trees while having more is usually good, it comes at a large cost of computation time, which would not be optimal.

- max_depth = range(2, 30)

We chose this range of depths because we can have our descion trees have a low depth, which might cause the model to underfit or a vary large depth, which usually means it overfits our training data. Online we saw that many descion tree models do not go over 30.

- min_samples_split = range(2, 10)

This parameter basically tells us the minimum samples needed to perform a split. This is another way for us to control the depth of our decision trees. Higher values means we need a minimum of 10 samples to perform a split of our data. A smaller values allows trees to capture fine details in our data at the cost of potentially over fitting. A larger value means it can capture more generalized trends at the cost of potentially under fitting. This is helpful for our data because these different values essentially acts as a grouping of our data to our class, we want to make sure we stay as general as possible.

- min_samples_leaf = range(1, 20)

This parameter is the number of samples needed for a node to be a leaf. This is very similar to the previous parameter and helps specifically with noise in our data, which there is alot due to some games being outliers having large kills or ending very quickly. 

- criterion = either 'gini' or 'entropy'

It also searches through both gini and entropy. We chose between the two because gini is default and according to our reseach this technically should result in the same results no matter what, but to make sure we threw it in to randomly search.


This resulted in these hyper parameters

| Parameter                     | Value |
|-------------------------------|-------|
| classifier__criterion         | gini  |
| classifier__max_depth         | 7     |
| classifier__min_samples_leaf  | 2     |
| classifier__min_samples_split | 7     |
| classifier__n_estimators      | 415   |

These parameters gave us testing scores of:

| Metric           | Value |
|------------------|-------|
| Accuracy         | 0.64  |
| Precision (Blue) | 0.65  |
| Recall (Blue)    | 0.61  |
| F1-Score (Blue)  | 0.63  |

These results resulted in a ~8-10% increase in all metrics, we take those! Although we do not have a perfect model, the increase in our results should that the feature engineering and hyperparameter tuning was effective. Overall, I would say that our process was successful and perhaps having a more rigours search in both features and hyperparameters could yield better results.

### Fairness analysis

TODO

### Digging deeper

At first, we were really bummed we were not able to get a high accuracy percent for our model. We thought changing models, hyperparameters and even features would help us find that perfect combination to find out what side a team was on. But if we recontextualize our model and instead look at what we could not achieve we could extract more meaning from our findings. The lack of accuracy could mean that **League of Legends is a (in terms of what side a team is on) _somewhat_ balanced** Now, I know for many League of Legends players this statement may be shocking, but the fact that we were unable to find features that significant tell us which side a team was on, including weather they won or lost, may signify that either Red or Blue have no significant advantage over the other.

