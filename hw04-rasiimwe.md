hw04-rasiimwe.rmd
================
Becky
2018-10-06

Homework 4: Data Wrangling with data aggregation and data reshaping
===================================================================

Data Reshaping
--------------

Overview: Problem - You have data in one “shape” but you wish it were in another. Usually this is because the alternative shape is superior for presenting a table, making a figure, or doing aggregation and statistical analysis. Solution: Reshape your data. For simple reshaping, gather() and spread() from tidyr will suffice.

### Activity #2

> Make a tibble with one row per year and columns for life expectancy for two or more countries. Use knitr::kable() to make this table look pretty in your rendered homework. Take advantage of this new data shape to scatterplot life expectancy for one country against that of another.

### Loading required packages for this assignment

``` r
suppressPackageStartupMessages(library(tidyverse)) 
```

    ## Warning: package 'dplyr' was built under R version 3.5.1

``` r
suppressPackageStartupMessages(library(gapminder))
suppressPackageStartupMessages(library(ggplot2))
suppressPackageStartupMessages(library(knitr))
suppressPackageStartupMessages(library(cowplot))
```

### Make a tibble with one row per year and columns for life expectancy for two or more countries.

``` r
countries <- c("Cambodia", "Bulgaria","Malawi", "Kuwait")

life_expectancy <- gapminder %>%
  filter(country %in% countries) %>%
  select(country, year, lifeExp) %>%
  group_by(year) %>%
  spread(key = country, value = lifeExp)
```

### Use knitr::kable() to make this table look pretty in your rendered homework.

``` r
kable(life_expectancy)
```

|  year|  Bulgaria|  Cambodia|  Kuwait|  Malawi|
|-----:|---------:|---------:|-------:|-------:|
|  1952|    59.600|    39.417|  55.565|  36.256|
|  1957|    66.610|    41.366|  58.033|  37.207|
|  1962|    69.510|    43.415|  60.470|  38.410|
|  1967|    70.420|    45.415|  64.624|  39.487|
|  1972|    70.900|    40.317|  67.712|  41.766|
|  1977|    70.810|    31.220|  69.343|  43.767|
|  1982|    71.080|    50.957|  71.309|  45.642|
|  1987|    71.340|    53.914|  74.174|  47.457|
|  1992|    71.190|    55.803|  75.190|  49.420|
|  1997|    70.320|    56.534|  76.156|  47.495|
|  2002|    72.140|    56.752|  76.904|  45.009|
|  2007|    73.005|    59.723|  77.588|  48.303|

### Take advantage of this new data shape to scatterplot life expectancy for one country against that of another.

``` r
CambodiaVsBulgaria <- life_expectancy %>%
  ggplot(aes(Cambodia, Bulgaria)) +
  geom_point() + 
  ggtitle("Life Expectancy") + 
  theme_gray()+
  labs(title="LifeExp-Cambodia_Vs_Bulgaria", x="life_expectancy of Cambodia", y="life_expectancy of Bulgaria")

KuwaitVsMalawi <- life_expectancy %>%
  ggplot(aes(Kuwait, Malawi)) +
  geom_point() + 
  ggtitle("Life Expectancy") + 
  theme_gray() +
  labs(title="LifeExp-Kuwait_Vs_Malawi ",x="life_expectancy of Kuwait", y="life_expectancy of Malawi")

plot_grid(CambodiaVsBulgaria,  KuwaitVsMalawi, nrow=1)
```

![](hw04-rasiimwe_files/figure-markdown_github/unnamed-chunk-4-1.png)

Join Prompts (join, merge, look up)
-----------------------------------

Overview: Problem - You have two data sources and you need info from both in one new data object. Solution: Perform a join, which borrows terminology from the database world, specifically SQL.

### Activity \#2 :Create your own cheatsheet

> Create your own cheatsheet patterned after [Jenny’s](http://stat545.com/bit001_dplyr-cheatsheet.html) but focused on something you care about more than comics! Inspirational examples: + Pets I have owned + breed + friendly vs. unfriendly + ??. Join to a table of pet breed, including variables for furry vs not furry, mammal true or false, etc. + Movies and studios…. + Athletes and teams…. You will likely need to iterate between your data prep and your joining to make your explorations comprehensive and interesting. For example, you will want a specific amount (or lack) of overlap between the two data.frames, in order to demonstrate all the different joins. You will want both the data frames to be as small as possible, while still retaining the expository value.

``` r
presidents <- tibble(
  name = c("Donald Trump", "Barack Obama", "George W. Bush", "Bill Clinton","George H. W. Bush", "Ronald Reagan", "Jimmy Carter","Gerald Ford", "Richard Nixon"),
  previous_office = c("Chairman of The Trump Organization", "U.S. Senator from Illinois", "Governor of Texas", "Governor of Arkansas","Vice President of the United States", "Governor of California", "Governor of Georgia","Vice President of the United States", "Vice President of the United States"),
  party=c("Republican", "Democratic", "Republican", "Democratic","Republican", "Republican", "Democratic","Republican", "Republican"),
  in_office=c(2007, 2009, 2001, 1993,1989, 1981, 1977,1974, 1969),
  out_office=c("Incumbent", 2017, 2009, 2001,1993, 1989, 1981,1977, 1974)
)
```

``` r
parties <- tibble(
  party=c("Republican", "Democratic", "Republican", "Democratic","Republican", "Republican", "Democratic","Republican", "Republican"),
  vice = c("Mike Pence", "Joe Biden", "Dick Cheney", "Al Gore","Dan Quayle", "George H. W. Bush", "Walter Mondale","Nelson Rockefeller", "Gerald Ford")
)
```
