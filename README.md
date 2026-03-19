# League of Legends First Dragon Significance

---

League of Legends First Dragon Significance is an exploratory data science project, conducted to discover how significant securing the first dragon of the game is on the outcome of the game and player stats. This project uses publically available data about professional League of Legends gameplay in 2020, and goes in depth with data visualizations, hypothesis testing, and predictive model creation. 

Authors: Benson Zhu, Ryan Luutuyen 

---

# Introduction

League of Legends is the most widely acclaimed multiplayer online battle arena (MOBA) game of all time, and for good reason. The game features a dynamically changing gameplay landscape, giving millions of players a different gameplay experience season by season. With this in mind, it is no wonder why League of Legends has made waves in both the e-sports scene and casual gaming alike. With so many e-sports leagues holding games, there are large datasets online for recording relevant information, such as the sets provided by Oracle's Elixir, from which we will be using the 2020 set. 

Within the gameplay loop of League of Legends, there are key objectives that provide a leading edge over the opposing team, and one of them is the dragons. Located near the center of the map, the dragon is a highly contested objective that teams can secure early on in the match, netting a permanent team-wide buff and a generous sum of gold. Thus, it is crucial for teams to fight for dragon kills throughout the game as to not allow the other team to snowball their advantage. However, given that each dragon buff is permanent and stacking, this begs the question: **How significant is the first dragon kill on the outcome of a League of Legends game?** We will be using various methods of EDA and modeling to help answer this question, rooted in real-world data. 

### Relevant Variables 

Due to the depth of a League of Legends game, there are many variables that are irrelevant to our project, so we keep the following columns:

'gameid'
'gamelength'
'result'
'dragons'
'firstdragon'
'golddiffat10' and 'golddiffat15' 
'xpdiffat10' and 'xpdiffat15'
'csdiffat10' and 'csdiffat15'
'killsat10' and 'killsat15'
'deathsat10' and 'deathsat15'
'firstblood'
'firsttower'
'firstherald'
'league'
'year'
'patch'
'firstbaron'

# Data Cleaning and Exploratory Data Analysis 

In the datasets provided by Oracle's Elixir, each professional game is represented by 12 individual rows, with 10 for each player and 2 for a summary of each team's cumulative statistics. With 117,012 rows available, that is nearly 10,000 games to analyze! We kept rows that were the teams' summary statistics, dropped rows that had missing completely at random values (less than 1 percent), and added an additional columnm, 'length_min', representing the length of the game in minutes rounded to the nearest hundredth. 

Below is the head of our cleaned dataset, with some columns removed for clarity:

| gameid                |   gamelength |   result |   dragons |   golddiffat10 |   length_min |
|:----------------------|-------------:|---------:|----------:|---------------:|-------------:|
| ESPORTSTMNT03/1241318 |         2220 |        1 |         2 |            117 |        37    |
| ESPORTSTMNT03/1241318 |         2220 |        0 |         3 |           -117 |        37    |
| ESPORTSTMNT03/1241322 |         2227 |        0 |         1 |          -2014 |        37.12 |
| ESPORTSTMNT03/1241322 |         2227 |        1 |         4 |           2014 |        37.12 |
| ESPORTSTMNT03/1241324 |         1711 |        1 |         4 |            682 |        28.52 |