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