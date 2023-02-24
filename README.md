
# League of Luxes

by Jason Hauk (jhauk@ucsd.edu)

---

## Introduction

Is Lux better as a midlaner or a support characters?

In the game of league of legends there's many decisions made throughout a game, that culminate in either a win or loss. One decision made at the beginning of the game is what character will be played in what position. Making good decisions here can have huge impacts on gameplay and the final result. To help make this decision lets explore what position lux should be played at if lux is being picked.

Looking at all of the competitive match data from 2022 I originally began with 149232 rows representing a player/team's game and 123 columns each represnting their own statistic tracked throughout a game. After isolating the rows to only look at when lux was played and only when she was in a position she was played in often, I was left with 361 rows. Of the 123 original columns, I selected to keep 7 of them that seemed relevant to exploring the characters performance between the two positions.

The 7 columns included:

champion: String containing only 'Lux' since at this stage all of the player stats we wanted were stats from her<br>win: Booleans reflecting True if the game was won and False otherwise<br>position: String was either sup or mid

Then 4 columns all of which contained ints and were different ways to measure how well they did during the game:
kills, deaths, assists, and totalgold

Now let us explore the process of how I got to this cleaned up data:

---

## Cleaning and EDA

I started by reading my data into a pandas dataframe I called ```data_raw```. One of the first decisions I made was to look at individual players rather than the teams as a whole, so I removed all rows that had their position as team.

The removal of these rows lefts many columns tracking team stats completely blanks and as such I deleted them:
['firstdragon', 'dragons', 'opp_dragons', 'elementaldrakes', 'opp_elementaldrakes', 'infernals', 
'mountains', 'clouds', 'oceans', 'chemtechs', 'hextechs', 'dragons (type unknown)', 'elders',
'opp_elders', 'firstherald', 'heralds', 'opp_heralds', 'firstbaron', 'firsttower', 'towers',
'opp_towers', 'firstmidtower', 'firsttothreetowers', 'turretplates', 'opp_turretplates', 'gspd']

With the remaining columns there were still a couple changes I wanted to make to make the data more easily usable.
I changed 'playoffs', 'results', 'firstblood', 'firstbloodassist', 'firstbloodkill', 'firstbloodvictim' all to booleans
since they were previously ints of 1 or 0 to represent boolean values. I also changed 'date' to pd.datetime() rather than
the string it was previously.

After all of these changes I was left with a dataframe I called ```players_df``` with had isolated the player stats, gotten rid of superfluous
columns, and converted columns to useable types.

While this dataframe would have been perfectly fine to work with, I still found the number of columns to be complicating matters and so I distilled the data more into just the columns I wanted and rows that had lux as the champion. Originally, I thought this would be enough.

<iframe src="assets/lux_all_pos.html" width=600 height=600 frameBorder=0></iframe>

After looking at the amount of times she appeared in each position, however, I decided to limit my queries to just when she had the position of sup or mid. That way I would have enough data to make meaningful conclusions. So in my final dataframe ```just_lux``` I only look at the champion lux, when she is played in the support or midlane position, with the 5 other columns that I explained in the introduction. Below I show the head of that dataframe as an example:

```py
just_lux.head()
```

|   | champion |  win  |  position | kills | deaths | assists | totalgold  |
|:--|----------|-------|-----------|-------|--------|---------|-----------:|  
| 0 |   Lux    | False |     sup   |   1   |    5   |     8   |    8609    |
| 1 |   Lux    | True  |     sup   |   3   |    3   |     8   |    8698    |
| 2 |   Lux    | False |     sup   |   0   |    6   |     1   |    8358    |
| 3 |   Lux    | False |     sup   |   1   |    3   |     7   |    8034    |
| 4 |   Lux    | False |     sup   |   0   |    9   |     1   |    4889    |

Once I had this dataframe it was important for me to get to understand the stats I was looking at before I tried to address my central question.

<iframe src="assets/lux_win_perc.html" width=600 height=600 frameBorder=0></iframe>

One of the first and simplest things that I looked at was how lux did overall in their games. From the graph it is easy to see that lux wins over 50% of her games and thus a majority of her games. That being said it's onlt 53.74% wins to 46.26% losses so it isn't by super wide margins.

<iframe src="assets/lux_kill_hist.html" width=600 height=600 frameBorder=0></iframe>

Next I wanted to get a better understanding of the impact that lux was having in the games that she was playing in. The histogram showing her kills shows the data is left-skewed indicating that many of her games she has a rather low kill count.

<iframe src="assets/lux_kill_box.html" width=600 height=600 frameBorder=0></iframe>

The boxplot of her kills, reinforces what I had already speculated based on the histogram, that indeed many of her games she has a low kill count. In fact, the median is 1 kill meaning in atleast half of her games she either got 1 kill or none at all. This would seem to indicate a rather weak presence in the game.

Now I was very curious to see if these trends would stay true if we split up the positions or if a trend might emerge. As such I began looking at the same data while also taking position into account. I discovered the following:



---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing
