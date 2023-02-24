
# League of Luxes

by Jason Hauk (jhauk@ucsd.edu)

---

## Introduction

Is Lux better as a midlaner or a support characters?

In the game of league of legends there's many decisions made throughout a game, that culminate in either a win or loss. One decision made at the beginning of the game is what character will be played in what position. Making good decisions here can have huge impacts on gameplay and the final result. To help make this decision lets explore what position lux should be played at if they're being picked.

Looking at all of the competitive match data from 2022 I originally began with 149232 rows and 123 columns. After isolating the rows to only look at lux and roles she'd been played in a significant number of times, I was left with 361 rows. Of the 123 original columns, I selected to keep 7 of them that seemed relevant to exploring the characters performance between the two positions.

The 7 columns included:

champion: String containing only 'Lux' since at this stage all of the player stats we wanted were stats from her
win: Booleans reflecting True if the game was won and False otherwise
position: String was either sup or mid

Then 4 columns all of which contained ints and were different ways to measure how well they did during the game:
kills, deaths, assists, and totalgold

Now let us explore the process of how I got to this cleaned up data:

---

## Cleaning and EDA

I started by reading my data into a pandas dataframe I called data_raw. One of the first decisions I made was to look at individual players rather than the teams as a whole, so I removed all rows that had their position as team.

The removal of these rows lefts many columns tracking team stats completely blanks and as such I deleted them:
['firstdragon', 'dragons', 'opp_dragons', 'elementaldrakes', 'opp_elementaldrakes', 'infernals', 
'mountains', 'clouds', 'oceans', 'chemtechs', 'hextechs', 'dragons (type unknown)', 'elders',
'opp_elders', 'firstherald', 'heralds', 'opp_heralds', 'firstbaron', 'firsttower', 'towers',
'opp_towers', 'firstmidtower', 'firsttothreetowers', 'turretplates', 'opp_turretplates', 'gspd']

With the remaining columns there were still a couple changes I wanted to make to make the data more easily usable.
I changed 'playoffs', 'results', 'firstblood', 'firstbloodassist', 'firstbloodkill', 'firstbloodvictim' all to booleans
since they were previously ints of 1 or 0 to represent boolean values. I also changed 'date' to pd.datetime() rather than
the string it was previously.

After all of these changes I was left with a dataframe I called players_df with had isolated the player stats, gotten rid of superfluous
columns, and converted columns to useable types.

While this dataframe would have been perfectly fine to work with, I still found the number of columns to be complicating matters and so I distilled the data more into just the columns I wanted and rows that had lux as the champion. Originally, I thought this would be enough.

<iframe src="assets/lux_all_pos.html" width=800 height=600 frameBorder=0></iframe>



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
