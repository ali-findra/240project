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

As students in economics and data science, our group was interested in how certain demographics were impacted by changes in the American economy throughout history. Our group was curious about the status of races for black and white populations in the workforce and how we could use the lessons we learned in Statistics 240 to better understand the economic changes this nation experienced.  In the project we wanted to ask ourselves: how has the employment rate changed among black and white populations through decades. 


Background: 
About the raw data:

We used the dataset that includes all the USA employment rate, not only is about the women and men but also the different races of women's and men's among time. This dataset come from the CORGIS Dataset Project By Austin Cory Bart form 2016 that includes datas of labor, race, age, sex, gender, america, usa, census, employed, unemployed, employability, job, work, civilian, black, white, asian, government. Where we are trying to compare the employment rate of differnt races. Where we named it as the labor.csv. This dataset contains the total employment rates every month through out the year. 


Data citation:
Women and men employment rate in the USA: " by Austin Cory Bart, created 3/11/2016, https://corgis-edu.github.io/corgis/csv/labor/"


Some focus:
Our first focus is the trend of total employment rate for white male vs. female over time. We also tries to see the differnce btw. the black male and female employment rate differnce over the past few decades. Our second focus is on trying to prove the hypothes of that, " What is the rate of the population over 40 is unemployed. 

This is the ratio, the employments counts is the major part, this is just my extension explanation.

```{r, echo=FALSE}
employed_black_ratio_men = labor_data$Data.Employed.Black.or.African.American.Employment.Population.Ratio.Men
employed_black_ratio_women = labor_data$Data.Employed.Black.or.African.American.Employment.Population.Ratio.Women
years = labor_data$Time.Year

plot_data = data.frame(years, employed_black_ratio_men, employed_black_ratio_women)

ggplot(plot_data, aes(x = years)) +
  geom_line(aes(y = employed_black_ratio_men, colour = "Black or African American Men")) +
  geom_line(aes(y = employed_black_ratio_women, colour = "Black or African American Women")) +
  labs(x = "Year", y = "Employment-Population Ratio (%)", 
       title = "Employment-Population Ratio Comparison: Black or African American Men vs Women") +
  scale_colour_manual("", 
                      breaks = c("Black or African American Men", "Black or African American Women"),
                      values = c("blue", "red")) +
  theme_minimal()
```

```{r, echo=FALSE}
black_lm <- lm(Data.Employed.Black.or.African.American.Employment.Population.Ratio.Women ~ Time.Year, data = w_men_data)

black_summary_table <- summary(black_lm)

black_t_test_statistic <- black_summary_table$coefficients[2, "t value"]
black_p_value <- black_summary_table$coefficients[2, "Pr(>|t|)"]

alpha <- 0.05

if (black_p_value < alpha) {
  cat("Reject the null hypothesis\n")
} else {
  cat("Fail to reject the null hypothesis\n")
}
```

```{r, echo=FALSE}
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

```{r, echo=FALSE}
white_lm <- lm(Data.Employed.White.Employment.Population.Ratio.Women ~ Time.Year, data = w_men_data)

white_summary_table <- summary(white_lm)

white_t_test_statistic <- white_summary_table$coefficients[2, "t value"]
white_p_value <- white_summary_table$coefficients[2, "Pr(>|t|)"]

alpha <- 0.05

if (white_p_value < alpha) {
  cat("Reject the null hypothesis\n")
} else {
  cat("Fail to reject the null hypothesis\n")
}
```

> The graph depicts the employment trends for white men and women over the past 50 years, showing a consistent increase in employment for both groups. While white men's employment remains higher, both trends parallel each other, indicating proportional growth without a closing gap.

(Franco)
