# League of Legends - Red Side or Blue Side?

This League of Legends project was developed as the final project for DSC 80 at UC San Diego. This project involved stepping through the various stages of the Data Science Life Cycle; the steps involved in reaching the final conclusions of the project included exploratory data analysis, data cleaning, assessment for missingness, hypothesis testing, the creation of a baseline model and an improved final model, and a fairness analysis for the model. The overall goal of the project was to determine whether either one of the sides in League of Legends has a significant advantage over the other side. With sufficient evidence, we may be able to answer the question: "Can I blame what side I am on for losing?"

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

## Hypothesis Testing

### Hypothesis Testing

For our hypothesis test, we assessed whether the Blue Side gets more kills, on average, than the Red Side. Like we mentioned above, knowing this information would be extremely crucial in determining whether the Blue Side actually has an advantage over the Red Side.

**Null Hypothesis:** The Blue Side gets the same number of kills, on average, as the Red Side.

**Alternative Hypothesis:** The Blue side gets more kills, on average, than the Red side.

Test Statistic: Difference in Group Means of Number of Kills (Blue - Red)

We decided to choose the difference in group means as our test statistic, as it provides us with a good idea as to how different the number of kills between each side is. A positive difference would mean the Blue Side gets more kills than the Red Side, while a negative difference would mean the Red Side gets more kills than the Blue Side.

Significance Level: 0.01

We wanted to make the conclusion we arrived at was backed by solid evidence, so we chose a fairly small significance level.

After performing our permutation test, we got a p-value of 0.0. An empirical distribution of our test statistics, along with the observed statistic, is shown below.

<iframe
  src="assets/chart-10.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since our p-value is less than the significance level of 0.01, we reject the null hypothesis. This suggests that The Blue side gets more kills, on average, than the Red side, which backs up the theory we've been analyzing.

## Framing a Prediction Problem

### Problem Identification

Above, we've been analyzing whether the Blue Side has an advantage over the Red Side, and whether this advantage leads to a higher probability of winning for the Blue Side. These "advantages" come in the form of certain features, like the ability for the Blue Side to get the "firstherald" and "firstdragon" more often than the Red Side. Will these features, in addition to other features like the number of kills or the match's length allow us to predict which side a player/team was on? This is the basis of our model below.

Since we are predicting which side a certain player/team is on, this is a classification problem. Specifically, this is a binary classification problem, as there are only two sides a player/team can be on: blue or red. We chose to predict "side", as, according to our exploration above, there seem to be certain trends for each side. If our machine learning model is able to capture these trends, its performance would be amazing.

As far as our evaluation metric goes, we chose accuracy. We chose accuracy, as it give us a general idea of the model's ability to correctly predict the "side" column. Although it is the most basic metric, it works in this situation, as our data is balanced; there are equal number of players/teams on the Red Side and the Blue Side. Although our model will be trained to maximize accuracy, we still score it on precison, recall, and F1-score aswell.

Since our time of prediciton occurs at the end of a game, these are the variables we have access to:
- result
- kills
- deaths
- assists
- firstdragon
- dragons
- firstherald
- heralds
- firstbaron
- barons
- gamelength

We will split our dataset into a training and a testing set to eliminate the presence of overfitting. We will use 77% of our dataset as training data, and the remaining 23% of our data as testing data.

Here's a very small subset of our training set:

|   result |   kills |   deaths |   assists |   firstdragon |   dragons |   firstherald |   heralds |   firstbaron |   barons |   gamelength |
|---------:|--------:|---------:|----------:|--------------:|----------:|--------------:|----------:|-------------:|---------:|-------------:|
|        6 |      32 |        0 |        68 |             1 |         4 |             0 |         0 |            1 |        2 |         9054 |
|        0 |      12 |       34 |        14 |             0 |         0 |             1 |         1 |            0 |        0 |        11940 |
|        6 |      50 |       16 |        64 |             1 |         3 |             1 |         2 |            1 |        2 |         7788 |
|        0 |      18 |       36 |        52 |             1 |         3 |             1 |         2 |            0 |        0 |        14772 |
|        6 |      40 |       20 |        78 |             1 |         4 |             1 |         2 |            0 |        0 |        12360 |


## Baseline Model

For our baseline model, we normalize the "kills", "deaths", "assists", "dragons", "heralds", "barons", and "gamelength" columns by using StandardScaler(), one hot encode the "firstdragon", "firstherald", and "firstbaron" columns by using OneHotEncoder(), and binarize the "result" column using Binarizer(). We will use a default decision tree classifier.

**To Predict:** We will be predicting which side a player is on, so for this we will binaraize the "side" column: 1 being Blue and 0 being Red. 

### Features

For our baseline model's features, we will use the following, grouped by the transformations performed.

---

**Feature(s):** kills, deaths, assists, dragons, heralds, barons, and gamelength

**Type of Variable:** It is impossible for these features to take on decimal values, and these variables are sortable, which makes them discrete quantitative variables.

**Transformation:** We use the StandardScaler() transformer.

**Reason:** Because we have discrete data, it's always a good idea to scale it to normalize the data.

---

**Feature(s):** firstdragon, first herald, firstbaron

**Type of Variable:** These columns simply tell us if a team got the first dragon in the game, the first herald in the game, or the first baron in the game; therefore, these columns are nominal categorical variables.

**Transformation:** For these variables, we use one hot encoding.

**Reason:** We chose to one hot encode instead of keeping the variables binary because we do not want to assume that taking these objectives first is "better".

---

**Feature(s):** result

**Type of Variable:** This column tells us if a team has won or lost the respective game.

**Transformation:** For this variable, we binarize it using Binarizer(), with 1 being a win and 0 being a loss.

**Reason:** The values of the "result" column are 0 for a loss or 6 for a win since we're accounting for players and teams; the 6 essentially acts as a 1, but in this case, we decided to make it binary (convert the 6 toa  1) to make it more cohesive.

---

### Results

We then utilized a decision tree classifier, and placed everything into a pipeline. We then evaulated the model using accuracy, precision, recall, and F1-score, but of course, we focused on accuracy, as that's the main evaluation metric we chose.

Our results were as follows:

| Metric           | Value |
|------------------|-------|
| Accuracy         | 0.54  |
| Precision (Blue) | 0.55  |
| Recall (Blue)    | 0.54  |
| F1-Score (Blue)  | 0.55  |

These scores look pretty bad. The accuracy of 0.54 signifies to us that the model is slightly better than flipping a coin on unseen data. Although it does not tell us if it underfits or overfits, we can see that there *is* something to learn about these features. Overall, we concluded that the model was bad because it just barely beats out a coin flip.

## Final Model

### Feature Engineering

For our final model, we developed many new features (and modified some) in hopes of finding meaningful relationships; we did this in hopes of improving our model's performance.

The plethora of **new** features we create include:
- gamelength_mins: We transform gamelength into minutes from seconds.
- Dragons Per Second: This new feature captures the number of dragons each player/team obtained per second in the game.
- Heralds Per Second: This new feature captures the number of heralds each player/team obtained per second in the game.
- Barons Per Second: This new feature captures the number of barons each player/team obtained per second in the game.
- K/D: This metric represents the kill-death ratio for the player/team.
- Objectives Per Second: This new feature captures the number of objectives each player/team obtained per second in the game.
- Objectives: This feature represents the total number of objectives each player/team attained during the match.
- numFirsts: This feature represents the total number of objectives the player/team got first.
- freqEncodedDragon: This new feature represents the proportion of 0s and 1s for "firstdragon", instead of simply 0s and 1s.
- freqEncodedBaron: This new feature represents the proportion of 0s and 1s for "firstbaron", instead of simply 0s and 1s.
- freqEncodedHerald: This new feature represents the proportion of 0s and 1s for "firstherald", instead of simply 0s and 1s.

For our final model's features, we used the following, grouped by the category of feature.

---

**Feature(s):** Dragons Per Second, Heralds Per Second, Barons Per Second

**Why:** We believe these three features increased our accuracy because instead of looking at the amount of objectives that a certain team took, we instead look at how often a team took said objectives. This is more important because in the game you may be putting off getting an objective to do other things. This should result in more meaningful features than simply dragons, heralds, and barons.

---

**Feature(s):** K/D

**Why:** We wanted to combine the kills and deaths columns to create a kill-death ratio column because a team could have lots of kills, either because they actually had a lot of kills or because they were playing in an extended game where there were a lot of kills. Simply put, more kills do not mean a team is in a better position or not. To mitigate this, we divide by the number of deaths a team had, which should result in a better indicator of a team's position.

---

**Feature(s):** dragons, heralds, barons

**Why:** Instead of our initial idea of standard scaling these columns, we instead keep these columns as-is. We believe this improved our accuracy because it pretty much turns these features into more of an ordinal column rather than a discrete column.

---

**Feature(s):** numFirsts

**Why:** We created this feature by summing up each of the "first" columns row-wise. This resulted in a better indicator of how many "firsts" a team got. We wanted to show that each of the "firsts" is equally as important as the other.

---

**Feature(s):** firstdragon, firstbaron, firstherald

**Why:** Instead of keeping these binary, we decided to turn them into a different encoding called "Frequency Encoding". Frequency encoding is a way to encode categorical features, where instead of having a binary encoding, you encode the features with the proportion they encompass of the entire column. This is to have more of a "weight" to these features. One may ask: "Is it just .5 for all columns?" This is only true if there was a dragon/baron/herald in every game, which is not always the case. In fact, for almost all of our splits, the proportions are around 60/40.

### Modeling Algorithm/Process

We started off by using another decision tree classifier, but with the newly-engineered features described above. This resulted in evaluation metrics that were about the same as our baseline model's.

So, we decided to use a different type of classifier to try to get better results. We used a RandromForestClassifier model. After running this model, our results were definitely better than our previous models, but we wanted to see if we could improve upon the model even more.

So, we decided to use a RandomForestClassifier() again, but this time with a RanomizedSearchCV to find optimal parameters. This process is described in the following section.

### Hyperparameter Search

To choose hyperparameters, we used _RandomizedSearchCV_ instead of _GridSearchCV_. We preferred this method because it allowed us to search a range of values instead of picking specific values. Our procedure for parameters was as follows:

1. Choose a hyperparameter to search.
2. Do some research on the expected range our optimal parameter would be in. We looked at several examples online of successful random forest classification on sites such as Kaggle.
3. Perform random grid search with a high iteration count to ensure that we are searching a wide range of different hyperparameters. (Note that RandomizedSearchCV also uses k-fold cross-validation; we used a value of 5, as our dataset is relatively small, and we wanted to ensure we had enough data for our model to train on.)
4. For the model, we picked the "best" model based on accuracy on the validation sets.

The hyperparameters we picked to tune in our random forest were:

- classifier__n_estimators = range(1, 500): We chose this range because it covers a single decision tree, as well as a very large number of decision trees. While having more decision trees is usually good, it comes at a large cost of computation time, which would not be optimal.

- max_depth = range(2, 30): We chose this range of depths because our decision trees might have a low depth, which might cause the model to underfit, or a very large depth, which usually means the decision tree overfits our training data. When conducting research online, we saw that many decision tree models do not go over 30.

- min_samples_split = range(2, 10): This parameter basically tells us the minimum samples needed to perform a split. This is another way for us to control the depth of our decision trees. Higher values mean we need a minimum of 10 samples to perform a split of our data. A smaller value allows trees to capture fine details in our data at the cost of potentially overfitting. A larger value means we can capture more generalized trends at the cost of potentially underfitting. This is helpful for our data because these different values essentially act as a grouping of our data to our class; we want to make sure we stay as general as possible.

- min_samples_leaf = range(1, 20): This parameter is the number of samples needed for a node to be a leaf. This is very similar to the previous parameter and helps specifically with noise in our data, which there is a lot of due to some games being outliers; for instance, these games might have a large number of kills or might end very quickly.

- criterion = 'gini' or 'entropy': This method searches through both "gini" and "entropy". We chose between the two because "gini" is the default and, according to our research, "entropy" should pretty much give the same results, but we passed it in to capture any possible incremental differences.

This resulted in these optimal hyperparameters:

| Parameter                     | Value |
|-------------------------------|-------|
| classifier__criterion         | gini  |
| classifier__max_depth         | 5     |
| classifier__min_samples_leaf  | 4     |
| classifier__min_samples_split | 8     |
| classifier__n_estimators      | 430   |

### Results

These parameters gave us testing scores of:

| Metric           | Value |
|------------------|-------|
| Accuracy         | 0.62  |
| Precision (Blue) | 0.64  |
| Recall (Blue)    | 0.59  |
| F1-Score (Blue)  | 0.61  |

These results resulted in an approximate 5% - 10% increase in all metrics; we'll take it! Although we do not have a perfect model, the increase in our results showed that the feature engineering and hyperparameter tuning was effective. Overall, we would say that our process was successful and perhaps having a more rigorous search in both features and hyperparameters could have yielded better results.

## Fairness Analysis

### Fairness Analysis

Now, we want to assess whether the final model performs the same for two different groups. We can perform a fairness analysis to test this.

Specifically, we will be determining whether the model performs the same for players/teams that won a game and for players/teams that lost a game. We will be utilizing accuracy as our evaluation metric. The hypotheses for our test are:

**Null Hypothesis:** Our model is fair. Its accuracy for players/teams that won is the same as the accuracy for players/teams that lost.

**Alternative Hypothesis:** Our model is unfair. Its accuracy for players/teams that won is NOT the same as the accuracy for players/teams that lost.

Test Statistic: Difference in Accuracy (Won - Lost)

Significance Level: 0.01

After performing our permutation test, we got a p-value of 0.48. An empirical distribution of our test statistics, along with the observed statistic, is shown below.

<iframe
  src="assets/chart-11.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since our p-value is greater than the significance level of 0.01, we fail to reject the null hypothesis. This suggests that the model is fair, and doesn't exhibit any bias towards certain groups.

## Final Thoughts

At first, we were really bummed we were not able to get a high accuracy percentage for our model. We thought changing the type of model, tuning the hyperparameters, and even engineering new features would help us find that perfect combination to determine which side a player/team was on. But, if we recontextualize our model and instead look at what we could not achieve, we can extract more meaning from our findings. The lack of accuracy could mean that League of Legends is a (in terms of what side a team is on) somewhat balanced game. Now, I know for many League of Legends players that this statement may be shocking, but the fact that we were unable to find features significant enough to tell us which side a player/team was on may signify that either Red Side or Blue Side have no significant advantage over the other side.
