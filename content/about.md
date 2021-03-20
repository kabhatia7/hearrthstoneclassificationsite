---
slug: about
title: About the project
---




*Background*: 

Hearthstone is a massively popular online collectible card game developed by Blizzard Entertainment. Players select a hero from one of ten classes. All classes have unique cards and abilities, known as hero powers, which help define class archetypes. Each player uses a deck of cards from their collection with the end goal being to reduce the opponent's health to zero. Blizzard releases expansions of additional cards every four months to increase the variety in the metagame. 
In my STAT 431 course I developed an R package using the Hearthstone API to pull data quickly and efficiently. That package was very much just a good way to bring data to perform further analysis, however, it did not employ any statistical computation or statistical learning methods. Card games like hearthstone lend themselves very well to classification. Based on x,y, or z factors this card can be a part of a,b, or c classes or because this deck has this combination of cards it is this archetype. This thought is what led me to building my senior project.

*Real World Application*:

The game has established a strong esports system and tournament circuit where thousands of players compete every week to try to qualify for the Master’s Tour system which hosts tournaments with hundreds of thousands in prize pools. To qualify players compete in open cups by bringing a lineup with 3 decks. As previously mentioned, there are thousands of competitors per week. So over the course of a month-long qualifying series there are approximately 200,000 decks used. Right now few members of the community have built websites around analysis for these events many of them build manually classify each of the decks based on a few cards and general understanding of the meta decks in order to conduct this analysis. It is very difficult and time consuming whenever there is a meta shift or a new expansion to create these case statements and then having to go through and manually change it throughout the ever shifting meta. So I decided I would attempt to build an application and a statistical model that would allow someone to quickly determine what archetype a deck is just by copying and pasting a deck code taken from the hearthstone client.

*Data Collection*: 

I collected data from two sources. First, I web-scraped approximately 1,000 decks from the largest user sourced decks for hearthstone, hsreplay.net. This site offers downloadable software that gives players the ability to track their winrate with the variety of decks they play. HSReplay then records various statistics for each deck with a significant sample size, determines what the deck is and manually lumps it under a group of decks in the same archetype. Since these decks were already classified with an incredibly low error rate by HSReplay I used this as my training data to build my model. For my test data, I thought it would be the most fitting to apply my models to what I wanted the real world application of this project to be. So I was able to get my hands on competitor lineups for the month of January 2021 for all open cup qualifiers for the Master’s Tour Series. This contained 20,000 unique deck codes, and while obviously there is a lot of overlap in the archetypes between those decks it is the ideal utilization of what I had in mind for this project. All of my data was processed through the Hearrthstone API package I had made last year, where the primary function used was converting a deck code into a data frame containing all 30 cards in the deck and information about each card. The next step was getting all the data in a format that would allow me to use it for modeling with methods such as KNN & Decision Trees. 

*Model Testing & Selection*: 

Naturally this type of problem lends itself to a classification learning method to determine the archetype of the deck. I decided to try three types of models to very similar levels of success: 
Decision Trees
K-Nearest Neighbors
At first I thought that I would just use one model to fit all 10 of the classes in hearthstone and in this case the decision tree performed the best having a cross-validated accuracy in the range of 92%-95% depending on the split. However, since there would sometimes the models would classify decks to be in a completely different class, so instead of training the model for all the decks I trained 10 different models (one for each class) using only decks for that particular class and evaluated which tuning parameters/models would work the best from there. We used the R package set tidymodels to do all of our modeling. This allowed us to test and hypertune the models we had set prior. I found based on the hyper tuning that different classes responded better to different tuning parameters and different models. This is caused by the fact that different classes have a lot of overlap in the cards used so it makes more sense to use a method like knn whereas some classes have very clear dillienations between the different archetypes in this case I found decision trees to be the most useful. Below are all the classes with the models used in the application, with all the tuning parameters as well as the cross validated accuracy & std error from my analysis. 

|Class      | Model   | Parameters | Model Accuracy | Std Err |
|_________|_______|__________|______________|_______|
|Demon Hunter      | Decision Tree   | Depth = 30, Minimum per node = 2, Cost complexity =  01.000000e-10 | 93.3% | 0.001284666 |
|Druid      | KNN   | K = 7 | 94.4% | 0.0154139 |
|Hunter      | Decision Tree | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |
|Class      | Model   | Parameters | Model Accuracy | Std Err |

