---
author: "ZEPEI WANG, FRANCO PASTOR CUETO, DAFFA RABIN" 
output: html_document
---


```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, message = FALSE,
                      warning = FALSE, error = TRUE, fig.height = 3)
library(tidyverse)
library(lubridate)
library(kableExtra)
library(broman)
source("~/Desktop/STAT 240/scripts/viridis.R")
source("~/Desktop/STAT 240/scripts/ggprob.R")
theme_set(theme_minimal())
```

## The Change of Black and White Employment Ratio and Counts


### Introduction

In the past decade, the employment landscape in the United States has undergone significant shifts, with varying impacts on different racial groups. This report examines the changes in employment ratios and counts between black and white populations, seeking to uncover trends and disparities that might not be immediately apparent. Our analysis aims to provide insights into the socio-economic dynamics shaping the job market and to contribute to a broader understanding of racial equality in employment.


### Background: 

We used the dataset that includes all the USA employment rate, not only is about the women and men but also the different races of women's and men's among time. This dataset come from the CORGIS Dataset Project By Austin Cory Bart form 2016 that includes datas of labor, race, age, sex, gender, America, USA, census, employed, unemployed, employ ability, job, work, civilian, black, white, Asian, government. Where we are trying to compare the employment rate of different races. Where we named it as the labor.csv. This dataset contains the total employment rates every month through out the year. 

Given the socio-economic transformations over the past decade, including technological advancements and shifts in industry demands, this analysis is particularly relevant. Additionally, the impact of external events, such as the 2008 financial crisis and the COVID-19 pandemic, is considered as they potentially influenced employment trends significantly.



### Data citation:
Women and men employment rate in the USA: " by Austin Cory Bart, created 3/11/2016, https://corgis-edu.github.io/corgis/csv/labor/"


### Some focus:
Our first focus is the trend of both employment counts for both white and black male vs. female over time by using line and area graphs of ratio data to show it. Our second focus is that we are trying to find out the comparison of unemployment counts between both the black and white male and female difference over the past years by using histograms and area plots. Our third focus is trying to find the total(black+white) comparison of gender employment counts by using box plot. Finally, our last goal is trying to state the hypothesis of whether there is no significant difference in the employment-population ratio between the total(black+white) men and women in the United States over the years 1970-2016. 


### Analysis:



#### 1. Employment Trends Among Black or African American Men and Women

First, we want to see the comparison of employment counts between Black or African American men and women over the years of 1970-2016, as per the data from the labor dataset. The blue line represents men, and the red line represents women. This visualization can provide insights into trends, disparities, or changes in employment patterns for these demographics over time.
```{r, echo = FALSE}
labor_data = read.csv("~/Desktop/STAT 240/Data/labor.csv")

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


In order to find out the reason that black women's employment count can surpasses men's, we continued to make research on the black men and women employed population ratio to find out the reason from 1970 to 2016. The blue area plot represents men, and the red area plot represents women.
```{r, echo=FALSE}
employed_black_ratio_men = labor_data$Data.Employed.Black.or.African.American.Employment.Population.Ratio.Men
employed_black_ratio_women = labor_data$Data.Employed.Black.or.African.American.Employment.Population.Ratio.Women
years = labor_data$Time.Year

plot_data = data.frame(years, 
                       Ratio = c(employed_black_ratio_men, employed_black_ratio_women),
                       Group = rep(c("Black or African American Men", "Black or African American Women"), each=length(years)))

ggplot(plot_data, aes(x = years, y = Ratio, fill = Group)) +
  geom_area(alpha = 0.5) +
  scale_fill_manual(values = c("Black or African American Men" = "blue", 
                               "Black or African American Women" = "red")) +
  labs(x = "Year", y = "Employment-Population Ratio (%)", 
       title = "Employment-Population Ratio Comparison") +
  facet_wrap(~ Group, ncol = 2)
```
This graph shows that at the start of 1970s, the employment population for men and women are at two extremes. The black African men's employed population ratio is more than 70% but the women's ratio is less than 30%. However, after 1980s, the ratio difference between men and women has become smaller and smaller, which is the reason that why the Black women employment counts can surpass men after 1980s from the previous graph.


Then, we used the histogram method to visually represents the comparison of unemployed trends between Black or African American men and women over the years.
```{r, echo=FALSE}
unemployed_black_men = labor_data$Data.Unemployed.Black.or.African.American.Counts.Men
unemployed_black_women = labor_data$Data.Unemployed.Black.or.African.American.Counts.Women
years = labor_data$Time.Year

labor_data$Decade = cut(labor_data$Time.Year, breaks=seq(1970, 2020, by = 25), labels = seq(1970, 2010, by = 25), right=FALSE)

Unemployed_data = data.frame(Decade = labor_data$Decade,
                       UnemploymentRate = c(unemployed_black_men, unemployed_black_women),
                       Group = rep(c("Black Men", "Black Women"), each=length(years)))

ggplot(Unemployed_data, aes(x = UnemploymentRate, fill = Group)) +
  geom_histogram(position = "dodge", bins = 30, alpha = 0.7) +
  scale_fill_manual(values = c("Black Men" = "black", "Black Women" = "skyblue")) +
  labs(x = "Unemployment Rate", y = "Frequency", 
       title = "Unemployment Rate Trends: Black Men vs Women") +
  facet_wrap(~ Decade, scales = "free_x")
```

Based on the two histograms, we find out that for both Black men and Black women, unemployment rates mostly fall between 500 and 1000. The frequency of unemployment rates decreases significantly above 1000 for both groups. Comparing the distributions, Black men's unemployment rates in 1970-1995 show a broader spread than Black women's, implying more variability. By 1995 to the present, the peak unemployment rates for both groups appear to have increased from the 1970 levels, which suggest a rise in unemployment over time for both Black men and Black women.

#### 2. Employment Dynamics in White Populations

Then, we decide to also look into the white women and men employment ratio in the past 50 years.
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


Then, we also dive into the unemployment trends among white population in the past decades.
```{r, echo=FALSE}
latest_year = max(labor_data$Time.Year)
filtered_data = subset(labor_data, Time.Year > (latest_year - 50))

plot_data = filtered_data[, c('Time.Year', 'Data.Unemployed.White.Counts.Men', 'Data.Unemployed.White.Counts.Women')]

names(plot_data) = c('Year', 'Unemployed_Men', 'Unemployed_Women')

plot_data_long = pivot_longer(plot_data, cols = c(Unemployed_Men, Unemployed_Women), names_to = 'Gender', values_to = 'Unemployment_Counts')

ggplot(plot_data_long, aes(x = Year, y = Unemployment_Counts, fill = Gender)) +
  geom_area(alpha = 0.6, position = 'stack') +
  scale_fill_manual(values = c('steelblue', 'salmon')) +
  theme_minimal() +
  labs(title = 'Trend of Unemployment Counts Between White Men and Women Over the Last 50 Years',
       x = 'Year',
       y = 'Unemployment Counts') +
  theme(legend.title = element_blank())
```
From the graph, we find valuable insights into the unemployment trends among the white population, highlighting gender disparities and changes over time. It is a useful tool to help us understanding how economic conditions and labor market dynamics have differently impacted the employment of white men and women through time. From the past 50 years, the unemployment counts for men is still much higher than women, especially the recent years, it has been worse.


#### 3. Total Population Employment Comparison by Gender
After by making comparison through two differnt races, we decide to explore the total population counts of women and men, combining data from both Black or African American and White demographics, over the period from 1970 to 2016, as per the data from the labor dataset. The graph is designed to provide a clear comparison between the two gender groups across these demographics.
```{r, echo=FALSE}
labor_data = labor_data %>%
  mutate(Total_Women = Data.Employed.Black.or.African.American.Employment.Population.Ratio.Women + 
                       Data.Employed.White.Employment.Population.Ratio.Women,
         Total_Men = Data.Employed.Black.or.African.American.Employment.Population.Ratio.Men + 
                     Data.Employed.White.Employment.Population.Ratio.Men)

gender_data = labor_data %>%
  select(Time.Year, Total_Women, Total_Men) %>%
  pivot_longer(cols = -Time.Year, names_to = "Group", values_to = "Population.Counts")

ggplot(gender_data, aes(x = Group, y = Population.Counts, fill = Group)) +
  geom_boxplot() +
  theme_minimal() +
  labs(title = "Comparison of Total Population Counts by Gender",
       x = "Gender Group",
       y = "Population Counts")
```
From the graph, we can observe trends, disparities, or changes in the total population counts for these gender groups over time. The purple box plot represents men, and the yellow box plot represents women. This graph can be particularly insightful in understanding how the demographics of employment have evolved over the decades, reflecting societal changes and shifts in labor market dynamics.


#### 4. Hypothesis Testing: Employment-Population Ratio Men and Women

Lastly, we want to test the hypothesis whehter if there is no significant difference in the employment-population ratio between men and women in the United States over the years.

##### 1. Data

```{r, echo=FALSE}
filtered_data = labor_data %>% 
  filter(Time.Year >= 1970 & Time.Year <= 2016)
men = filtered_data$Data.Employed.Black.or.African.American.Counts.Men + 
      filtered_data$Data.Employed.White.Counts.Men
women = filtered_data$Data.Employed.Black.or.African.American.Counts.Women + 
        filtered_data$Data.Employed.White.Counts.Women
t_test_result = t.test(men, women)
cat("p-value:", t_test_result$p.value, "\n")
print(t_test_result)
```

- mean difference(\( \bar{x} \)): 11,233.83.
- z(z-score): 1.96
- sd(standard deviation): 3,331.89.
- n(sample size): 528
- p-value: 2.2e-16

##### 2. Model
The hypothesis test can be modeled as follows, assuming a normal distribution based on the Central Limit Theorem (CLT) as n > 30.

- Model: \( X \sim Normal(\mu, \frac{\sigma}{\sqrt{n}}) \)


##### State Hypotheses
In this context, let's define the null and alternative hypotheses:


- Null Hypothesis (\(H_0\)): \( \mu_{men} = \mu_{women} \)
  - This states that there is no significant difference in the employment-population ratios between men and women.

- Alternative Hypothesis (\(H_1\)): \( \mu_{men} \neq \mu_{women} \)
  - This states that there is a significant difference in the employment-population ratios between men and women.

##### 3. Calculation
The confidence interval can be calculated using the formula:

\[ CI = \bar{x} \pm z \times \frac{s}{\sqrt{n}} \]

The 95% confidence interval for the mean difference between the employment counts of men and women is approximately:

- Lower Bound: 10,949.63
- Upper Bound: 11,518.02

Plugging in the values:

- Margin of Error = \( 1.96 \times \frac{3331.89}{\sqrt{528}} \)
- Lower Bound = \( 11233.83 - \text{Margin of Error} \)
- Upper Bound = \( 11233.83 + \text{Margin of Error} \)

##### 4. Test the Hypothesis
Using the t-test result, we examine the p-value to decide whether to reject the null hypothesis.

```{r, echo=FALSE}
if (t_test_result$p.value < 0.05) {
  cat("Reject the null hypothesis. There is a significant difference in employment-population ratio.\n")
} else {
  cat("Fail to reject the null hypothesis. There is no significant difference in employment-population ratio.\n")
}
```

Finally, by comparing this to the null hypothesis, we can see that the entire confidence interval is far above zero. This further supports the decision to reject the null hypothesis, indicating a significant difference in employment-population ratios between men and women in this dataset.



### Discussion

In our exploration of employment patterns among Black and White American populations in the United States, we present key findings and insights derived from our data analysis. The first important thing is the employment counts between Black or African American men and women from 1970 to 2016, we observed a significant increase in both genders' employment during the late 1980s and 1990s. Notably, the employment count for Black women surpassed that of Black men after 1990 but the employment count for White men still surpassed White women the whole time. We also understand the shift in employment counts, where we investigated the employment population ratio for Black men and women. In the 1970s, there was a stark contrast, with men having a ratio above 70% and women below 30%. However, this gap narrowed after the 1980s, contributing to the observed increase in Black women's employment counts by using area plots. Beside that, we also dived into unemployment rate trends for both balk and white men vs women by using histogram, where we persistent and possibly increasing unemployment issue among Black men and women, with variability in how this issue has manifested over time and between genders. External factors and broader socio-economic conditions likely play a significant role in these trends.
Then, after comparing to all the employment ratio for both races(black and white), we decide to find out the total comparison of men and women through the past decades by using box plot and finds out that the employment counts for women have a higher median and greater variability.In the end, we conducted a hypothesis testing on the employment population ratio with the null hypothesis stating that if there is no significant difference in the employment-population ratio between Black or African American men and women in the United States over the years and the result is false. There is a significant difference in employment-population ratio.

There are two short comings in the future for our project. First, since the data set comprises 63 observations, it provides a foundation for analysis but potentially limiting the generalization ability of findings. The second short coming is that based on our analysis, we focused on factors exhibiting strong patterns, potentially overlooking nuances present in weaker patterns. Exploring additional factors could unveil more insights by this way.The future directions for our project are mainly focused in two ways. First is by explore Racial and Gender Wage Gaps and the second way is to do more qualitative and quantitative researches. Lastly, I think we should also do more research on Asian employed ratio and count since if you take a close look at our database, you would find out that all the datas for Asian population is missing.

Our project provides valuable insights into the changing employment landscape for Black and White American populations through the recent years. While the data set has limitations, our findings contribute to understanding the dynamics of gender disparities in employment counts. Future research avenues include exploring additional factors, comparing wage gaps by ages, and to do more qualitative an quantitative researches.
