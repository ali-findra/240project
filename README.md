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
