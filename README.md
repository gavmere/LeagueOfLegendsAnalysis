# League of Legends - Red Side or Blue Side?

This League of Legends project was developed as the final project for DSC 80 at UC San Diego. This project involved stepping through the various stages of the Data Science Life Cycle; the steps involved in reaching the final conclusions of the project included exploratory data analysis, data cleaning, assessment for missingness, hypothesis testing, the creation of a baseline model and an improved final model, and a fairness analysis for the model. The overall goal of the project was to determine whether either one of the sides in League of Legends has a significant advantage over the other side.

## Introduction

### What is League of Legends?

League of Legends, also known as LoL, is a multiplayer online battle arena video game developed by Riot Games; the game launched in 2009, and has over 150 million registered players as of May 2024. The game has been especially dominant in the e-sports industry, which is the basis for the dataset utilized in this project.

### The Dataset

The professional dataset utilized in this project was developed by Oracle's Elixir. The dataset is composed of information taken from various e-sports matches throughout 2022; this information includes gameplay statistics and outcomes representing player behavior and team dynamics.

The dataset contains 148,992 rows. From our understanding, this is how the dataset is structured: Each game takes up 12 rows. 5 of the rows are for players on the Blue Side, 5 of the rows are for players on the Red Side, 1 row is summary statistics for the Blue Side team, and 1 row is summary statistics for the Red Side team.

### Our Question

A commonly agreed-upon theory amongst many League of Legends players is that the Blue Side tends to have a significant advantage over the Red Side due to easier access to Rift Herald and Baron, which make it easier to attain objectives. Red Side typically has easier access to Dragons, but Dragons aren't as helpful when it comes to attaining objectives. As a result, the Blue Side typically sees a higher win rate. We will be exploring this theory throughout the project.

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
  
- league: This string column represents the specific league tournament in which the corresponding match took place.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The first step of our data cleaning process involved removing the irrelevant columns. Out of the 131 columns in the original dataset, we only kept 15 of the columns, which are described in more detail above.

Second, we decided to only keep the "team" rows of the dataset. Our question above only concerns comparisons between the two teams, so the player rows would be irrelevant.

Finally, the columns "result", "firstdragon", "firstherald", and "firstbaron" should be of type Boolean, not int or float. These columns were therefore converted appropriately.

After performing the two steps above, here's what our dataset looks like:

| gameid                | side   |   result |   kills |   deaths |   assists |   firstdragon |   dragons |   firstherald |   heralds |   firstbaron |   barons | position   |   gamelength | league   |
|:----------------------|:-------|---------:|--------:|---------:|----------:|--------------:|----------:|--------------:|----------:|-------------:|---------:|:-----------|-------------:|:---------|
| ESPORTSTMNT01_2690210 | Blue   |        0 |       9 |       19 |        19 |             0 |         1 |             1 |         2 |            0 |        0 | team       |         1713 | LCKC     |
| ESPORTSTMNT01_2690210 | Red    |        1 |      19 |        9 |        62 |             1 |         3 |             0 |         0 |            0 |        0 | team       |         1713 | LCKC     |
| ESPORTSTMNT01_2690219 | Blue   |        0 |       3 |       16 |         7 |             0 |         1 |             1 |         1 |            0 |        0 | team       |         2114 | LCKC     |
| ESPORTSTMNT01_2690219 | Red    |        1 |      16 |        3 |        39 |             1 |         4 |             0 |         1 |            1 |        2 | team       |         2114 | LCKC     |
| 8401-8401_game_1      | Blue   |        1 |      13 |        6 |        35 |           nan |         2 |           nan |       nan |          nan |        1 | team       |         1365 | LPL      |

*Note: The DataFrame above is not the full DataFrame. Only the first 5 rows are shown for reference.

### Univariate Analysis

To find some interesting trends for singular variables, we performed a univariate analysis on our data by generating a few plots.

<iframe
  src="assets/chart-1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It looks like the distribution of kills is mostly normal, but slightly skewed to the right, meaning the data is well-behaved. The distribution seems to reflect real life, as most players hover around a similar level, whereas a select few are very good, and are thus able to get more kills than normal.

<iframe
  src="assets/chart-2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of deaths is the exact same as the distribution of kills! This is expected, as the kills committed by one team contribute to the deaths of the other team. We might be able to ignore one of these two variables, as it would be repetitive to utilize both.

<iframe
  src="assets/chart-3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It looks like the distribution of assists is similar to the distributions of kills and deaths; namely, it looks mostly normal, but with a slight right skew. This suggests there are a few teams with a large number of assists, while the remaining teams have a normal number of assists.

<iframe
  src="assets/chart-4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of game length looks almost perfectly normal, which makes sense; naturally, we would expect most games to last about 30 minutes, while fewer games last fewer than 30 minutes and more than 30 minutes.

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

It's said that the Blue Side tends to have a higher win rate due to easier access to Rift Herald and Baron, which make it easier to attain objectives. The Red Side typically has easier access to Dragons, but Dragons aren't as helpful when it comes to attaining objectives.

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
reason: We chose to make it a binary feature beacuse 
DSFJHSAKFDAJKHFSDAFHSDAKFHKSDFJKASDFHKDALFLSADLJFSADJFSA FIX THIS

Using these variables we use a default descion tree classifier and place it into a pipeline. We then evaulate the model using accuracy, precision, recall, and f1-score.

Our results are as follows:
Accuracy: 0.54
Precision (blue): 0.55
Recall (blue): 0.54
F1-score (blue): 0.54

These scores look pretty bad. The accuracy of .54 signifies to me that the model is slightly better then flipping a coin on unseen data. Although it does not tell me if it underfits or overfits, we can see that there *is* something to learn about these features. The same goes for precision, recall, and f1-score. Overall, we determine the model as bad because we are just barely beating out a coin flip. 

##Final Model
For our final model we really need to look deep into our data. In pursuit of this, we not only added features, but dropped many as well.

**Features Engineering:**
The final features we seattled on are:

Assits, gamelength, Dragons Per second, geralds Per second, Barons Per second, K/D, Dragons, Heralds, Barons, numFirsts, result

**The ones we added**

We added the features Dragons Per second, heralds Per second, Barons Per second, K/D, Objectives, numFirsts, firstDragon, firstBaron, firstHerald 

**Feature(s):**Dragons Per second, geralds Per second, Barons Per second

**Why:** We believe these features increased our accuracy because instead of looking at the amount that a certain team took, we instead look out how often a team took said objective. This is more important because in the game you may be putting of getting an objectives to do other things. This should result in high values as high dragon/baron/herald frequency

**Feature(s):**K/D

**Why:**We wanted to combine the kills and deaths columns to create a kill-death ratio column because a team could have lots of kills either because they actually had a lot of kills or it was an extended game where there were a lot of kills. That is to say that more kills does not mean a team is in a better position or not. To mitigate this, we divide by the amount of deaths a team had which should result in a better indicator of a teams position.

**Feature(s):**Dragons, Heralds, Barons

**Why:**: Instead of our intial idea of standard scaling these columns, we instead keep these columns as is. We believe this improved our accuracy because it adds pretty much turns these features as more of an ordinal column rather than a discrete one.

**Feature(s):**numFirsts

**Why:** We created this feature by summing up each of the "first" columns row wise. This should result in a better indicator of how many "firsts" a team gotten in total while also reducing dimensionality.

**The ones we changed**

**Feature(s):**Dragons, Heralds, Barons

**Why:**: Instead of our intial idea of standard scaling these columns, we instead keep these columns as is. We believe this improved our accuracy because it adds pretty much turns these features as more of an ordinal column rather than a discrete one.


**Feature(s):**firstDragon, firstBaron, firstHerald 

**Why:** Instead of keeping these binary, we decieded to turn it to a different encoding called "Frequency Encoding". Frequency encoding is a way to encode categorical features where instead of having a binary encoding, you encode them with the proportion they are of the data. This is to have more of a "weight" to these features. One may ask "Is it just .5 for all columns?", this is only true if there was a dragon/baron/herald in every game, which is not always the case. In fact, for almost all of our splits the proportions are around 60/40.


##Fairness analysis

TODO

##Digging deeper

At first, we were really bummed we were not able to get a high accuracy percent for our model. We thought changing models, hyperparameters and even features would help us find that perfect combination to find out what side a team was on. But if we recontextualize our model and instead look at what we could not achieve we could extract more meaning from our findings. The lack of accuracy could mean that **League of Legends is a (in terms of what side a team is on) _somewhat_ balanced** Now, I know for many League of Legends players this statement may be shocking, but the fact that we were unable to find features that significant tell us which side a team was on, including weather they won or lost, may signify that either Red or Blue have no significant advantage over the other.

##Conclusion

