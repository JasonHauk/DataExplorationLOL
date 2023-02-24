
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

```py
print(counts[['champion', 'win', 'position' 'kills', 'deaths', 'assists', 'totalgold']].head().to_markdown(index=False))
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
