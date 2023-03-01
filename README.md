
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

<iframe src="assets/lux_win_per.html" width=600 height=600 frameBorder=0></iframe>

One of the first and simplest things that I looked at was how lux did overall in their games. From the graph it is easy to see that lux wins over 50% of her games and thus a majority of her games. That being said it's onlt 53.74% wins to 46.26% losses so it isn't by super wide margins.

<iframe src="assets/lux_kill_hist.html" width=600 height=600 frameBorder=0></iframe>

Next I wanted to get a better understanding of the impact that lux was having in the games that she was playing in. The histogram showing her kills shows the data is right-skewed indicating that many of her games she has a rather low kill count.

<iframe src="assets/lux_kill_box.html" width=600 height=600 frameBorder=0></iframe>

The boxplot of her kills, reinforces what I had already speculated based on the histogram, that indeed many of her games she has a low kill count. In fact, the median is 1 kill meaning in atleast half of her games she either got 1 kill or none at all. This would seem to indicate a rather weak presence in the game.

Now I was very curious to see if these trends would stay true if we split up the positions or if a trend might emerge. As such I began looking at the same data while also taking position into account. I discovered the following:

<iframe src="assets/lux_win_per_split.html" width=600 height=600 frameBorder=0></iframe>

When you break down the wins based on position an interesting change occurs. While the support position still wins a majority of their games, the midlane lux games do not. Midlane lux has a win percentage of 39.47% compared to the support lux win percentage of 55.42%. This creates an interesting discrepency which is at the heart of the question I'm seeking to answer: is lux better at midlane or support? Based on this graph alone I might be tempted to say that lux is better at support, but we still have a few tests to run before we could support a conclusion like that.

First lets see if we can find any supportive evidence in the way that the amount of kills change based on role:

<iframe src="assets/lux_kill_hist_split.html" width=600 height=600 frameBorder=0></iframe>

This graph is interesting because it's the opposite of what I would've initially expected. Since support lux had a higher win percentage I also thought that they would end up having more kills. Looking at this graph, however, the left skew is less extreme for lux mid. This indicates that lux mid was in fact getting more kills.

<iframe src="assets/lux_kill_box_split.html" width=600 height=600 frameBorder=0></iframe>

The previous histogram is supported by this boxplot which further illustrates the kill disparity between support and midlane. While the support lux is still stuck on a median of 1 kill a game, midlane lux has a median of 2.5. It's pretty apparent that midlane lux is getting more kills, but does this disprove our theory that support lux is better than midlane lux? Not necesssarily...

A potential explanation for this discrepancy is that the roles are simply meant to do different things. Even if a midlane lux is getting more kills than a support lux, the midlaner isn't competing against the support. They're competing against the enemy midlaner. The same goes for the support not competing against mid, but rather the opposing teams support. The goal of a support might not be to get kills, but rather instead to support your teammates in getting them.

| position |    mid    |    sup    |
| assists  |           |           |
|:---------|-----------|----------:|  
|   0-9    | 78.947368 | 48.297214 |
|   9-18   | 21.052632 | 46.130031 |
|   18-27  |  0.000000 | 5.263158  |
|   27-36  |  0.000000 | 0.309598  |

This pivot table seems to support my theory that supports focus on a more assist heavy playstyle. The pivot table is indexed by a range of assists that were obtained by the the luxes and then the columns split up mid and support. In each box is the percent of that position, which fit in that range of assists. For example, in the 0-9 row and in the middle column we have approx. 78.95, thus 78.95% of midlane lux got 0-9 assists. While a vast majority of midlane luxes got 0-9 assists, the supports saw a relatively close split between 0-9 and 9-18 assists. With a majority of support lux getting 9+ assists (the interval is left inclusive, right exclusive) it seems that my theory about lux mid and lux sup simply having different roles in the game is pretty likely.

---

## Assessment of Missingness

Taking a step back from the datafram we had just been looking at, I wanted to go back and explore the raw data. Specifically, I was curious if any of the columns I left behind we missing data due to the value of the data itself. That is to say, was there any data not missing at random?

An example of Not Missing at Random (NMAR) I found would be the missing values in the column teamname. It seems that it's always the same team missing since the teamid value matches for every row that was missing a value in teamname. Perhaps the person entering the data didn't like the team or more maybe it was in another language and they weren't able to translate it. Regardless of the reason, it seems probable that the reason it wasn't included was because of the teamname value itself.

Another example of missing data that I found curious was when a playerid was missing. I wasn't sure what would cause this, but I had theory that it could be correlated or dependent on the league that the game/player played in, since leagues with bigger audiences and more budgets seemed less liekly to have these types of mistakes.

I wanted to run a perumtation test to check out my theory and to do so I would use total variation distance as I was dealing with categorical distributions. My null hypothesis would be that the distribution of 'league' when 'playerid' is missing is the same as the distribution of 'league' when 'playerid' is not missing, and thus the two columns are independent. My alternative hypothesis would be that the distribution of 'league' when 'playerid' is missing is different then the distribution of 'league' when 'playerid' is not missing, and thus the two columns are dependent. I got the following results when I ran my permutation test:

<iframe src="assets/league_mar.html" width=600 height=600 frameBorder=0></iframe>

With a p-value of 0, which is less than 0.05, I was forced to reject the null and thus support the hypothesis that the two columns are dependent. Therefor, I believe that the 'playerid' column is Missing at Random (MAR) rather than MCAR.

---

## Hypothesis Testing

Now back to my original question, I had wondered if lux was better as a midlaner or support. I decided that the best way to analyze this was based on the win percentages, because as I explained other measurements like kills, assists, or gold could simply be changing due to the role that each position is supposed to play. That means the best way to measure if a champion is best played in a specific role, we should simply look at if they win in that role.

As such, I wanted to see if the difference we had observed between the mid and sup win percentage was significant or not.<br>Null Hypothesis: There is no meaningful difference between playing lux sup or lux mid, and any difference observed can be attributed to random chance.<br>Alternative Hypothesis: Playing lux sup gives a statistically significant advantage over playing lux mid.

To test my hypothesis I will be doing a permutation test using the mean differences between the win percentage of mid an sup. This was the result of my permutation test: 

<iframe src="assets/perm_test.html" width=600 height=600 frameBorder=0></iframe>

The resuling p-value was 0.0465.

Since the p-value of 0.0465 is less than 0.05, with a confidence level of 95%, I chose to reject the null hypothesis. Thus our sample data instead supports our alternative hypothesis, that playing lux sup gives a meaningful adnvantage compared to playing lux mid.

Thus I was able to answer my question: Is Lux better as a midlaner or a support characters?<br>-Lux is better as a support character :D
