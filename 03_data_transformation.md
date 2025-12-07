# 03 Data transformation
Max Hachemeister

## Prerequisites

``` r
library(tidyverse)
library(nycflights13)
set_theme(theme_light())
```

## Introduction

Take a look at the `flights` dataframe, by either `glimpse()` or just
typing the object directly. I will use the former:

``` r
glimpse(flights)
```

    Rows: 336,776
    Columns: 19
    $ year           <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2…
    $ month          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    $ day            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    $ dep_time       <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 558, 558, …
    $ sched_dep_time <int> 515, 529, 540, 545, 600, 558, 600, 600, 600, 600, 600, …
    $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2, -2, -1…
    $ arr_time       <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 753, 849,…
    $ sched_arr_time <int> 819, 830, 850, 1022, 837, 728, 854, 723, 846, 745, 851,…
    $ arr_delay      <dbl> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -3, 7, -1…
    $ carrier        <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV", "B6", "…
    $ flight         <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79, 301, 4…
    $ tailnum        <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN", "N394…
    $ origin         <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR", "LGA",…
    $ dest           <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL", "IAD",…
    $ air_time       <dbl> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138, 149, 1…
    $ distance       <dbl> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 944, 733, …
    $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5, 6, 6, 6…
    $ minute         <dbl> 15, 29, 40, 45, 0, 58, 0, 0, 0, 0, 0, 0, 0, 0, 0, 59, 0…
    $ time_hour      <dttm> 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 0…

## Rows

`filter()` changes which rows are present without changing their order.

`arrange()` changes the order of the rows without changing which are
present.

`distinct()` finds rows with unique values.

### `filter()`

Find all flights with that departed more than 120 minutes late:

``` r
flights |> 
  filter(dep_delay > 120)
```

    # A tibble: 9,723 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      848           1835       853     1001           1950
     2  2013     1     1      957            733       144     1056            853
     3  2013     1     1     1114            900       134     1447           1222
     4  2013     1     1     1540           1338       122     2020           1825
     5  2013     1     1     1815           1325       290     2120           1542
     6  2013     1     1     1842           1422       260     1958           1535
     7  2013     1     1     1856           1645       131     2212           2005
     8  2013     1     1     1934           1725       129     2126           1855
     9  2013     1     1     1938           1703       155     2109           1823
    10  2013     1     1     1942           1705       157     2124           1830
    # ℹ 9,713 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

|      |                                    |
|------|------------------------------------|
| `>`  | greater than                       |
| `>=` | greater than or equal              |
| `<`  | less than                          |
| `<=` | less than or equal to              |
| `==` | equal to (notice the two “**=**” ) |
| `!=` | not equal to                       |

Also several conditions can be combined:

|      |     |
|------|-----|
| `&`  | AND |
| `\|` | OR  |

E.g. find all flights that departed in January OR February (which can be
translated to \[…\] all departures of January and February):

``` r
flights |> 
  filter(month == 1 | month == 2)
```

    # A tibble: 51,955 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 51,945 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

Ah you couldn’t even navigate to the ones from February, because there
are 51955 rows in total, even after the filtering.

Let’s find all flights that departed on the 1st of January:

``` r
flights |> 
  filter(month == 1 & day == 1)
```

    # A tibble: 842 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 832 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

Oh and there is this `%in%`, which followed by a vector
(`c(value1, value2)`) can be used to spare you typing a lot of `==`, `|`
combinations. E.g. for finding all the flights that departed in January,
or February, or March you could just write:

``` r
flights |> 
  filter(month %in% c(1, 2, 3))
```

    # A tibble: 80,789 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 80,779 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

While the long version would be:

``` r
flights |> 
  filter(month == 1 | month == 2 | month == 3)
```

    # A tibble: 80,789 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 80,779 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

Important to know is that `filter()` never changes the input. That means
to save the results you need to assign them to an object:

``` r
jan1 <- 
  flights |> 
  filter(month == 1 & day == 2)

jan1
```

### `arrange()`

`arrange()` changes the order of the rows, by the columns you provide as
argument. Every additional column will break ties of the preceding ones,
which translates to: “Arrange by attribute1 and then arrange all those
with the same attribute1-values by the values of attribute2”.

For example: “Arrange all flights by Year, and all of the same year then
by month, and then within each month by day and within each day by
departure time.”, would translate to code as:

``` r
flights |> 
  arrange(
    year,
    month,
    day,
    dep_time
  )
```

    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 336,766 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

And If I wanted to arrange any of the columns from highest to lowest, I
can put it in the `desc()` function:

``` r
flights |> 
  arrange(
    year,
    desc(month),
    desc(day),
    dep_time
  )
```

    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013    12    31       13           2359        14      439            437
     2  2013    12    31       18           2359        19      449            444
     3  2013    12    31       26           2245       101      129           2353
     4  2013    12    31      459            500        -1      655            651
     5  2013    12    31      514            515        -1      814            812
     6  2013    12    31      549            551        -2      925            900
     7  2013    12    31      550            600       -10      725            745
     8  2013    12    31      552            600        -8      811            826
     9  2013    12    31      553            600        -7      741            754
    10  2013    12    31      554            550         4     1024           1027
    # ℹ 336,766 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

### `distinct()`

So `distinct()` foremost filters out duplicate rows in a dataset, so we
could see, whether there are duplicates in the `flights` dataset:

``` r
# from the flights dataframe filter out duplicate rows and then count the remaining
flights |> 
  distinct() |> 
  nrow()
```

    [1] 336776

``` r
# from the flights dataframe count all the rows
flights |> 
  nrow()
```

    [1] 336776

`distinct()` further finds all the unique combinations for provided
columns:

``` r
# from the flights dataframe find all unique tailnumber and destination pairs
flights |> 
  distinct(tailnum, dest)
```

    # A tibble: 44,465 × 2
       tailnum dest 
       <chr>   <chr>
     1 N14228  IAH  
     2 N24211  IAH  
     3 N619AA  MIA  
     4 N804JB  BQN  
     5 N668DN  ATL  
     6 N39463  ORD  
     7 N516JB  FLL  
     8 N829AS  IAD  
     9 N593JB  MCO  
    10 N3ALAA  ORD  
    # ℹ 44,455 more rows

``` r
# lets see whether the order makes a difference
flights |> 
  distinct(dest, tailnum)
```

    # A tibble: 44,465 × 2
       dest  tailnum
       <chr> <chr>  
     1 IAH   N14228 
     2 IAH   N24211 
     3 MIA   N619AA 
     4 BQN   N804JB 
     5 ATL   N668DN 
     6 ORD   N39463 
     7 FLL   N516JB 
     8 IAD   N829AS 
     9 MCO   N593JB 
    10 ORD   N3ALAA 
    # ℹ 44,455 more rows

So the order of the argument just changes the form of the output. If I
want to keep all the columns in the output, I can use the
`.keep_all = TRUE` option:

``` r
flights |> 
  distinct(tailnum, dest,
           .keep_all = TRUE)
```

    # A tibble: 44,465 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      544            545        -1     1004           1022
     5  2013     1     1      554            600        -6      812            837
     6  2013     1     1      554            558        -4      740            728
     7  2013     1     1      555            600        -5      913            854
     8  2013     1     1      557            600        -3      709            723
     9  2013     1     1      557            600        -3      838            846
    10  2013     1     1      558            600        -2      753            745
    # ℹ 44,455 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

`distinct()` always gives you the first instance of a distinct row it
finds. As somewhat of an extension to this `count()` also give you the
distinct combinations of the defined columns, but also the number times
each combination occurs. More on that in another section though.

``` r
flights |> 
  count(tailnum, dest, sort = TRUE)
```

    # A tibble: 44,465 × 3
       tailnum dest      n
       <chr>   <chr> <int>
     1 <NA>    BOS     373
     2 N328AA  LAX     313
     3 <NA>    DCA     288
     4 <NA>    ORD     288
     5 N338AA  LAX     286
     6 N327AA  LAX     280
     7 N335AA  LAX     275
     8 N323AA  LAX     265
     9 N336AA  LAX     265
    10 N319AA  LAX     257
    # ℹ 44,455 more rows

## Exercises

### 1.

> In a single pipeline for each condition, find all flights that meet
> the condition:
>
> - Had an arrival delay of two or more hours
> - Flew to Houston (`IAH` or `HOU`)
> - Were operated by United, American, or Delta
> - Departed in summer (July, August, and September)
> - Arrived more than two hours late but didn’t leave late
> - Were delayed by at least an hour, but made up over 30 minutes in
>   flight

``` r
# Had an arrival delay of two or more hours
flights |> 
  filter(arr_delay >= 120)
```

    # A tibble: 10,200 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      811            630       101     1047            830
     2  2013     1     1      848           1835       853     1001           1950
     3  2013     1     1      957            733       144     1056            853
     4  2013     1     1     1114            900       134     1447           1222
     5  2013     1     1     1505           1310       115     1638           1431
     6  2013     1     1     1525           1340       105     1831           1626
     7  2013     1     1     1549           1445        64     1912           1656
     8  2013     1     1     1558           1359       119     1718           1515
     9  2013     1     1     1732           1630        62     2028           1825
    10  2013     1     1     1803           1620       103     2008           1750
    # ℹ 10,190 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Flew to Houston (`IAH` or `HOU`)
flights |> 
  filter(dest == "IAH" | dest == "HOU")
```

    # A tibble: 9,313 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      623            627        -4      933            932
     4  2013     1     1      728            732        -4     1041           1038
     5  2013     1     1      739            739         0     1104           1038
     6  2013     1     1      908            908         0     1228           1219
     7  2013     1     1     1028           1026         2     1350           1339
     8  2013     1     1     1044           1045        -1     1352           1351
     9  2013     1     1     1114            900       134     1447           1222
    10  2013     1     1     1205           1200         5     1503           1505
    # ℹ 9,303 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
## or the elegant version
flights |> 
  filter(dest %in% c("IAH", "HOU"))
```

    # A tibble: 9,313 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      623            627        -4      933            932
     4  2013     1     1      728            732        -4     1041           1038
     5  2013     1     1      739            739         0     1104           1038
     6  2013     1     1      908            908         0     1228           1219
     7  2013     1     1     1028           1026         2     1350           1339
     8  2013     1     1     1044           1045        -1     1352           1351
     9  2013     1     1     1114            900       134     1447           1222
    10  2013     1     1     1205           1200         5     1503           1505
    # ℹ 9,303 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Were operated by United, American, or Delta
## Check the abbreviation for the `airlines`
airlines
```

    # A tibble: 16 × 2
       carrier name                       
       <chr>   <chr>                      
     1 9E      Endeavor Air Inc.          
     2 AA      American Airlines Inc.     
     3 AS      Alaska Airlines Inc.       
     4 B6      JetBlue Airways            
     5 DL      Delta Air Lines Inc.       
     6 EV      ExpressJet Airlines Inc.   
     7 F9      Frontier Airlines Inc.     
     8 FL      AirTran Airways Corporation
     9 HA      Hawaiian Airlines Inc.     
    10 MQ      Envoy Air                  
    11 OO      SkyWest Airlines Inc.      
    12 UA      United Air Lines Inc.      
    13 US      US Airways Inc.            
    14 VX      Virgin America             
    15 WN      Southwest Airlines Co.     
    16 YV      Mesa Airlines Inc.         

``` r
## So we have UA, AA, and DL
flights |> 
  filter(carrier %in% c("UA", "AA", "DL"))
```

    # A tibble: 139,504 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1      517            515         2      830            819
     2  2013     1     1      533            529         4      850            830
     3  2013     1     1      542            540         2      923            850
     4  2013     1     1      554            600        -6      812            837
     5  2013     1     1      554            558        -4      740            728
     6  2013     1     1      558            600        -2      753            745
     7  2013     1     1      558            600        -2      924            917
     8  2013     1     1      558            600        -2      923            937
     9  2013     1     1      559            600        -1      941            910
    10  2013     1     1      559            600        -1      854            902
    # ℹ 139,494 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Departed in summer (July, August, and September)
flights |> 
  filter(month %in% c(6, 7, 8))
```

    # A tibble: 86,995 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     6     1        2           2359         3      341            350
     2  2013     6     1      451            500        -9      624            640
     3  2013     6     1      506            515        -9      715            800
     4  2013     6     1      534            545       -11      800            829
     5  2013     6     1      538            545        -7      925            922
     6  2013     6     1      539            540        -1      832            840
     7  2013     6     1      546            600       -14      850            910
     8  2013     6     1      551            600        -9      828            850
     9  2013     6     1      552            600        -8      647            655
    10  2013     6     1      553            600        -7      700            711
    # ℹ 86,985 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
## Or slightly more elegant
flights |> 
  filter(month %in% 6:8)
```

    # A tibble: 86,995 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     6     1        2           2359         3      341            350
     2  2013     6     1      451            500        -9      624            640
     3  2013     6     1      506            515        -9      715            800
     4  2013     6     1      534            545       -11      800            829
     5  2013     6     1      538            545        -7      925            922
     6  2013     6     1      539            540        -1      832            840
     7  2013     6     1      546            600       -14      850            910
     8  2013     6     1      551            600        -9      828            850
     9  2013     6     1      552            600        -8      647            655
    10  2013     6     1      553            600        -7      700            711
    # ℹ 86,985 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Arrived more than two hours late but didn't leave late
flights |> 
  filter(arr_delay >= 120 & dep_delay <= 0)
```

    # A tibble: 29 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1    27     1419           1420        -1     1754           1550
     2  2013    10     7     1350           1350         0     1736           1526
     3  2013    10     7     1357           1359        -2     1858           1654
     4  2013    10    16      657            700        -3     1258           1056
     5  2013    11     1      658            700        -2     1329           1015
     6  2013     3    18     1844           1847        -3       39           2219
     7  2013     4    17     1635           1640        -5     2049           1845
     8  2013     4    18      558            600        -2     1149            850
     9  2013     4    18      655            700        -5     1213            950
    10  2013     5    22     1827           1830        -3     2217           2010
    # ℹ 19 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Were delayed by at least an hour, but made up over 30 minutes
flights |> 
  filter(dep_delay >= 60 & arr_delay < dep_delay - 30)
```

    # A tibble: 1,844 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1     2205           1720       285       46           2040
     2  2013     1     1     2326           2130       116      131             18
     3  2013     1     3     1503           1221       162     1803           1555
     4  2013     1     3     1839           1700        99     2056           1950
     5  2013     1     3     1850           1745        65     2148           2120
     6  2013     1     3     1941           1759       102     2246           2139
     7  2013     1     3     1950           1845        65     2228           2227
     8  2013     1     3     2015           1915        60     2135           2111
     9  2013     1     3     2257           2000       177       45           2224
    10  2013     1     4     1917           1700       137     2135           1950
    # ℹ 1,834 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
## Just checking whether I can write it differently
flights |> 
  filter(dep_delay >= 60 & dep_delay - arr_delay >= 30)
```

    # A tibble: 2,074 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     1     1716           1545        91     2140           2039
     2  2013     1     1     2205           1720       285       46           2040
     3  2013     1     1     2326           2130       116      131             18
     4  2013     1     3     1503           1221       162     1803           1555
     5  2013     1     3     1821           1530       171     2131           1910
     6  2013     1     3     1839           1700        99     2056           1950
     7  2013     1     3     1850           1745        65     2148           2120
     8  2013     1     3     1923           1815        68     2036           1958
     9  2013     1     3     1941           1759       102     2246           2139
    10  2013     1     3     1950           1845        65     2228           2227
    # ℹ 2,064 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
### It seems to get different results. Let's compare the solutions: 
flights |> 
  select(dep_delay, arr_delay) |>
  mutate(time_won = dep_delay - arr_delay,
         won_30 = arr_delay < dep_delay - 30,
         won_30_alt = time_won >= 30) |> 
  filter(won_30 != won_30_alt)
```

    # A tibble: 2,445 × 5
       dep_delay arr_delay time_won won_30 won_30_alt
           <dbl>     <dbl>    <dbl> <lgl>  <lgl>     
     1        -5       -35       30 FALSE  TRUE      
     2        -2       -32       30 FALSE  TRUE      
     3        91        61       30 FALSE  TRUE      
     4        15       -15       30 FALSE  TRUE      
     5        -6       -36       30 FALSE  TRUE      
     6        -1       -31       30 FALSE  TRUE      
     7        -2       -32       30 FALSE  TRUE      
     8         0       -30       30 FALSE  TRUE      
     9        -2       -32       30 FALSE  TRUE      
    10        -1       -31       30 FALSE  TRUE      
    # ℹ 2,435 more rows

``` r
### Ah yeah, actually they both give the same result, when I keep the `=` for both comparisons
### Hat another intuition when writing the first call
### Correct version
flights |> 
  select(dep_delay, arr_delay) |>
  mutate(time_won = dep_delay - arr_delay,
         won_30 = arr_delay <= dep_delay - 30,
         won_30_alt = time_won >= 30) |> 
  filter(won_30 != won_30_alt)
```

    # A tibble: 0 × 5
    # ℹ 5 variables: dep_delay <dbl>, arr_delay <dbl>, time_won <dbl>,
    #   won_30 <lgl>, won_30_alt <lgl>

### 2.

> Sort `flights` to find the flights with the longest departure delays.
> Find the flights that left earliest in the morning.

``` r
# Flights with the longest delay
flights |> 
  arrange(desc(dep_delay))
```

    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1     9      641            900      1301     1242           1530
     2  2013     6    15     1432           1935      1137     1607           2120
     3  2013     1    10     1121           1635      1126     1239           1810
     4  2013     9    20     1139           1845      1014     1457           2210
     5  2013     7    22      845           1600      1005     1044           1815
     6  2013     4    10     1100           1900       960     1342           2211
     7  2013     3    17     2321            810       911      135           1020
     8  2013     6    27      959           1900       899     1236           2226
     9  2013     7    22     2257            759       898      121           1026
    10  2013    12     5      756           1700       896     1058           2020
    # ℹ 336,766 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Flights that left earliest in the morning
flights |> 
  arrange(dep_time)
```

    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1    13        1           2249        72      108           2357
     2  2013     1    31        1           2100       181      124           2225
     3  2013    11    13        1           2359         2      442            440
     4  2013    12    16        1           2359         2      447            437
     5  2013    12    20        1           2359         2      430            440
     6  2013    12    26        1           2359         2      437            440
     7  2013    12    30        1           2359         2      441            437
     8  2013     2    11        1           2100       181      111           2225
     9  2013     2    24        1           2245        76      121           2354
    10  2013     3     8        1           2355         6      431            440
    # ℹ 336,766 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

### 3.

> Sort `flights` to find the fastest flights. (Hint: Try including a
> math calculation inside of your function)

``` r
flights |> 
  arrange(air_time)
```

    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
     1  2013     1    16     1355           1315        40     1442           1411
     2  2013     4    13      537            527        10      622            628
     3  2013    12     6      922            851        31     1021            954
     4  2013     2     3     2153           2129        24     2247           2224
     5  2013     2     5     1303           1315       -12     1342           1411
     6  2013     2    12     2123           2130        -7     2211           2225
     7  2013     3     2     1450           1500       -10     1547           1608
     8  2013     3     8     2026           1935        51     2131           2056
     9  2013     3    18     1456           1329        87     1533           1426
    10  2013     3    19     2226           2145        41     2305           2246
    # ℹ 336,766 more rows
    # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    #   hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
# Ahh okay with fastest, I guess It should be the most `distance` per `air_time`
# This would translate to miles per minute
flights |> 
  arrange(desc(distance / air_time)) |> 
  select(distance, air_time)
```

    # A tibble: 336,776 × 2
       distance air_time
          <dbl>    <dbl>
     1      762       65
     2     1008       93
     3      594       55
     4      748       70
     5     1035      105
     6     1598      170
     7     1598      172
     8     1623      175
     9     1598      173
    10     1598      173
    # ℹ 336,766 more rows

``` r
## Yeah let's actually add this as a column to see what's happening
flights |>
  mutate(miles_minutes = distance / air_time) |> 
  arrange(desc(miles_minutes)) |> 
  select(distance, air_time, miles_minutes)
```

    # A tibble: 336,776 × 3
       distance air_time miles_minutes
          <dbl>    <dbl>         <dbl>
     1      762       65         11.7 
     2     1008       93         10.8 
     3      594       55         10.8 
     4      748       70         10.7 
     5     1035      105          9.86
     6     1598      170          9.4 
     7     1598      172          9.29
     8     1623      175          9.27
     9     1598      173          9.24
    10     1598      173          9.24
    # ℹ 336,766 more rows

``` r
## Can I do this directly in `arrange()`
flights |> 
  arrange(miles_minutes = distance / air_time) |> 
  select(distance, air_time, miles_minutes)
```

    Error in `select()`:
    ! Can't select columns that don't exist.
    ✖ Column `miles_minutes` doesn't exist.

``` r
### No :-)

## But I can make it miles per hour at least
flights |> 
  mutate(miles_hour = distance / (air_time / 60)) |> 
  arrange(desc(miles_hour)) |> 
  select(distance, air_time, miles_hour)
```

    # A tibble: 336,776 × 3
       distance air_time miles_hour
          <dbl>    <dbl>      <dbl>
     1      762       65       703.
     2     1008       93       650.
     3      594       55       648 
     4      748       70       641.
     5     1035      105       591.
     6     1598      170       564 
     7     1598      172       557.
     8     1623      175       556.
     9     1598      173       554.
    10     1598      173       554.
    # ℹ 336,766 more rows

### 4.

> Was there a flight on every day of 2013?

``` r
flights |> 
  distinct(day)
```

    # A tibble: 31 × 1
         day
       <int>
     1     1
     2     2
     3     3
     4     4
     5     5
     6     6
     7     7
     8     8
     9     9
    10    10
    # ℹ 21 more rows

``` r
# A yeah this way I only get the distinct days.
# But they can repeat for each month

flights |> 
  distinct(month, day)
```

    # A tibble: 365 × 2
       month   day
       <int> <int>
     1     1     1
     2     1     2
     3     1     3
     4     1     4
     5     1     5
     6     1     6
     7     1     7
     8     1     8
     9     1     9
    10     1    10
    # ℹ 355 more rows

``` r
# To make it precise, let's count the rows
flights |> 
  distinct(month, day) |> 
  nrow()
```

    [1] 365

### 5.

> Which flights traveled the farthest distance? Which traveled the least
> distance?

``` r
# Farthest distance
flights |> 
  arrange(desc(distance)) |> 
  select(flight, distance)
```

    # A tibble: 336,776 × 2
       flight distance
        <int>    <dbl>
     1     51     4983
     2     51     4983
     3     51     4983
     4     51     4983
     5     51     4983
     6     51     4983
     7     51     4983
     8     51     4983
     9     51     4983
    10     51     4983
    # ℹ 336,766 more rows

``` r
# Least distance
flights |> 
  arrange(distance) |> 
  select(flight, distance)
```

    # A tibble: 336,776 × 2
       flight distance
        <int>    <dbl>
     1   1632       17
     2   3833       80
     3   4193       80
     4   4502       80
     5   4645       80
     6   4193       80
     7   4619       80
     8   4619       80
     9   4619       80
    10   4619       80
    # ℹ 336,766 more rows

### 6.

> Does it matter what order you used `filter()` and `arrange()` if
> you’re using both? Why/Why not? Think about the results and how much
> work the functions would have to do.

`arrange()` will always compare all rows, so If I would `filter()`
before that, fewer rows will have to be compared by `arrange()`.

For example `flights` has 336776 rows, while after filtering for the
`AA` `carrier` there will be only 32729 to `arrange()`.

``` r
flights |> 
  filter(carrier == "AA") |> 
  arrange(desc(arr_delay)) |> 
  select(arr_delay)
```

    # A tibble: 32,729 × 1
       arr_delay
           <dbl>
     1      1007
     2       878
     3       852
     4       846
     5       802
     6       783
     7       783
     8       648
     9       645
    10       614
    # ℹ 32,719 more rows
