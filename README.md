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

As students in economics and data science, our group was interested in how certain demographics were impacted by changes in the American economy throughout history, specifically women. Our group was curious about the status of women in the workforce compared to men and how we could use the lessons we learned in Statistics 240 to better understand the economic changes the women of this nation experienced. In the project we wanted to ask ourselves: how has the employment and unemployment rate changed among women when compared to American society as a whole? 
- Firstly we
- Secondly we
- Finally, we

Background: 
About the raw data:

We combined two different datasets, all about the USA employment rate, not only is about the women and men but also the different races of women's among time. The first data set we used is called US Employment and Unemployment rates since 1939 (renamed to employment-overall.csv) from the Us government official website. This dataset contains the total employment rate every month through out the year. The second dataset come from the CORGIS Dataset Project By Austin Cory Bart form 2016 that includes datas of labor, race, age, sex, gender, america, usa, census, employed, unemployed, employability, job, work, civilian, black, white, asian, government. WHere we are trying to compare the unemployment rate vs the employment rate of women to the all. Where we named it as the overall_employment.csv. 


Data citation:

Overall employment rate in the USA: " BLS Beta Labs, [https://datahub.io/core/employment-us#resource-employment-us_zip](https://beta.bls.gov/dataViewer/view/timeseries/CES0000000001)"
Women and men employment rate in the USA: " by Austin Cory Bart, created 3/11/2016, https://corgis-edu.github.io/corgis/csv/labor/"


Some focus:

Our first focus is the trend of total employment rate over time. We also tries to see the differnce btw. the women and men employment rate differnce over the past few decades. Our second focus is on data about differnt races of women's employment rate over time and the cause between each increase or decrease. Third is the unemployment rate vs the employment rate of women compare to the all population.


w_men_data = read.csv("../data/labor.csv")

recent_data = w_men_data %>%
  filter(Time.Year >= max(Time.Year) - 50)

employment_data = recent_data %>%
  select(Time.Year, Data.Employed.White.Counts.Men, Data.Employed.White.Counts.Women) %>%
  pivot_longer(cols = c("Data.Employed.White.Counts.Men", "Data.Employed.White.Counts.Women"), 
               names_to = "Gender", values_to = "Employment") %>%
  mutate(Gender = recode(Gender, 
                         "Data.Employed.White.Counts.Men" = "White Men", 
                         "Data.Employed.White.Counts.Women" = "White Women"))

employment_graph = ggplot(employment_data, aes(x = Time.Year, y = Employment, color = Gender)) +
  geom_line() +
  scale_color_manual(values = c("White Men" = "blue", "White Women" = "purple")) + # Changed colors
  labs(title = "Employment Comparison Between White Women and White Men (Last 50 Years)",
       x = "Year", y = "Employment Counts") +
  theme_minimal()

employment_graph

```

> The graph depicts the employment trends for white men and women over the past 50 years, showing a consistent increase in employment for both groups. While white men's employment remains higher, both trends parallel each other, indicating proportional growth without a closing gap.

(Franco)
