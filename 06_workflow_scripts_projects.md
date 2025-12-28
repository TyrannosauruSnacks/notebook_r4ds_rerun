# Workflow: Scripts and projects
Max Hachemeister
2025-12-28

- [Prerequisites](#prerequisites)
  - [Scripts](#scripts)
  - [Projects](#projects)
  - [Exercises](#exercises)
    - [1. Go to the RStudio Tips Twitter account,
      https://twitter.com/rstudiotips and find one tin that looks
      interesting. Practice using
      it!](#1-go-to-the-rstudio-tips-twitter-account-httpstwittercomrstudiotips-and-find-one-tin-that-looks-interesting-practice-using-it)
    - [2. What other common mistakes will RStudio diagnostics report?
      Read
      https://posit.co/blog/rstudio-1-4-preview-rainbow-parentheses/ to
      find
      out.](#2-what-other-common-mistakes-will-rstudio-diagnostics-report-read-httpspositcoblogrstudio-1-4-preview-rainbow-parentheses-to-find-out)

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

## Scripts

- Are the the next stable unit after writing directly in the R console.
- You can write and format the code first, and afterwards send it for
  execution to the R process.
- Good for troubleshooting and iterative programming.
- R Studio will also help you with marking possible problems in the
  sidebar.
- You can also put sections in the script for overview and navigation.
- These scripts can be shared and are reproducible in some sense.
  - Also as you save them you can search them later on for functions or
    other code blocks that might be useful beyond the direct purpose of
    the script.

## Projects

- Refer to a folder containing scripts, plots, data and other in- or
  output related to a single project.
- There is also an `environment`, which is the collection of functions,
  data and objects related to the current project, or eben globally.
  - It is advised to clear it at the end of each session, so to make
    sure your scripts do not rely on some ephemeral objects, but create
    everything within their own code.

## Exercises

### 1. Go to the RStudio Tips Twitter account, <https://twitter.com/rstudiotips> and find one tin that looks interesting. Practice using it!

- Rainbow parentheses, yeas!
  - <https://posit.co/blog/rstudio-1-4-preview-rainbow-parentheses/>

### 2. What other common mistakes will RStudio diagnostics report? Read <https://posit.co/blog/rstudio-1-4-preview-rainbow-parentheses/> to find out.

- Check whether all arguments in a function were used.
- Check for missing arguments and commas.
- Check correct variable definition and usage.
- Check adherence to RStudio code style.
- Spellcheck, nice!
