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

`gameid`
`gamelength`
`result`
`dragons`
`firstdragon`
`golddiffat10` and `golddiffat15`
`xpdiffat10` and `xpdiffat15`
`csdiffat10` and `csdiffat15`
`killsat10` and `killsat15`
`deathsat10` and `deathsat15`
`firstblood`
`firsttower`
`firstherald`
`league`
`year`
`patch`
`firstbaron`

# Data Cleaning and Exploratory Data Analysis 

In the datasets provided by Oracle's Elixir, each professional game is represented by 12 individual rows, with 10 for each player and 2 for a summary of each team's cumulative statistics. With 117,012 rows available, that is nearly 10,000 games to analyze! We kept rows that were the teams' summary statistics, dropped rows that had missing completely at random values (less than 1 percent), and added an additional columnm, `length_min`, representing the length of the game in minutes rounded to the nearest hundredth. This dataset will be used for both hypothesis testing and training a prediction model. 

Below is the head of our cleaned dataset, with some columns removed for visual clarity:

| gameid                |   gamelength |   result |   dragons |   golddiffat10 |   length_min |
|:----------------------|-------------:|---------:|----------:|---------------:|-------------:|
| ESPORTSTMNT03/1241318 |         2220 |        1 |         2 |            117 |        37    |
| ESPORTSTMNT03/1241318 |         2220 |        0 |         3 |           -117 |        37    |
| ESPORTSTMNT03/1241322 |         2227 |        0 |         1 |          -2014 |        37.12 |
| ESPORTSTMNT03/1241322 |         2227 |        1 |         4 |           2014 |        37.12 |
| ESPORTSTMNT03/1241324 |         1711 |        1 |         4 |            682 |        28.52 |

## Univariate and Bivariate Analyses 

To gain a deeper understanding of the data, we created univariate and bivariate analyses to visualize what we were working with. 

### Univariate Visualizations

We first did a histogram visualizing the distribution of `gamelength`, which shows that most games last around 30 minutes, with the data being skewed to the right, showing that games rarely go over 40 minutes.

<iframe
  src="assets/univariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We then did a histogram visualizing the distribution of `dragons`, or how many dragons were killed throughout a game by a team. This visualization showed a normal distribution centered around 3, so on average both teams killed 3 dragons each, or 6 total. Dragons spawn every 5 minutes, which implies that dragons were killed very quickly after spawning for 6 dragons to be killed in a typical match lasting around 30 minutes. 

<iframe
  src="assets/univariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Visualization 

After having a look at how variables stand alone, we then created bivariate visualizations to look at how variables affect one another. We first looked at how often the team who ended up winning the match had secured the first dragon when compared to the team that had lost, and created the barplot shown below. 

<iframe
  src="assets/bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Data Aggregation 

To examine the data numerically, we aggregated by `firstdragon` and `result`, then took the mean to observe the average values for columns we are interested in for hypothesis testing. With this data, it was clear that the team that took the first dragon had much stronger statistics at the 10 minute mark. 

|(first dragon, result)|   ('dragons', 'mean') |   ('golddiffat10', 'mean') |   ('csdiffat10', 'mean') |   ('killsat10', 'mean') |   ('deathsat10', 'mean') |
|:---------------------|----------------------:|---------------------------:|-------------------------:|------------------------:|-------------------------:|
| (0, 0)               |               1.01915 |                   -775.06  |                -11.2673  |                 1.56834 |                  2.61142 |
| (0, 1)               |               2.79557 |                    548.887 |                  5.85781 |                 2.34766 |                  1.76979 |
| (1, 0)               |               2.17843 |                   -549.324 |                 -5.87523 |                 1.75905 |                  2.35348 |
| (1, 1)               |               3.66489 |                    775.398 |                 11.2801  |                 2.60443 |                  1.57589 |

# Assessment of Missingness 

## MNAR Analysis

After carefully examining the whole dataset, we found that `pentakills` is missing not at random. The values being missing likely implied that there were no pentakills in the game, since it is a rare event in a game, where the entire team of 5 is killed by one player. Looking closer, we found there were only 284 pentakills throughout the 10000 games in 2020, or 3% of games.

## Missingness Dependency

In this part, we are going to test if the missingness of `firstbaron` column depends on the current patch of the game. We chose a significance level of 0.05 and using Total Variance Distance (TVD) as the test statistic. We perform the permutation test on `firstbaron` and `patch`, and found that the missingness of `firstbaron` is independent of `patch`.

Null Hypothesis: The distribution of patch when firstbaron is missing is the same as when firstbaron is not missing.

Alternative Hypothesis: The distribution of patch when firstbaron is missing is not the same as when firstbaron is not missing.

The chart below shows the distribution of `patch` depending on whether `firstbaron` is missing or not:

|   patch |   firstbaron_missing = False |   firstbaron_missing = True |
|--------:|-----------------------------:|----------------------------:|
|    9.24 |                   0.0010469  |                  0.00102137 |
|   10.01 |                   0.0258585  |                  0.0255955  |
|   10.02 |                   0.0585218  |                  0.0570944  |
|   10.03 |                   0.0596734  |                  0.0582179  |
|   10.04 |                   0.0648032  |                  0.0638354  |
|   10.05 |                   0.0561139  |                  0.0559709  |
|   10.06 |                   0.0672111  |                  0.0705969  |
|   10.07 |                   0.0430276  |                  0.0423459  |
|   10.08 |                   0.0136097  |                  0.0132778  |
|   10.09 |                   0.00554858 |                  0.00541325 |
|   10.1  |                   0.0166457  |                  0.0162397  |
|   10.11 |                   0.0633375  |                  0.0644891  |
|   10.12 |                   0.0810302  |                  0.0821179  |
|   10.13 |                   0.0862647  |                  0.0856314  |
|   10.14 |                   0.08616    |                  0.0865098  |
|   10.15 |                   0.0932789  |                  0.0946807  |
|   10.16 |                   0.0681533  |                  0.0679618  |
|   10.18 |                   0.00345477 |                  0.00337051 |
|   10.19 |                   0.0430276  |                  0.0424684  |
|   10.2  |                   0.0116206  |                  0.0113372  |
|   10.21 |                   0.0116206  |                  0.0113372  |
|   10.22 |                   0.0177973  |                  0.0173632  |
|   10.23 |                   0.0100503  |                  0.00980512 |
|   10.24 |                   0.00376884 |                  0.00367692 |
|   10.25 |                   0.00837521 |                  0.0096417  |

After performing this permutation test, we found an observed TVD of 0.008643318217101778 and a p-value of 0.993, so we do not reject the null hypothesis. Below is the distribution of the empirical TVD. 

<iframe
  src="assets/independent.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Now, we are going to test if the missingness of `firstbaron` column depends on the `position` of the player. We chose a significance level of 0.05 and using Total Variance Distance (TVD) as the test statistic. We perform the permutation test on `firstbaron` and `position`, and found that the missingness of `firstbaron` is dependent on `position`.

Null Hypothesis: The distribution of position when firstbaron is missing is the same as when firstbaron is not missing.

Alternative Hypothesis: The distribution of position when firstbaron is missing is not the same as when firstbaron is not missing.

The chart below shows the distribution of `position` depending on whether `firstbaron` is missing or not:

| position   |   firstbaron_missing = False |   firstbaron_missing = True |
|:-----------|-----------------------------:|----------------------------:|
| bot        |                          nan |                  0.199187   |
| jng        |                          nan |                  0.199187   |
| mid        |                          nan |                  0.199187   |
| sup        |                          nan |                  0.199187   |
| team       |                            1 |                  0.00406504 |
| top        |                          nan |                  0.199187   |

After performing this permutation test, we found an observed TVD of 0.49796747967479676 and a p-value of 0.0, so we reject the null hypothesis. Below is the distribution of the empirical TVD. 

<iframe
  src="assets/dependent.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

