# Data tidying
Max Hachemeister
2025-12-24

- [Prerequisites](#prerequisites)
- [Introduction](#introduction)
  - [Tidy data](#tidy-data)
    - [Exercises](#exercises)
  - [Lengthening data](#lengthening-data)
    - [How does pivoting work?](#how-does-pivoting-work)
    - [Many variables in columns
      names](#many-variables-in-columns-names)
    - [Data an variable names in the column
      headers](#data-an-variable-names-in-the-column-headers)

# Prerequisites

``` r
library(tidyverse)
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

1.  To calculate the `rate` from `table2` I would have to take the
    following steps:

    - Create separate columns for `cases` and `population`.
    - Move the values accordingly.
    - Mutate a `rate` column with “`cases` / `population`”.

2.  To do get the `rate` from `table2` the following steps occur to me:

    - Mutate a `rate_result` column from the two values in the `rate`
      column.
      - Like “The number before ‘/’ divided by the number after ‘/’”.
    - Basically it comes down to separating the two numbers before and
      after the `/`.

## Lengthening data

- The most common task in tidying original data.

- Used to convert many columns that are actually the same variable into
  two columns.

  - One being the common variable in which the old columns are now the
    values, and the other inheriting the values that were in the
    original columns.

- So You are looking at four things in a sense.

- Take note that within `pivot_longer()` the names for the *new* columns
  have to be quoted, aka. strings, as they do not yet refer to existing
  objects, like it would be case for `mutate()`.

- Use `parse_number()` to extract the first number after a prefix string
  like `wk1`, `wk2`, `wk3`, etc. .

### How does pivoting work?

Create an example with `tribble()`:

``` r
df <- 
  tribble(
    ~id, ~bp1, ~bp2,
    "A", 100,  120,
    "B", 140,  115,
    "C", 120,  125
  )

df
```

    # A tibble: 3 × 3
      id      bp1   bp2
      <chr> <dbl> <dbl>
    1 A       100   120
    2 B       140   115
    3 C       120   125

Now pivot the tibble to have a column for the `number` of the blood
pressure measurement and the `value` of that measurement.

``` r
df |> 
  pivot_longer(
    cols = bp1:bp2,
    names_to = "measurement",
    values_to = "value"
  )
```

    # A tibble: 6 × 3
      id    measurement value
      <chr> <chr>       <dbl>
    1 A     bp1           100
    2 A     bp2           120
    3 B     bp1           140
    4 B     bp2           115
    5 C     bp1           120
    6 C     bp2           125

### Many variables in columns names

- It is always sensible to describe all the different variables /
  variables that can be made out from looking at the raw data to get a
  feeling for how many columns there are and which name they might get.
- In the `who2` example there are six columns / variables, including
  that describing the values that are given in the original columns.
- Try to visualize how each original rows get repeatet by the number of
  original columns to be pivoted.
  - Simply because, and that’s not so intuitive, no new data is created,
    but the data as is just rearranged.

### Data an variable names in the column headers

- Okay, this is a complex case:

``` r
household
```

    # A tibble: 5 × 5
      family dob_child1 dob_child2 name_child1 name_child2
       <int> <date>     <date>     <chr>       <chr>      
    1      1 1998-11-26 2000-01-29 Susan       Jose       
    2      2 1996-06-22 NA         Mark        <NA>       
    3      3 2002-07-11 2004-04-05 Sam         Seth       
    4      4 2004-10-10 2009-08-27 Craig       Khai       
    5      5 2000-12-05 2005-02-28 Parker      Gracie     

- The original columns describe several values of a single variable
  (`child`), and at the same time two other distinc variables (`name`,
  and `dob`).

- `pivot_longer()` has special solution for that with the `.value`
  *sentinel* as a value for the `names_to` argument.

  - This way, each unique value of the position in the string you
    described with `.value` will become a new column which will be
    populated with the original values. Check it out:

``` r
household |> 
  pivot_longer(
    # All columns, but `family`
    cols = !family,
    # Take the first value in the column string and separate unique names
    names_to = c(".value", "child"),
    # The column strings are separeted by "_"
    names_sep = "_",
    # Ignore rows that would have NA as value.
    values_drop_na = TRUE
  )
```

    # A tibble: 9 × 4
      family child  dob        name  
       <int> <chr>  <date>     <chr> 
    1      1 child1 1998-11-26 Susan 
    2      1 child2 2000-01-29 Jose  
    3      2 child1 1996-06-22 Mark  
    4      3 child1 2002-07-11 Sam   
    5      3 child2 2004-04-05 Seth  
    6      4 child1 2004-10-10 Craig 
    7      4 child2 2009-08-27 Khai  
    8      5 child1 2000-12-05 Parker
    9      5 child2 2005-02-28 Gracie

- So now we see that the actual observation is not the family, but the
  single child.
