STAT545 Homework 4: Data wrangling with data aggregation and data reshaping
================
Rebecca Asiimwe
2018-10-06

-   [STAT545 Homework 4: Data wrangling with data aggregation and data reshaping](#stat545-homework-4-data-wrangling-with-data-aggregation-and-data-reshaping)
    -   [The dplyr library as a major component of the data analysis ecosystem.](#the-dplyr-library-as-a-major-component-of-the-data-analysis-ecosystem.)
        -   [Loading required packages](#loading-required-packages)
    -   [Qn 1. Data Reshaping Prompt](#qn-1.-data-reshaping-prompt)
        -   [Activity \#2](#activity-2)
        -   [Make a tibble with one row per year and columns for life expectancy for two or more countries.](#make-a-tibble-with-one-row-per-year-and-columns-for-life-expectancy-for-two-or-more-countries.)
        -   [Use knitr::kable() to make this table look pretty in your rendered homework.](#use-knitrkable-to-make-this-table-look-pretty-in-your-rendered-homework.)
        -   [Take advantage of this new data shape to scatterplot life expectancy for one country against that of another.](#take-advantage-of-this-new-data-shape-to-scatterplot-life-expectancy-for-one-country-against-that-of-another.)
    -   [Qn 2. Join Prompt](#qn-2.-join-prompt)
        -   [Activity \#2 :Create your own cheatsheet](#activity-2-create-your-own-cheatsheet)
        -   [Introduction to dplyr's join functions](#introduction-to-dplyrs-join-functions)
        -   [Creating tibble 1](#creating-tibble-1)
        -   [Creating tibble 2](#creating-tibble-2)
    -   [Mutating joins:](#mutating-joins)
        -   [Left Join](#left-join)
        -   [Right Join](#right-join)
        -   [Inner Join](#inner-join)
        -   [Full Join](#full-join)
    -   [Filtering joins:](#filtering-joins)
        -   [Semi Join](#semi-join)
        -   [Anti Join](#anti-join)
    -   [Set Operations:](#set-operations)
        -   [Intersect](#intersect)
        -   [Union](#union)
        -   [setdiff](#setdiff)
    -   [Binding datasets:](#binding-datasets)
        -   [bind\_rows()](#bind_rows)
        -   [bind\_cols()](#bind_cols)
    -   [Supplementary Activity \#3](#supplementary-activity-3)
        -   [merge() function](#merge-function)
        -   [match() function](#match-function)
    -   [Sources to acknowledge:](#sources-to-acknowledge)

STAT545 Homework 4: Data wrangling with data aggregation and data reshaping
===========================================================================

Data analysis tasks comprise of 3 main components, **(1) Data Manipulation**, **(2) Data Cleaning** and **(3) Data Visualization**. It is estimated that 80% of data analysis tasks are inclined to data manipulation and cleaning attributed to the growth in data sources that need preparation for effective data analysis.

The dplyr library as a major component of the data analysis ecosystem.
----------------------------------------------------------------------

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/dplyr.png" width="500" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/dplyr.png)

As shown in the figure above, data manipulation, cleaning and visualisation are key components to the data analysis process. Herein, I present key R functions and their application in data manipulation operations towards supporting data analysis. I will focus on manipulating data using functions such as `gather()`, `spread()`, `mutating joins`, `filtering joins`, `set operations` and `biding` datasets.

### Loading required packages

First and foremost, I load all packages required for this assignment

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

Qn 1. Data Reshaping Prompt
---------------------------

Overview: Problem - You have data in one “shape” but you wish it were in another. Usually this is because the alternative shape is superior for presenting a table, making a figure, or doing aggregation and statistical analysis. Solution: Reshape your data. For simple reshaping, gather() and spread() from tidyr will suffice.

### Activity \#2

> Make a tibble with one row per year and columns for life expectancy for two or more countries. Use knitr::kable() to make this table look pretty in your rendered homework. Take advantage of this new data shape to scatterplot life expectancy for one country against that of another.

### Make a tibble with one row per year and columns for life expectancy for two or more countries.

``` r
countries <- c("Cambodia", "Bulgaria","Malawi", "Kuwait") #selecting countries for inclusion in my tibble

life_expectancy <- gapminder %>%
  filter(country %in% countries) %>% #filtering the gapminder dataset for the four countries in my vector countries
  select(country, year, lifeExp) %>% #selecting variable for inclusion in my tibble life_expectancy
  group_by(year) #choosing to group by year
  
kable(life_expectancy) #applying the kable function to render a neat/ nicely presented table
```

| country  |  year|  lifeExp|
|:---------|-----:|--------:|
| Bulgaria |  1952|   59.600|
| Bulgaria |  1957|   66.610|
| Bulgaria |  1962|   69.510|
| Bulgaria |  1967|   70.420|
| Bulgaria |  1972|   70.900|
| Bulgaria |  1977|   70.810|
| Bulgaria |  1982|   71.080|
| Bulgaria |  1987|   71.340|
| Bulgaria |  1992|   71.190|
| Bulgaria |  1997|   70.320|
| Bulgaria |  2002|   72.140|
| Bulgaria |  2007|   73.005|
| Cambodia |  1952|   39.417|
| Cambodia |  1957|   41.366|
| Cambodia |  1962|   43.415|
| Cambodia |  1967|   45.415|
| Cambodia |  1972|   40.317|
| Cambodia |  1977|   31.220|
| Cambodia |  1982|   50.957|
| Cambodia |  1987|   53.914|
| Cambodia |  1992|   55.803|
| Cambodia |  1997|   56.534|
| Cambodia |  2002|   56.752|
| Cambodia |  2007|   59.723|
| Kuwait   |  1952|   55.565|
| Kuwait   |  1957|   58.033|
| Kuwait   |  1962|   60.470|
| Kuwait   |  1967|   64.624|
| Kuwait   |  1972|   67.712|
| Kuwait   |  1977|   69.343|
| Kuwait   |  1982|   71.309|
| Kuwait   |  1987|   74.174|
| Kuwait   |  1992|   75.190|
| Kuwait   |  1997|   76.156|
| Kuwait   |  2002|   76.904|
| Kuwait   |  2007|   77.588|
| Malawi   |  1952|   36.256|
| Malawi   |  1957|   37.207|
| Malawi   |  1962|   38.410|
| Malawi   |  1967|   39.487|
| Malawi   |  1972|   41.766|
| Malawi   |  1977|   43.767|
| Malawi   |  1982|   45.642|
| Malawi   |  1987|   47.457|
| Malawi   |  1992|   49.420|
| Malawi   |  1997|   47.495|
| Malawi   |  2002|   45.009|
| Malawi   |  2007|   48.303|

As we can see, the tibble is in long format but what we would like to have is a tibble with one row per year and columns for life expectancy for two or more countries. To do this, I apply the spread function as shown below.

``` r
life_expectancy <- life_expectancy %>% 
  spread(key = country, value = lifeExp) #applying the spread function from dplyr to spread selected countries based on lifeExp (value) across the tibble columns
```

### Use knitr::kable() to make this table look pretty in your rendered homework.

Below I show a more tidy and usable tibble using the kable() function

``` r
kable(life_expectancy) #applying the kable function from knitr to render my tibble in a neat table form
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

As we can see above, our initial table that was in long format is now in a wide and more tidy form to support the creation of the scatter plot required in the subsequent section.

### Take advantage of this new data shape to scatterplot life expectancy for one country against that of another.

``` r
life_expectancy %>%
  ggplot(aes(Kuwait, Malawi)) + #specifying axis variables x=Kuwait and y=Malawi
  geom_point() + #specifying the type of plot prefered, in this case scatter plot
  ggtitle("Life Expectancy - Kuwait Vs Malawi") + #adding a title to my plot
  theme_gray()+ #setting plot theme
  labs(x="life_expectancy of Kuwait", y="life_expectancy of Malawi") #specifying x and y axis labels respectively
```

![](hw04-rasiimwe_files/figure-markdown_github/unnamed-chunk-5-1.png)

Above, we see a scatter plot that has been built from the new transformed tibble (life\_expectancy). At times the shapes that our data takes on may or may not support the kinds of analyses we need. In this case, we wouldn't have been able to create this plot or it would have been more challenging had the data stayed in the long format in which it was prior. This further goes to show for the need of data reshaping before further analysis is done. It also helps to know which data formats support which type of analysis. I have shown the usage of the spread function.

Qn 2. Join Prompt
-----------------

Overview: Problem - You have two data sources and you need info from both in one new data object. Solution: Perform a join, which borrows terminology from the database world, specifically SQL.

### Activity \#2 :Create your own cheatsheet

> Create your own cheatsheet patterned after [Jenny’s](http://stat545.com/bit001_dplyr-cheatsheet.html) but focused on something you care about more than comics! Inspirational examples: + Pets I have owned + breed + friendly vs. unfriendly + ??. Join to a table of pet breed, including variables for furry vs not furry, mammal true or false, etc. + Movies and studios…. + Athletes and teams…. You will likely need to iterate between your data prep and your joining to make your explorations comprehensive and interesting. For example, you will want a specific amount (or lack) of overlap between the two data.frames, in order to demonstrate all the different joins. You will want both the data frames to be as small as possible, while still retaining the expository value.

### Introduction to dplyr's join functions

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/join-venn.png" width="800" height="250"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/join-venn.png)

Before we dig deeper into the various join functions, I will create the tibbles required to explore the application of these functions

### Creating tibble 1

The idea and data used is derived from the wikipedia page of [preseidents of the United States](https://en.wikipedia.org/wiki/List_of_Presidents_of_the_United_States)

``` r
presidents <- tibble(
  name = c("Donald Trump", "Barack Obama", "George W. Bush", "Bill Clinton","George H. W. Bush", "Ronald Reagan", "Jimmy Carter","Gerald Ford", "Richard Nixon", "Rebecca Asiimwe"),
  previous_office = c("Chairman of The Trump Organization", "U.S. Senator from Illinois", "Governor of Texas", "Governor of Arkansas","Vice President of the United States", "Governor of California", "Governor of Georgia","Vice President of the United States", "Vice President of the United States", "Senior Bioinformatician BCCRC"),
  party=c("Republican", "Democratic", "Republican", "Democratic","Republican", "Republican", "Democratic","Republican", "Republican","Independent"),
  vice = c("Mike Pence", "Joe Biden", "Dick Cheney", "Al Gore","Dan Quayle", "George H. W. Bush", "Walter Mondale","Nelson Rockefeller", "Gerald Ford","Unknown"),
  in_office=c(2007, 2009, 2001, 1993,1989, 1981, 1977,1974, 1969,1900),
  out_office=c(2021, 2017, 2009, 2001,1993, 1989, 1981,1977, 1974,1969)
) # the variables captured in this tibble are name, previous_office, party, vice, in_office and out_office

kable(presidents)
```

| name              | previous\_office                    | party       | vice               |  in\_office|  out\_office|
|:------------------|:------------------------------------|:------------|:-------------------|-----------:|------------:|
| Donald Trump      | Chairman of The Trump Organization  | Republican  | Mike Pence         |        2007|         2021|
| Barack Obama      | U.S. Senator from Illinois          | Democratic  | Joe Biden          |        2009|         2017|
| George W. Bush    | Governor of Texas                   | Republican  | Dick Cheney        |        2001|         2009|
| Bill Clinton      | Governor of Arkansas                | Democratic  | Al Gore            |        1993|         2001|
| George H. W. Bush | Vice President of the United States | Republican  | Dan Quayle         |        1989|         1993|
| Ronald Reagan     | Governor of California              | Republican  | George H. W. Bush  |        1981|         1989|
| Jimmy Carter      | Governor of Georgia                 | Democratic  | Walter Mondale     |        1977|         1981|
| Gerald Ford       | Vice President of the United States | Republican  | Nelson Rockefeller |        1974|         1977|
| Richard Nixon     | Vice President of the United States | Republican  | Gerald Ford        |        1969|         1974|
| Rebecca Asiimwe   | Senior Bioinformatician BCCRC       | Independent | Unknown            |        1900|         1969|

The first tibble I have created above is that of presidents of the United states from 1969 to date. For the fun of it, I also include a fake president to help point out salient dimensions when working with table joins.

### Creating tibble 2

``` r
parties <- tibble(
  party=c("Republican", "Democratic", "Other"),
  lead=c("Ronna McDaniel", "Tom Perez", "Party Bandit")
) # the variable captured in this tibble are party and lead, that is for the political party and the party lead respectively.

kable(parties) 
```

| party      | lead           |
|:-----------|:---------------|
| Republican | Ronna McDaniel |
| Democratic | Tom Perez      |
| Other      | Party Bandit   |

The second tibble above shows the unique political parties in the United States and the respective party leads.

Mutating joins:
---------------

Mutating joins combine variables from two tibbles. They do this by adding variables from one table to matching observations in another table.

**Below are the mutating functions we shall explore:**

1.  `left_join`
2.  `right_join`
3.  `inner_join`
4.  `full_join`

By default, all joins will be by party. The general syntax for writing this by specifying the variable to join on is join\_function(x, t, by="variable"), however, since all my joins will be by party, my lines of code will conform to lines similar to `left_join(presidents, parties)`, this is also so because the variable party is the primary key in the parties tibble while it is a foreign key in the presidents table - this is also what helps R select it as the variable to join the tibbles on.

### Left Join

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/left-join.gif" width="300" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/left-join.gif)

**Basic syntax:** `left_join(x, y)`

**Function:** keep all x, drop unmatched y:- Return all rows from ‘x’, and all columns from ‘x’ and ‘y’. Rows in ‘x’ with no match in ‘y’ will have ‘NA’ values in the new columns. If there are multiple matches between ‘x’ and ‘y’, all combinations of the matches are returned.

#### left\_join on presidents & parties

``` r
left_join(presidents, parties) %>% 
  kable()
```

    ## Joining, by = "party"

| name              | previous\_office                    | party       | vice               |  in\_office|  out\_office| lead           |
|:------------------|:------------------------------------|:------------|:-------------------|-----------:|------------:|:---------------|
| Donald Trump      | Chairman of The Trump Organization  | Republican  | Mike Pence         |        2007|         2021| Ronna McDaniel |
| Barack Obama      | U.S. Senator from Illinois          | Democratic  | Joe Biden          |        2009|         2017| Tom Perez      |
| George W. Bush    | Governor of Texas                   | Republican  | Dick Cheney        |        2001|         2009| Ronna McDaniel |
| Bill Clinton      | Governor of Arkansas                | Democratic  | Al Gore            |        1993|         2001| Tom Perez      |
| George H. W. Bush | Vice President of the United States | Republican  | Dan Quayle         |        1989|         1993| Ronna McDaniel |
| Ronald Reagan     | Governor of California              | Republican  | George H. W. Bush  |        1981|         1989| Ronna McDaniel |
| Jimmy Carter      | Governor of Georgia                 | Democratic  | Walter Mondale     |        1977|         1981| Tom Perez      |
| Gerald Ford       | Vice President of the United States | Republican  | Nelson Rockefeller |        1974|         1977| Ronna McDaniel |
| Richard Nixon     | Vice President of the United States | Republican  | Gerald Ford        |        1969|         1974| Ronna McDaniel |
| Rebecca Asiimwe   | Senior Bioinformatician BCCRC       | Independent | Unknown            |        1900|         1969| NA             |

In the table above table, we see the effect of a `left_join(presidents, parties)`. We see that in the output we have maintained the presidents tibble concatenated d with an additional variable `lead` from the parties tibble. In this case, the party lead from the parties tibble is being matched with each row in the presidents table based on the party variable. The parties are unique and since president Rebecca Asiimwe's party does not appear in the parties tibble, the lead is replaced with NA in the output table above. Let's try a left\_join on parties and presidents.

#### left\_join on parties & presidents

``` r
left_join(parties, presidents) %>% 
  kable()
```

    ## Joining, by = "party"

| party      | lead           | name              | previous\_office                    | vice               |  in\_office|  out\_office|
|:-----------|:---------------|:------------------|:------------------------------------|:-------------------|-----------:|------------:|
| Republican | Ronna McDaniel | Donald Trump      | Chairman of The Trump Organization  | Mike Pence         |        2007|         2021|
| Republican | Ronna McDaniel | George W. Bush    | Governor of Texas                   | Dick Cheney        |        2001|         2009|
| Republican | Ronna McDaniel | George H. W. Bush | Vice President of the United States | Dan Quayle         |        1989|         1993|
| Republican | Ronna McDaniel | Ronald Reagan     | Governor of California              | George H. W. Bush  |        1981|         1989|
| Republican | Ronna McDaniel | Gerald Ford       | Vice President of the United States | Nelson Rockefeller |        1974|         1977|
| Republican | Ronna McDaniel | Richard Nixon     | Vice President of the United States | Gerald Ford        |        1969|         1974|
| Democratic | Tom Perez      | Barack Obama      | U.S. Senator from Illinois          | Joe Biden          |        2009|         2017|
| Democratic | Tom Perez      | Bill Clinton      | Governor of Arkansas                | Al Gore            |        1993|         2001|
| Democratic | Tom Perez      | Jimmy Carter      | Governor of Georgia                 | Walter Mondale     |        1977|         1981|
| Other      | Party Bandit   | NA                | NA                                  | NA                 |          NA|           NA|

What happened here!! .... the output table looks different and yet more interesting. When we look closely we see that Rebecca Asiimwe has been dropped in this join operation in which we are doing a left join of presidents on parties and joining by the party variable from the parties table. The drop of this president is mainly because, her party does not appear in the parties table. In a nutshell what is going on here is that the left join on (parties, presidents) has maintained all variables from the parties tibble plus matching rows from the presidents table - this also explains why the party called "Other" that has no matching row in the presidents table is being maintained.

### Right Join

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/right-join.gif" width="300" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/right-join.gif)

**Basic syntax:** `right_join(x, y)`

**Function**: keep all y, drop unmatched x:- return all rows from ‘y’, and all columns from ‘x’ and y. Rows in ‘y’ with no match in ‘x’ will have ‘NA’ values in the new columns. If there are multiple matches between ‘x’ and ‘y’, all combinations of the matches are returned

#### right\_join on presidents & parties

``` r
right_join(presidents, parties) %>% 
  kable()
```

    ## Joining, by = "party"

| name              | previous\_office                    | party      | vice               |  in\_office|  out\_office| lead           |
|:------------------|:------------------------------------|:-----------|:-------------------|-----------:|------------:|:---------------|
| Donald Trump      | Chairman of The Trump Organization  | Republican | Mike Pence         |        2007|         2021| Ronna McDaniel |
| George W. Bush    | Governor of Texas                   | Republican | Dick Cheney        |        2001|         2009| Ronna McDaniel |
| George H. W. Bush | Vice President of the United States | Republican | Dan Quayle         |        1989|         1993| Ronna McDaniel |
| Ronald Reagan     | Governor of California              | Republican | George H. W. Bush  |        1981|         1989| Ronna McDaniel |
| Gerald Ford       | Vice President of the United States | Republican | Nelson Rockefeller |        1974|         1977| Ronna McDaniel |
| Richard Nixon     | Vice President of the United States | Republican | Gerald Ford        |        1969|         1974| Ronna McDaniel |
| Barack Obama      | U.S. Senator from Illinois          | Democratic | Joe Biden          |        2009|         2017| Tom Perez      |
| Bill Clinton      | Governor of Arkansas                | Democratic | Al Gore            |        1993|         2001| Tom Perez      |
| Jimmy Carter      | Governor of Georgia                 | Democratic | Walter Mondale     |        1977|         1981| Tom Perez      |
| NA                | NA                                  | Other      | NA                 |          NA|           NA| Party Bandit   |

The right join on (presidents, parties) maintains all tupples/rows from the presidents tibble for which there are matching rows in the parties table. As we can see, this includes all the rows from the parties and only those from the presidents tibble that match. President Rebecca Asiimwe was dropped by the right join since their party does not match any of those in the parties tibble.

#### right\_join on parties & presidents

``` r
right_join(parties, presidents) %>% 
  kable()
```

    ## Joining, by = "party"

| party       | lead           | name              | previous\_office                    | vice               |  in\_office|  out\_office|
|:------------|:---------------|:------------------|:------------------------------------|:-------------------|-----------:|------------:|
| Republican  | Ronna McDaniel | Donald Trump      | Chairman of The Trump Organization  | Mike Pence         |        2007|         2021|
| Democratic  | Tom Perez      | Barack Obama      | U.S. Senator from Illinois          | Joe Biden          |        2009|         2017|
| Republican  | Ronna McDaniel | George W. Bush    | Governor of Texas                   | Dick Cheney        |        2001|         2009|
| Democratic  | Tom Perez      | Bill Clinton      | Governor of Arkansas                | Al Gore            |        1993|         2001|
| Republican  | Ronna McDaniel | George H. W. Bush | Vice President of the United States | Dan Quayle         |        1989|         1993|
| Republican  | Ronna McDaniel | Ronald Reagan     | Governor of California              | George H. W. Bush  |        1981|         1989|
| Democratic  | Tom Perez      | Jimmy Carter      | Governor of Georgia                 | Walter Mondale     |        1977|         1981|
| Republican  | Ronna McDaniel | Gerald Ford       | Vice President of the United States | Nelson Rockefeller |        1974|         1977|
| Republican  | Ronna McDaniel | Richard Nixon     | Vice President of the United States | Gerald Ford        |        1969|         1974|
| Independent | NA             | Rebecca Asiimwe   | Senior Bioinformatician BCCRC       | Unknown            |        1900|         1969|

Here we see that the right\_join on (parties, presidents) maintains all rows from the parties tibble and concatenated s them with all rows from the presidents tibble. Unlike the left joins on (parties, presidents) that dropped non-matching rows as seen above, here non matching rows are included as well with NAs used for cases of missing values.

### Inner Join

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/inner-join.gif" width="300" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/inner-join.gif)

**Basic syntax:** `inner_join(x, y)`

**Function:** keep only matching:- Return all rows from x where there are matching values in y, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned.

#### inner\_join on presidents & parties

``` r
inner_join(presidents, parties) %>% 
  kable()
```

    ## Joining, by = "party"

| name              | previous\_office                    | party      | vice               |  in\_office|  out\_office| lead           |
|:------------------|:------------------------------------|:-----------|:-------------------|-----------:|------------:|:---------------|
| Donald Trump      | Chairman of The Trump Organization  | Republican | Mike Pence         |        2007|         2021| Ronna McDaniel |
| Barack Obama      | U.S. Senator from Illinois          | Democratic | Joe Biden          |        2009|         2017| Tom Perez      |
| George W. Bush    | Governor of Texas                   | Republican | Dick Cheney        |        2001|         2009| Ronna McDaniel |
| Bill Clinton      | Governor of Arkansas                | Democratic | Al Gore            |        1993|         2001| Tom Perez      |
| George H. W. Bush | Vice President of the United States | Republican | Dan Quayle         |        1989|         1993| Ronna McDaniel |
| Ronald Reagan     | Governor of California              | Republican | George H. W. Bush  |        1981|         1989| Ronna McDaniel |
| Jimmy Carter      | Governor of Georgia                 | Democratic | Walter Mondale     |        1977|         1981| Tom Perez      |
| Gerald Ford       | Vice President of the United States | Republican | Nelson Rockefeller |        1974|         1977| Ronna McDaniel |
| Richard Nixon     | Vice President of the United States | Republican | Gerald Ford        |        1969|         1974| Ronna McDaniel |

The output of the inner join as see above results in the inclusion of all the rows of the parties tibble to match rows in presidents tibble - matching is done by party. As we can see, president Rebecca Asiimwe was dropped by the inner join because the party to which she belongs (Independent) does not match any of those in the parties tibble/ does not exist in the parties table.

#### inner\_join on parties & presidents

``` r
inner_join(parties, presidents) %>% 
  kable()
```

    ## Joining, by = "party"

| party      | lead           | name              | previous\_office                    | vice               |  in\_office|  out\_office|
|:-----------|:---------------|:------------------|:------------------------------------|:-------------------|-----------:|------------:|
| Republican | Ronna McDaniel | Donald Trump      | Chairman of The Trump Organization  | Mike Pence         |        2007|         2021|
| Republican | Ronna McDaniel | George W. Bush    | Governor of Texas                   | Dick Cheney        |        2001|         2009|
| Republican | Ronna McDaniel | George H. W. Bush | Vice President of the United States | Dan Quayle         |        1989|         1993|
| Republican | Ronna McDaniel | Ronald Reagan     | Governor of California              | George H. W. Bush  |        1981|         1989|
| Republican | Ronna McDaniel | Gerald Ford       | Vice President of the United States | Nelson Rockefeller |        1974|         1977|
| Republican | Ronna McDaniel | Richard Nixon     | Vice President of the United States | Gerald Ford        |        1969|         1974|
| Democratic | Tom Perez      | Barack Obama      | U.S. Senator from Illinois          | Joe Biden          |        2009|         2017|
| Democratic | Tom Perez      | Bill Clinton      | Governor of Arkansas                | Al Gore            |        1993|         2001|
| Democratic | Tom Perez      | Jimmy Carter      | Governor of Georgia                 | Walter Mondale     |        1977|         1981|

An inner join of parties and presidents on variable party results in every party in the parties table being matched with every president in the presidents tibble. We can also see that every party appears multiple times in the above output table and appearing once for each matching row. We can still see that president Rebecca Asiimwe was dropped from the result of the join because her party does not appear in the parties table and therefore wouldn't match any row in the presidents table.

### Full Join

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/full-join.gif" width="300" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/full-join.gif)

**Basic syntax:** `full_join(x, y)`

**Function:**Return all rows and all columns from both x and y. Where there are not matching values, returns NA for the one missing.

#### full\_join on presidents & parties

``` r
full_join(presidents, parties) %>% 
  kable()
```

    ## Joining, by = "party"

| name              | previous\_office                    | party       | vice               |  in\_office|  out\_office| lead           |
|:------------------|:------------------------------------|:------------|:-------------------|-----------:|------------:|:---------------|
| Donald Trump      | Chairman of The Trump Organization  | Republican  | Mike Pence         |        2007|         2021| Ronna McDaniel |
| Barack Obama      | U.S. Senator from Illinois          | Democratic  | Joe Biden          |        2009|         2017| Tom Perez      |
| George W. Bush    | Governor of Texas                   | Republican  | Dick Cheney        |        2001|         2009| Ronna McDaniel |
| Bill Clinton      | Governor of Arkansas                | Democratic  | Al Gore            |        1993|         2001| Tom Perez      |
| George H. W. Bush | Vice President of the United States | Republican  | Dan Quayle         |        1989|         1993| Ronna McDaniel |
| Ronald Reagan     | Governor of California              | Republican  | George H. W. Bush  |        1981|         1989| Ronna McDaniel |
| Jimmy Carter      | Governor of Georgia                 | Democratic  | Walter Mondale     |        1977|         1981| Tom Perez      |
| Gerald Ford       | Vice President of the United States | Republican  | Nelson Rockefeller |        1974|         1977| Ronna McDaniel |
| Richard Nixon     | Vice President of the United States | Republican  | Gerald Ford        |        1969|         1974| Ronna McDaniel |
| Rebecca Asiimwe   | Senior Bioinformatician BCCRC       | Independent | Unknown            |        1900|         1969| NA             |
| NA                | NA                                  | Other       | NA                 |          NA|           NA| Party Bandit   |

The full join function works in a similar fashion as that of taking the union of two sets. From the output table of this join, we get all variables of presidents plus all variables from the lead table, however, we should note that the unique variable on which the join is being made (where the two table intersect=party) is not duplicated, that is we do not have two variables called party(one from presidents and another from parties) in the above output. With a full join, all rows without matching values from either table carry NAs in the variables found only in the other table.

#### full\_join on parties % presidents

``` r
full_join(parties, presidents) %>% 
  kable()
```

    ## Joining, by = "party"

| party       | lead           | name              | previous\_office                    | vice               |  in\_office|  out\_office|
|:------------|:---------------|:------------------|:------------------------------------|:-------------------|-----------:|------------:|
| Republican  | Ronna McDaniel | Donald Trump      | Chairman of The Trump Organization  | Mike Pence         |        2007|         2021|
| Republican  | Ronna McDaniel | George W. Bush    | Governor of Texas                   | Dick Cheney        |        2001|         2009|
| Republican  | Ronna McDaniel | George H. W. Bush | Vice President of the United States | Dan Quayle         |        1989|         1993|
| Republican  | Ronna McDaniel | Ronald Reagan     | Governor of California              | George H. W. Bush  |        1981|         1989|
| Republican  | Ronna McDaniel | Gerald Ford       | Vice President of the United States | Nelson Rockefeller |        1974|         1977|
| Republican  | Ronna McDaniel | Richard Nixon     | Vice President of the United States | Gerald Ford        |        1969|         1974|
| Democratic  | Tom Perez      | Barack Obama      | U.S. Senator from Illinois          | Joe Biden          |        2009|         2017|
| Democratic  | Tom Perez      | Bill Clinton      | Governor of Arkansas                | Al Gore            |        1993|         2001|
| Democratic  | Tom Perez      | Jimmy Carter      | Governor of Georgia                 | Walter Mondale     |        1977|         1981|
| Other       | Party Bandit   | NA                | NA                                  | NA                 |          NA|           NA|
| Independent | NA             | Rebecca Asiimwe   | Senior Bioinformatician BCCRC       | Unknown            |        1900|         1969|

In the above table we have a similar output to that of a full join on presidents and parties except for the fact that joining is done on the parties table, (inner vs outer / left vs right table in join). And we can also see that variables from the presidents table get appended to the party table, however, we see the contrary in the full join on presidents and parties since we are joining the parties tibble to the presidents tibble.

Filtering joins:
----------------

Filtering joins retain observations in one table based on whether or not they match the observations in another table

**Below are the filtering functions we shall explore:**

1.  `semi_join`
2.  `anti_join`

### Semi Join

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/semi-join.gif" width="300" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/semi-join.gif)

**Basic syntax:** `semi_join(x, y)`

**Function:** return rows from x where there are matching values in y:- Return all rows from x where there are matching values in y, keeping just columns from x. A semi join differs from an inner join because an inner join will return one row of x for each matching row of y, where a semi join will never duplicate rows of x.

#### semi\_join on presidents & parties

``` r
semi_join(presidents, parties) %>% 
  kable()
```

    ## Joining, by = "party"

| name              | previous\_office                    | party      | vice               |  in\_office|  out\_office|
|:------------------|:------------------------------------|:-----------|:-------------------|-----------:|------------:|
| Donald Trump      | Chairman of The Trump Organization  | Republican | Mike Pence         |        2007|         2021|
| Barack Obama      | U.S. Senator from Illinois          | Democratic | Joe Biden          |        2009|         2017|
| George W. Bush    | Governor of Texas                   | Republican | Dick Cheney        |        2001|         2009|
| Bill Clinton      | Governor of Arkansas                | Democratic | Al Gore            |        1993|         2001|
| George H. W. Bush | Vice President of the United States | Republican | Dan Quayle         |        1989|         1993|
| Ronald Reagan     | Governor of California              | Republican | George H. W. Bush  |        1981|         1989|
| Jimmy Carter      | Governor of Georgia                 | Democratic | Walter Mondale     |        1977|         1981|
| Gerald Ford       | Vice President of the United States | Republican | Nelson Rockefeller |        1974|         1977|
| Richard Nixon     | Vice President of the United States | Republican | Gerald Ford        |        1969|         1974|

Here we get a similar result as that got from the inner\_join(presidents, parties) however, we only maintain variables in the presidents tibble.

#### semi\_join on parties & presidents

``` r
semi_join(parties, presidents) %>% 
  kable()
```

    ## Joining, by = "party"

| party      | lead           |
|:-----------|:---------------|
| Republican | Ronna McDaniel |
| Democratic | Tom Perez      |

From the above output of the semi join, we can see that a semi join of presidents on parties provides results that are similar to our parties tibble. In the example tibbles I created, all observations in the parties table appear in the presidents table. We also see here that the party named "Other" for which there are no matching rows in the presidents tibble has been dropped by the semi join.

### Anti Join

[<img align ="center" src="https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/anti-join.gif" width="300" height="300"/>](https://github.com/STAT545-UBC-students/hw04-rasiimwe/blob/master/plugins/anti-join.gif)

**Basic syntax:** `anti_join(x, y)`

**Function:** Return all rows from x where there are no matching values in y, keeping just columns from x.

#### anti\_join on presidents & parties

``` r
anti_join(presidents, parties) %>% 
  kable()
```

    ## Joining, by = "party"

| name            | previous\_office              | party       | vice    |  in\_office|  out\_office|
|:----------------|:------------------------------|:------------|:--------|-----------:|------------:|
| Rebecca Asiimwe | Senior Bioinformatician BCCRC | Independent | Unknown |        1900|         1969|

From the above output table, we can see that the operation of the anti join filters and returns rows in the presidents table that do not match values in the parties tibble. Here president Rebecca Asiimwe whose party does not appear in the parties table has been returned as the output from the anti join.

#### anti\_join on parties & presidents

``` r
anti_join(parties, presidents) %>% 
  kable()
```

    ## Joining, by = "party"

| party | lead         |
|:------|:-------------|
| Other | Party Bandit |

The anti\_join(parties, presidents) now works in the opposite direction to the anti\_join(presidents, parties) by returning rows in the parties table that do not match values in the presidents tibble. Here the party called "Other" that does not appear in the presidents table has been returned as the result of the anti\_join(parties, presidents)

Set Operations:
---------------

In this section, I explore set operations and how we can apply them in our data manipulation tasks. To allow for the exploration of set operations, I will create two more datasets of professors at UBC, in the Bioinformatics and Statistics Departments.

``` r
#creating Bioinformatics_profs tibble
Bioinformatics_profs <- tibble(
  name=c("Sara Mostafavi", "Sohrab Shah", "Charmaine Dean", "Steve Jones", "Jinko Graham", "Brad McNeney","Fiona Brinkman"),
  course=c("STAT543","BIOF55","STAT542","BIOF56","STAT541","STAT540","BIOF57")
)
Bioinformatics_profs %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Sara Mostafavi | STAT543 |
| Sohrab Shah    | BIOF55  |
| Charmaine Dean | STAT542 |
| Steve Jones    | BIOF56  |
| Jinko Graham   | STAT541 |
| Brad McNeney   | STAT540 |
| Fiona Brinkman | BIOF57  |

``` r
#creating STAT_profs tibble

STAT_profs <- tibble(
  name=c("Vincenzo Coia","Jennifer Bryan", "Sara Mostafavi", "Charmaine Dean","Jinko Graham","Brad McNeney", "Suborna Ahmed"),
  course=c("STAT545","STAT544","STAT543","STAT542","STAT541","STAT540","STAT514")
)
STAT_profs %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Vincenzo Coia  | STAT545 |
| Jennifer Bryan | STAT544 |
| Sara Mostafavi | STAT543 |
| Charmaine Dean | STAT542 |
| Jinko Graham   | STAT541 |
| Brad McNeney   | STAT540 |
| Suborna Ahmed  | STAT514 |

**Below are the set operations we shall explore:**

1.  `intersect()`
2.  `union()`
3.  `setdiff()`

### Intersect

**Basic syntax:** `intersect(x,y)`

**Function:** The intersect function returns rows that appear in both x and y

``` r
intersect(Bioinformatics_profs, STAT_profs) %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Sara Mostafavi | STAT543 |
| Charmaine Dean | STAT542 |
| Jinko Graham   | STAT541 |
| Brad McNeney   | STAT540 |

The purpose of the intersect function is to return rows that appear in both tibbles. In the above output table we see professors that are both in the Statistics and Bioinformatics programs. If there were no observations that appear in both tables, R would throw back the error message: "`Error in intersect_data_frame(x, y) : not compatible: - Cols in y but not x:`lead`. - Cols in x but not y:`out\_office`,`vice`,`in\_office`,`previous\_office`,`name`.`"

### Union

**Basic syntax:** `union(x,y)`

**Function:** The union function returns rows that appear in either or both x and y

``` r
union(STAT_profs, Bioinformatics_profs) %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Fiona Brinkman | BIOF57  |
| Steve Jones    | BIOF56  |
| Sohrab Shah    | BIOF55  |
| Suborna Ahmed  | STAT514 |
| Brad McNeney   | STAT540 |
| Jinko Graham   | STAT541 |
| Charmaine Dean | STAT542 |
| Sara Mostafavi | STAT543 |
| Jennifer Bryan | STAT544 |
| Vincenzo Coia  | STAT545 |

From the above output we can see that the union set of all rows in the STAT\_profs and Bioinformatics\_profs tibles are returned. Just like the union of sets, intersecting observations are not duplciated.

### setdiff

**Basic syntax:** `setdiff(x,y)`

**Function:** The setdiff function returns rows that appear in x but not y.

#### setdiff(Bioinformatics\_profs, STAT\_profs)

``` r
setdiff(Bioinformatics_profs, STAT_profs) %>% 
  kable
```

| name           | course |
|:---------------|:-------|
| Sohrab Shah    | BIOF55 |
| Steve Jones    | BIOF56 |
| Fiona Brinkman | BIOF57 |

From the table above generated from the setdiff(Bioinformatics\_profs, STAT\_profs), we can see that all rows that appear in the Bioinformatics\_profs tibble but not in the STAT\_profs tibble are returned. Let's also try setdiff( STAT\_profs, Bioinformatics\_profs) and see what we get!

#### \#\#\#\# setdiff( STAT\_profs, Bioinformatics\_profs)

``` r
setdiff(STAT_profs, Bioinformatics_profs) %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Vincenzo Coia  | STAT545 |
| Jennifer Bryan | STAT544 |
| Suborna Ahmed  | STAT514 |

Nice! this is the output I expected. The setdiff(STAT\_profs, Bioinformatics\_profs) returns rows in the STAT\_profs tible that are not in the Bioinformatics\_profs tibble. More like (STAT\_profs - Bioinformatics\_profs)

Binding datasets:
-----------------

Many a times we have the need to concatenated tibbles/ dataframes - to combine observations from multiple tables into one. To achive this, we apply the bind\_rows or bind\_cols functions as will be explored below.

**Below are the dataset binding functions we shall explore:**

1.  `bind_rows()`
2.  `bind_cols()`

### bind\_rows()

``` r
#bind_rows is synonymous to base R's rbind()
bind_rows(STAT_profs, Bioinformatics_profs) %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Vincenzo Coia  | STAT545 |
| Jennifer Bryan | STAT544 |
| Sara Mostafavi | STAT543 |
| Charmaine Dean | STAT542 |
| Jinko Graham   | STAT541 |
| Brad McNeney   | STAT540 |
| Suborna Ahmed  | STAT514 |
| Sara Mostafavi | STAT543 |
| Sohrab Shah    | BIOF55  |
| Charmaine Dean | STAT542 |
| Steve Jones    | BIOF56  |
| Jinko Graham   | STAT541 |
| Brad McNeney   | STAT540 |
| Fiona Brinkman | BIOF57  |

From the above output we see that all rows from Bioinformatics\_profs have been bound to the rows from STAT\_profs. Notice that the output contains duplicates. For example, we see Sarah Mostavi, appearing twice with the first observation from the STAT\_profs tibble and the second observation from the Bioinformatics\_profs tibble. Next, let's see what bind\_cols() returns.

### bind\_cols()

``` r
#bind_cols is synonymous to base R's cbind()
bind_cols(STAT_profs, Bioinformatics_profs) %>% 
  kable
```

| name           | course  | name1          | course1 |
|:---------------|:--------|:---------------|:--------|
| Vincenzo Coia  | STAT545 | Sara Mostafavi | STAT543 |
| Jennifer Bryan | STAT544 | Sohrab Shah    | BIOF55  |
| Sara Mostafavi | STAT543 | Charmaine Dean | STAT542 |
| Charmaine Dean | STAT542 | Steve Jones    | BIOF56  |
| Jinko Graham   | STAT541 | Jinko Graham   | STAT541 |
| Brad McNeney   | STAT540 | Brad McNeney   | STAT540 |
| Suborna Ahmed  | STAT514 | Fiona Brinkman | BIOF57  |

We still see dublicates maintained however, unlike the output we saw in bind\_rows(), here we see binding by columns. This is a vertical bind where the initial STAT\_profs tibble that had two columns now has 4. To avoid duplication of names, notice that R has smartly added a distinguising nubmer(1) to distiguish name from name1 as the incoming variables from the Bioinformatics\_profs tibble have similar names to those of the STAT\_profs tibble.

Supplementary Activity \#3
--------------------------

> This is really an optional add-on to either of the previous activities. Explore the base R function merge(), which also does joins. Compare and contrast with dplyr joins. Explore the base R function match(), which is related to joins and merges, but is really more of a “table lookup”. Compare and contrast with a true join/merge.

### merge() function

**Basic syntax:** `merge(x,y)`

**Function:** The rows in two data frames that match on the specified columns are extracted, and joined together. If there is more than one match, all possible matches contribute one row each.

``` r
merge(Bioinformatics_profs, STAT_profs) %>% 
  kable
```

| name           | course  |
|:---------------|:--------|
| Brad McNeney   | STAT540 |
| Charmaine Dean | STAT542 |
| Jinko Graham   | STAT541 |
| Sara Mostafavi | STAT543 |

From the above output, we see that the merge funtion has merged the two tibbles on observations that appear in both tibbles. The merge function gives us a similar result to that seen when we run intersec(Bioinformatics\_profs, STAT\_profs). Let's also look into the match() function and see the results it returns.

### match() function

**Function:** The match function returns a vector of the positions of (first) matches of its first argument in its second.

``` r
name=c("Sara Mostafavi", "Sohrab Shah", "Charmaine Dean", "Steve Jones", "Jinko Graham", "Brad McNeney","Fiona Brinkman")
name2=c("Vincenzo Coia","Jennifer Bryan", "Sara Mostafavi", "Charmaine Dean","Jinko Graham","Brad McNeney", "Suborna Ahmed")
match(name, name2)
```

    ## [1]  3 NA  4 NA  5  6 NA

When we run the match function on the two vectors name and name2, the function works by returning positions in name2 that have a matching value in name. For example, the first value to assess is "Sara Mostafavi", a look up is done on the second vector. We see that "Sara Mostafavi" isin position 3 of the name2 vector so position 3 is returned. The next value to look up is "Sohrab Shah", a lookup in name2 does not find any value matching "Sohrab Shah", so the function returns NA, while "Charmaine Dean" in name appears in position 4 of name2. We can also look for a specific value as shown below:

``` r
match("Sara Mostafavi", name2) #match Sara Mostafavi to values in name2 and if found return Sara Mostafavi's position in name2
```

    ## [1] 3

The obove code snipped matches the value Sara Mostafavi to values in the name2 vector, if found, the position with the matching value is returned, in this case position 3 is returned.

``` r
match("Sohrab Shah", name2)#match Sohrab Shah to values in name2 and if found return Sohrab Shah's position in name2
```

    ## [1] NA

Similarily, the above code snipped looks for matching values of Sohrab Shah in the name2 vector. The returned NA implies that there is no occurance of Sohrab Shah in the name2 vector.

``` r
"Sara Mostafavi" %in% name2
```

    ## [1] TRUE

We can also use the above code snipped to check whether a value exists in a vector. In this case, either TRUE or FALSE will be returned.

------------------------------------------------------------------------

Sources to acknowledge:
-----------------------

[STAT545 Class notes and exercises by Rashedul Islam](https://github.com/rasiimwe/STAT545_participation/blob/master/cm010/cm010-exercise.md)

[Jenny Bryan's Cheatsheet for dplyr join functions](http://stat545.com/bit001_dplyr-cheatsheet.html)

[Garrick Aden-Buie's tidy verbs](https://github.com/gadenbuie/tidyexplain#readme) for gifs used

[R-bloggers](https://www.r-bloggers.com/express-intro-to-dplyr/)
