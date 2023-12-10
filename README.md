---
author: "Zepei Wang, FRANCO PASTOR CUETO, DAFFA RABIN" 
output: html_document
---


```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, message=FALSE, warning=FALSE)
library(tidyverse)
library(lubridate)
library(kableExtra)
library(broman)
source("../../scripts/viridis.R")
source("../../scripts/ggprob.R")
```

<br/>

## The Change of Black and White Employment Ratio and Counts


### Introduction

As students in economics and data science, our group was interested in how certain demographics were impacted by changes in the American economy throughout history. Our group was curious about the status of races for black and white populations in the workforce and how we could use the lessons we learned in Statistics 240 to better understand the economic changes this nation experienced.  In the project we wanted to ask ourselves: how has the employment rate changed among black and white populations through decades. 


### Background: 
About the raw data:

We used the dataset that includes all the USA employment rate, not only is about the women and men but also the different races of women's and men's among time. This dataset come from the CORGIS Dataset Project By Austin Cory Bart form 2016 that includes datas of labor, race, age, sex, gender, america, usa, census, employed, unemployed, employability, job, work, civilian, black, white, asian, government. Where we are trying to compare the employment rate of differnt races. Where we named it as the labor.csv. This dataset contains the total employment rates every month through out the year. 


### Data citation:
Women and men employment rate in the USA: " by Austin Cory Bart, created 3/11/2016, https://corgis-edu.github.io/corgis/csv/labor/"


### Some focus:
Our first focus is the trend of total employment rate for white male vs. female over time by using line graphs of ratio and counts data to show it. Our second focus is that we are trying to find out the differnce btween the black male and female employment rate differnce over the past years. Our last focus is trying to state the hypothesis of whether there is no significant difference in the employment-population ratio between Black or African American men and women in the United States over the years 1970-2016. 


### Analysis:

First, we want to see the comparison of employment counts between Black or African American men and women over the years of 1970-2016, as per the data from the labor dataset. The blue line represents men, and the red line represents women. This visualization can provide insights into trends, disparities, or changes in employment patterns for these demographics over time.
```{r, echo=FALSE}
labor_data = read.csv("../../data/labor.csv")

employed_black_men = labor_data$Data.Employed.Black.or.African.American.Counts.Men
employed_black_women = labor_data$Data.Employed.Black.or.African.American.Counts.Women
years = labor_data$Time.Year

plot_data = data.frame(years, employed_black_men, employed_black_women)

ggplot(plot_data, aes(x = years)) +
  geom_line(aes(y = employed_black_men, colour = "Black or African American Men")) +
  geom_line(aes(y = employed_black_women, colour = "Black or African American Women")) +
  labs(x = "Year", y = "Employed Counts", 
       title = "Employment Comparison: Black or African American Men vs Women") +
  scale_colour_manual("", 
                      breaks = c("Black or African American Men", "Black or African American Women"),
                      values = c("blue", "red")) +
  theme_minimal()
```
Based on the graph, we can tell that both the Black women and men's employments have increased rapidly through 1985 to 1995 and than the trend is increasing at a more gentle pace. In addition, we can tell that the women's employment count has surpass the men's employment count after 1990.


In order to find out the reason that black women's employment count can surpasses men's, we continued to make research on the black men and women employed population ratio to find out the reason from 1970 to 2016. The blue line represents men, and the red line represents women.
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
This graph shows that at the start of 1970s, the employment population for men and women are at two extremes. The black African men's employed population ratio is more than 70% but the women's ratio is less than 30%. However, after 1980s, the ratio difference between men and women has become smaller and smaller, which is the reason that why the Black women employment counts can surpass men after 1980s from the previous graph.


Then, we used the area method to visually represents the comparison of employed counts between Black or African American men and women over the years.
```{r, echo=FALSE}
employed_black_men = labor_data$Data.Employed.Black.or.African.American.Counts.Men
employed_black_women = labor_data$Data.Employed.Black.or.African.American.Counts.Women
years = labor_data$Time.Year

combined_data = gather(plot_data, key = "Gender", value = "Employed_Counts", -years)

combined_plot = ggplot(combined_data, aes(x = years, y = Employed_Counts, fill = Gender)) +
  geom_area(color = "black", alpha = 0.7) +
  labs(x = "Year", y = "Employed Counts", 
       title = "Employment Comparison: Black or African American Men vs Women") +
  scale_fill_manual(values = c("blue", "red")) +
  theme_minimal() +
  facet_wrap(~Gender, scales = "free_y")

print(combined_plot)
```
Based on the plot, we think it will give you guys a more obvious idea of the increase in employment for both genders, with women's employment surpassing men's after 1990.

After the research of black women and men employment ratio, we decide to also look into the white women and men employment ratio in the past 50 years.
```{r, echo=FALSE}
recent_data = labor_data %>%
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
  scale_color_manual(values = c("White Men" = "blue", "White Women" = "red")) + 
  labs(title = "Employment Comparison Between White Women and White Men (Last 50 Years)",
       x = "Year", y = "Employment Counts") +
  theme_minimal()

employment_graph
```
Based on the graph, we find out that the although white women's employment counts also shows a great increase in trend in the past decades, white men's employment's rate is still much higher than women.


Lastly, we want to test the hypothesis whehter if there is no significant difference in the employment-population ratio between Black or African American men and women in the United States over the years.
#### 1. data
```{r, echo=FALSE}
filtered_data = labor_data %>% 
  filter(Time.Year >= 1970 & Time.Year <= 2016)
men = filtered_data$Data.Employed.Black.or.African.American.Counts.Men
women = filtered_data$Data.Employed.Black.or.African.American.Counts.Women
t_test_result = t.test(employed_black_men, employed_black_women)
print(t_test_result)
```

#### 2.model
```{r, echo=FALSE}
t_test_result <- t.test(employed_black_men, employed_black_women)

mean_diff <- mean(employed_black_men) - mean(employed_black_women)
sd_diff <- sd(employed_black_men - employed_black_women)

cat("1. Model: X ∼ Normal(μ =", round(mean_diff, 2), ", σ =", round(sd_diff, 2), ")\n")
```


#### 3.Test the Hypothesis
```{r, echo=FALSE}
t_test_result <- t.test(employed_black_men, employed_black_women)

cat("p-value:", format.pval(t_test_result$p.value), "\n")

if (t_test_result$p.value < 0.05) {
  cat("Reject the null hypothesis. There is a significant difference in employment-population ratio.\n")
} else {
  cat("Fail to reject the null hypothesis. There is no significant difference in employment-population ratio.\n")
}
```
1. First we create a line that fits a linear model (lm) where the employment ratio for women of each race is regressed against the variable Time.Year using the dataset w_men_data. $\overline{y} = (a) + (b) * x$.
2. Next, we generate a summary table for the linear model fitted in the previous step.
3. Then we extract the t-test statistic associated with the coefficient for Time.Year from the summary table using the formula $t = \frac{\overline{x} - \mu}{\frac{s}{\sqrt{n}}}$.
4. Then we extract the p-value statistic associated with the coefficient for Time.Year from the summary table using the formula $z = \frac{x - \mu}{\delta}$ .
5. Finally we set the significance level or $\alpha$ to 0.05 and check whether the p-value is less than alpha. If true, it prints "Reject the null hypothesis", meaning the hypothesis is $TRUE$. Otherwise, it prints "Fail to reject the null hypothesis" meaning the hypothesis is $TRUE \sim FALSE$

### Discussion

In our exploration of employment patterns among Black and White American populations in the United States, we present key findings and insights derived from our data analysis. The first important thing is the employment counts between Black or African American men and women from 1970 to 2016, we observed a significant increase in both genders' employment during the late 1980s and 1990s. Notably, the employment count for Black women surpassed that of Black men after 1990 but the employment count for White men still surpassed White women the whole time. We also understand the shift in employment counts, where we investigated the employment population ratio for Black men and women. In the 1970s, there was a stark contrast, with men having a ratio above 70% and women below 30%. However, this gap narrowed after the 1980s, contributing to the observed increase in Black women's employment counts. Beside that, we also concrete an area plot, which can visually represents the comparison of employed counts between Black or African American men and women over the years more obvious. The plot highlights the increase in employment for both genders, with women's employment surpassing men's after 1990. In the end, we conducted a hypothesis testing on the employment population ratio with the null hypothesis stating that if there is no significant difference in the employment-population ratio between Black or African American men and women in the United States over the years and the result is false. There is a significant difference in employment-population ratio.

There are two short comings in the future for our project. First, since the data set comprises 63 observations, it provids a foundation for analysis but potentially limiting the generalizability of findings. The second short coming is that based on our analysis, we focused on factors exhibiting strong patterns, potentially overlooking nuances present in weaker patterns. Exploring additional factors could unveil more insights by this way.The future directions for our project are mainly focused in two ways. First is by explore Racial and Gender Wage Gaps and the second way is to do more qualitative and quantitative researches.

Our project provides valuable insights into the changing employment landscape for Black and White American populations through the recent years. While the data set has limitations, our findings contribute to understanding the dynamics of gender disparities in employment counts. Future research avenues include exploring additional factors, comparing wage gaps by ages, and to do more qualitative an quantitative researches.

> The graph depicts the employment trends for white men and women over the past 50 years, showing a consistent increase in employment for both groups. While white men's employment remains higher, both trends parallel each other, indicating proportional growth without a closing gap.

(Franco)
