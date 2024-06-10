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
