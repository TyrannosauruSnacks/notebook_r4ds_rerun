# Workflow: getting help
Max Hachemeister
2026-01-02

- [Prerequisites](#prerequisites)
  - [Google is your friend](#google-is-your-friend)
  - [Making a reprex](#making-a-reprex)

# Prerequisites

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.6
    ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ✔ ggplot2   4.0.1     ✔ tibble    3.3.0
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.2
    ✔ purrr     1.2.0     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
theme_set(theme_light())
```

## Google is your friend

I guess this is a little bit deprecated since AI and such. The third
edition of this book will probably address this.

## Making a reprex

Reprex  
Short for minimal **repr**oducible **ex**ample.

Take this example code:

``` r
y <- 1:4
mean(y)
```

    [1] 2.5

Copy it to the clipboard and then call `reprex::reprex()` in the
console.

If in doubt, try updating your packages first. Maybe the source of you
problem has already been adressed. Use `tidyverse_update()` for packages
in the tidyverse. Other than that you can use the package manager of
RStudio.

Also use `dput()` to get an text only representation of R objects, like
special dataframes, or functions from other packages.
