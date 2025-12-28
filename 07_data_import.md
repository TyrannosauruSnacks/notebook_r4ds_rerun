# Data import
Max Hachemeister
2025-12-28

- [Prerequisites](#prerequisites)
  - [Reading data from a file](#reading-data-from-a-file)
    - [Other arguments](#other-arguments)
    - [Other file types](#other-file-types)
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

## Reading data from a file

Get the `students` csv.

``` r
students <- 
  read_csv("https://pos.it/r4ds-students-csv")
```

    Rows: 6 Columns: 5
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (4): Full Name, favourite.food, mealPlan, AGE
    dbl (1): Student ID

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
students
```

    # A tibble: 6 × 5
      `Student ID` `Full Name`      favourite.food     mealPlan            AGE  
             <dbl> <chr>            <chr>              <chr>               <chr>
    1            1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2            2 Barclay Lynn     French fries       Lunch only          5    
    3            3 Jayendra Lyne    N/A                Breakfast and lunch 7    
    4            4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5            5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6            6 Güvenç Attila    Ice cream          Lunch only          6    

Have `"N/A"` be recognized as actual `NA`:

``` r
students <- 
  read_csv(
    "https://pos.it/r4ds-students-csv",
    # Need to also add 'empty' fields as `NA`
    na = c("N/A", "")
  )
```

    Rows: 6 Columns: 5
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (4): Full Name, favourite.food, mealPlan, AGE
    dbl (1): Student ID

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
students
```

    # A tibble: 6 × 5
      `Student ID` `Full Name`      favourite.food     mealPlan            AGE  
             <dbl> <chr>            <chr>              <chr>               <chr>
    1            1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2            2 Barclay Lynn     French fries       Lunch only          5    
    3            3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4            4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5            5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6            6 Güvenç Attila    Ice cream          Lunch only          6    

Convert row names from strings to values:

``` r
students |> 
  rename(
    student_id = `Student ID`,
    full_name = `Full Name`
  )
```

    # A tibble: 6 × 5
      student_id full_name        favourite.food     mealPlan            AGE  
           <dbl> <chr>            <chr>              <chr>               <chr>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2          2 Barclay Lynn     French fries       Lunch only          5    
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6          6 Güvenç Attila    Ice cream          Lunch only          6    

Or use the `clean_names()` function from the `janitor` package which
cleans all the other column names in the same call:

``` r
students |> 
  janitor::clean_names()
```

    # A tibble: 6 × 5
      student_id full_name        favourite_food     meal_plan           age  
           <dbl> <chr>            <chr>              <chr>               <chr>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2          2 Barclay Lynn     French fries       Lunch only          5    
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6          6 Güvenç Attila    Ice cream          Lunch only          6    

`meal_plan` are a fixed set of known values, i. e. “factors”. So convert
the column type:

``` r
students |> 
  janitor::clean_names() |> 
  mutate(meal_plan = factor(meal_plan))
```

    # A tibble: 6 × 5
      student_id full_name        favourite_food     meal_plan           age  
           <dbl> <chr>            <chr>              <fct>               <chr>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2          2 Barclay Lynn     French fries       Lunch only          5    
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6          6 Güvenç Attila    Ice cream          Lunch only          6    

`age` column is still character because one value is literal “five”.
Convert to numerical:

``` r
students <- 
  students |> 
    janitor::clean_names() |> 
    mutate(
      meal_plan = factor(meal_plan),
      age = parse_number(if_else(age == "five", "5", age))
    )
```

### Other arguments

`read_csv` uses the first row as the column names by default:

``` r
read_csv(
  "a,b,c
  1,2,3
  4,5,6"
)
```

    # A tibble: 2 × 3
          a     b     c
      <dbl> <dbl> <dbl>
    1     1     2     3
    2     4     5     6

Sometimes there are metadata rows in the raw csv. Use the argument
`skip = n` to drop the first `n` rows, or `comment = "#"` to drop lines
with a given prefix:

``` r
read_csv(
  "Some first line
  And also a second line
  y,x,z
  1,2,3
  4,5,6",
  skip = 2
)
```

    # A tibble: 2 × 3
          y     x     z
      <dbl> <dbl> <dbl>
    1     1     2     3
    2     4     5     6

``` r
read_csv(
  ";; First line comment
  ;; Second line comment
  x,y,z
  1,2,3
  4,5,6
  7,8,9",
  comment = ";;"
)
```

    # A tibble: 3 × 3
          x     y     z
      <dbl> <dbl> <dbl>
    1     1     2     3
    2     4     5     6
    3     7     8     9

The other way around use `col_names = FALSE` when there are no column
names in the raw data:

``` r
read_csv(
  "1,2,3
  4,5,6
  7,8,9",
  col_names = FALSE
)
```

    # A tibble: 3 × 3
         X1    X2    X3
      <dbl> <dbl> <dbl>
    1     1     2     3
    2     4     5     6
    3     7     8     9

At this point you can also pass the column names as a character vector:

``` r
read_csv(
  "1,2,3
  4,5,6
  7,8,9",
  col_names = c("x", "y", "z")
)
```

    Rows: 3 Columns: 3
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    dbl (3): x, y, z

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    # A tibble: 3 × 3
          x     y     z
      <dbl> <dbl> <dbl>
    1     1     2     3
    2     4     5     6
    3     7     8     9

### Other file types

There are also special functions for other file types which are worth
considering. Check the `read_csv` documentation.

### Exercises

#### 1. What function would you use to read a file where fields were separated with “\|”?

The documentation for `read_csv()` states that the base function for
this is `read_delim()` in which the delimiter can be specified:

``` r
read_delim(
  "x|y|z
  1|2|3
  4|5|6",
  delim = "|"
)
```

    # A tibble: 2 × 3
      x         y     z
      <chr> <dbl> <dbl>
    1 "  1"     2     3
    2 "  4"     5     6

#### 2. Apart from `file`, `skip`, and `comment`, what other arguments do `read_csv()` and `read_tsv()` have in common?

Check the documentation again:

``` r
?read_csv
?read_tsv
```

`read_csv()` and `read_tsv()` share all the same arguments, as they are
variations of the same base function, that is `read_delim()`.

#### 3. What are the most important arguments to `read_fwf()`?

Check the documentation:

``` r
?read_fwf
```

From what I’ve gathered `read_fwf()` need at least the filename or
string, the `col_position` argument which defines where/how the columns
are split up ,and the `col_names` argument wich can be part to the
`col_position` values.

#### 4. Sometimes strings in a CSV file contain commas.

> To prevent them from causing problem, they need to be sorrounded by a
> quoting character, like `"` or `'`. By default, `read_csv()` assumes
> that the quoting character will be `"`. To read the following text
> into a data frame, what argument to `read_csv()` do you need to
> specify? \`“x,y,‘a,b’”

I think I need to specifiy the non standard quote with the `quote`
argument:

``` r
read_csv(
  "x,y\n1,'a,b'",
  quote = "\'"
)
```

    # A tibble: 1 × 2
          x y    
      <dbl> <chr>
    1     1 a,b  
