
# Exploratory Data Analysis

by Jason Hauk (jhauk@ucsd.edu)

---

## Introduction

Is Lux better as a midlaner or a support characters?

In the game of league of legends there's many decisions made throughout a game, that culminate in either a win or loss. One decision made at the beginning of the game is what character will be played in what position. Making good decisions here can have huge impacts on gameplay and the final result. To help make this decision, I'm trying to figure out if lux is being chosen, what position should it be played at?

Looking at all of the competitive match data from 2022 I originally began iwht 149232 rows and 123 columns. After isolating the rows to only look at lux in the position of mid or sup I was left with 361 rows/games. Of the 123 original columns, I selected to keep 7 of them that seemed relevant to exploring the characters performance between the two positions.

The 7 columns included:

champion: all Lux
win: held booleans reflecting True if the game was won and False otherwise
position: either sup or mid

Then 4 columns all of which were different ways to measure how well they did during the game:
kills, deaths, assists, and totalgold

Now let us explore the process of how I got to this cleaned up data:

---

## Cleaning and EDA

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
