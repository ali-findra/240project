# 240project
---
author: "Zepei Wang, Daffa Rabin, Pastor Franco Cueto
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, message = FALSE,
                      warning = FALSE, error = TRUE, fig.height = 3)
library(tidyverse)
library(kableExtra)
library(broman)
source("../../scripts/viridis.R")
source("../../scripts/ggprob.R")
theme_set(theme_minimal())
```
```{r}
w_men_data = read.csv("~/Desktop/STAT 240/Data/labor.csv")
t_data = read.csv("~/Desktop/STAT 240/Data/employment-overall.csv")
```
Introduction:





Background: 
About the raw data:

We combined two different datasets, all about the USA employment rate, not only is abou the women and men but also the different races among time. The first data set we used is called US Employment and Unemployment rates since 1940 (renamed to employment-overall.csv), was collected from datahub 6 years ago. This dataset contains the yearly NBA finals and NBA regular-season MVPs, including the champion of western and eastern conferences, the vice NBA champion, the NBA champion, and the MVP’s physical info such as height, weight, etc. The second dataset and third dataset come from Basketball Reference, which contains official data records for the NBA. This dataset, originally called NBA winners, was renamed to mvp_satats.csv. This dataset contains data about each mvp of each year and their basketball ability. The third dataset, called players, contains the basic data of all NBA players, not only MVPs. It gives information such as their college and birth place. By matching the name of MVPs, we extrach the MVP info that we need.

The MVP dataset we finally use:

The data we are using is all based on the MVP of the last 60 or so seasons. The data dives deeper into each player who won the award, giving statistics such as height, field goal percentage, and three-point percentage. Some of the statistics may seem hard to understand for people who don’t watch basketball. MP means minutes played per each game. One of the easier ones to encode is PTS, which is points scored per game. TRB is total rebounds per game, AST is total assists per game (an assist is when a player passes the ball to the scorer). FG% is the percent of total shots made, whereas 3PT% is the percentage of shots made behind the 3 point arc. FT% is the total percentage of free throws made. Some less common stats are WS and WS/48 which are the estimated number of wins contributed by a certain player and estimated number of wins contributed per 48 minutes respectively. Something to keep in mind is that the three point line wasn’t added until the beginning of the 1979 season which explains the missing values in some columns.

Data citation:

Overall employment rate in the USA: " Datahub, https://datahub.io/core/employment-us#resource-employment-us_zip"
Women and men employment rate in the USA: " by Austin Cory Bart, created 3/11/2016, https://corgis-edu.github.io/corgis/csv/labor/"


Some focus:

Our first focus is the trend in MVP’s positions over time. Due to the trend of MVP’s positions over time, we also see a trend of incresed emphasis on three point shooting, a player’s ability to score a three point shot. Our second focus is on data about each MVP’s physical information: Their height and weight in relation to the year they win the MVP. Third is how much the mvp contributed to the team. We focused on win shares to see whether the MVP deserves the award.

