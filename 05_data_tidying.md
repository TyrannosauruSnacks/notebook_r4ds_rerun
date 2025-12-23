# Data tidying
Max Hachemeister
2025-12-23

- [Prerequisites](#prerequisites)
- [Introduction](#introduction)
  - [Tidy data](#tidy-data)
    - [Exercises](#exercises)

# Prerequisites

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.1     ✔ stringr   1.5.2
    ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.1.0     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
theme_set(theme_light())
```

# Introduction

## Tidy data

Rules of tidy data:

1.  Each variable is a column; each column is a variable.
2.  Each observation is a row; each row is an observation.
3.  Each value is a cell; each cell is a single value.

### Exercises

#### 1. For each of the sample tables, describe what each observation and each column represents.

1.  In `table1` each observation represents the population and
    tuberculosis cases per year per country, while the column represent
    the variables countries, year, cases and population.

    ``` r
    table1
    ```

        # A tibble: 6 × 4
          country      year  cases population
          <chr>       <dbl>  <dbl>      <dbl>
        1 Afghanistan  1999    745   19987071
        2 Afghanistan  2000   2666   20595360
        3 Brazil       1999  37737  172006362
        4 Brazil       2000  80488  174504898
        5 China        1999 212258 1272915272
        6 China        2000 213766 1280428583

2.  In `table2` each observation represents either the number of cases
    or the population per year per country. Therefore the columns `type`
    represents two possible variables, namely cases or population.

    ``` r
    table2
    ```

        # A tibble: 12 × 4
           country      year type            count
           <chr>       <dbl> <chr>           <dbl>
         1 Afghanistan  1999 cases             745
         2 Afghanistan  1999 population   19987071
         3 Afghanistan  2000 cases            2666
         4 Afghanistan  2000 population   20595360
         5 Brazil       1999 cases           37737
         6 Brazil       1999 population  172006362
         7 Brazil       2000 cases           80488
         8 Brazil       2000 population  174504898
         9 China        1999 cases          212258
        10 China        1999 population 1272915272
        11 China        2000 cases          213766
        12 China        2000 population 1280428583

3.  In `table3` each observation represents the calculation of the
    tuberculosis rate per year per country. The columns `country` and
    `year` represent their literal variables, whereas `rate`, even
    though being a variable has two values in its cells.

    ``` r
    table3
    ```

        # A tibble: 6 × 3
          country      year rate             
          <chr>       <dbl> <chr>            
        1 Afghanistan  1999 745/19987071     
        2 Afghanistan  2000 2666/20595360    
        3 Brazil       1999 37737/172006362  
        4 Brazil       2000 80488/174504898  
        5 China        1999 212258/1272915272
        6 China        2000 213766/1280428583

#### 2. Sketch out the process you’d use to calculate the `rate` for `table2` and `table3`.

> You will need to perform four operations:
>
> 1.  Extract the number of TB cases per country per year.
> 2.  Extract the matching population per country per year.
> 3.  Divide cases by population, and multiply by 1000.
> 4.  Store back in the approriate place.
