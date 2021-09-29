# hw2
---
title: "Week 3 homework"
output:
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(nycflights13)
library(tidyverse)
```


#1. How many flights have a missing dep_time? What other variables are missing? What might these rows represent?

```{r }
sum(is.na(flights$dep_time))

summary(flights)
```
8255 flights have a missing `dep_time`, 8255 have a missing `dep_delay`, 8713 have a missing `arr_time`, 9430 have a missing `arr_delay`, and 9430 have a missing `air_time`. I guess these are the flights that failed to fly normally due to some objective factors (failure to take off and land on time)

#2.Currently dep_time and sched_dep_time are convenient to look at, but hard to compute with because they’re not really continuous numbers. Convert them to a more convenient representation of number of minutes since midnight.

```{r }
mutate(flights,
       dep_time = (dep_time %/% 100) * 60 + (dep_time %% 100),
       sched_dep_time = (sched_dep_time %/% 100) * 60 + (sched_dep_time %% 100))
```

#3.Look at the number of canceled flights per day. Is there a pattern? Is the proportion of canceled flights related to the average delay? 


```{r }
flights %>% group_by(month, day) %>%
  summarize(avg_dep_delay = mean(dep_delay, na.rm = TRUE),
            prop_cancelled = sum(is.na(dep_time)/n())) %>%
  ggplot(mapping = aes(x = avg_dep_delay, y = prop_cancelled)) +
  geom_point()
```
