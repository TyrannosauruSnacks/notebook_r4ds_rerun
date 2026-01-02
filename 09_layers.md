# Layers
Max Hachemeister
2026-01-02

- [Prerequisites](#prerequisites)
  - [Aesthetic mappings](#aesthetic-mappings)
    - [Exercises](#exercises)
  - [Geometric objects](#geometric-objects)

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

## Aesthetic mappings

### Exercises

#### 1. Create a scatterplot of `hwy` vs. `displ` where the points are pink filled in triangles.

I usually do `y` vs. `x` as a convention, where `y` is the variable to
be explained and `x` the one in relation to that.

``` r
mpg |> 
  ggplot(aes(x = displ, y = hwy)) +
  geom_point(
    shape = 17,
    color = "pink"
  )
```

![](09_layers_files/figure-commonmark/unnamed-chunk-1-1.png)

Interestingly the “filled” triangle shape also uses the `color`, instead
of `fill`.

#### 2. Why did the following code not result in a plot with blue points?

> ``` r
> ggplot(mpg) +
> geom_point(aes(x = displ, y = hwy, color = "blue))
> ```

This might be because the color is mapped as an aesthetic, hence
dynamically changing instead of being set for every point. Let’s see
what happens when the `color` mapping is outside of the `aes()`
function.

``` r
mpg |> 
  ggplot(aes(x = displ, y = hwy), color = "blue")
```

    Warning in fortify(data, ...): Arguments in `...` must be used.
    ✖ Problematic argument:
    • color = "blue"
    ℹ Did you misspell an argument name?

![](09_layers_files/figure-commonmark/unnamed-chunk-3-1.png)

Okay this did not work. The `aes()` function has to be in the actual
`ggplot()` function, and not as part of the geom.

What happens, when the whole `aes()` call is put into the `ggplot()`
function:

``` r
mpg |> 
  ggplot(aes(x = displ, y = hwy, color = "blue")) +
  geom_point()
```

![](09_layers_files/figure-commonmark/unnamed-chunk-4-1.png)

Almost there. I think the `aes()` always expects to be mapped to a
column of a dataframe, or at least to a vector with several values.
That’s why just mapping the single value of `"blue"` to `color` does not
result in actually blue points for the geom.

I think I can either set the `"blue"` directly within the geom, or leave
it in `ggplot()`, but outside of `aes()`, so that it will be passed down
to all following geom calls.

I will try both here:

``` r
# Specify color within geom
mpg |> 
  ggplot(aes(x = displ, y = hwy)) +
  geom_point(color = "blue")
```

![](09_layers_files/figure-commonmark/unnamed-chunk-5-1.png)

``` r
# Set color in ggplot, but outside of aes() to be passed down.
mpg |> 
  ggplot(aes(x = displ, y = hwy), color = "blue") +
  geom_point()
```

    Warning in fortify(data, ...): Arguments in `...` must be used.
    ✖ Problematic argument:
    • color = "blue"
    ℹ Did you misspell an argument name?

![](09_layers_files/figure-commonmark/unnamed-chunk-5-2.png)

Aha, It only works directly in the geom function. Good to know.

#### 3. What does the `stroke` aesthetic do? What shapes does it work with?

> Hint: use `?geom_point`

The documentation states that `stroke` defines the width of the
“borders” of shapes that have these, like 21.

Let’s see:

``` r
# Make one version
mpg |> 
  ggplot(aes(displ, hwy)) +
  # Set is in the geom as I just learnedn
  geom_point(
    shape = 21,
    stroke = 1
  )
```

![](09_layers_files/figure-commonmark/unnamed-chunk-6-1.png)

``` r
# And another version to see the difference
mpg |> 
  ggplot(aes(displ, hwy)) +
  # Set is in the geom as I just learnedn
  geom_point(
    shape = 21,
    stroke = 2
  )
```

![](09_layers_files/figure-commonmark/unnamed-chunk-6-2.png)

#### 4. What happens if you map an aesthetic to something other than a variable name, like `aes(color = displ < 5)`?

> Note, you’ll also need to specify x and y.

Let’s see:

``` r
mpg |> 
  ggplot(aes(displ, hwy, color = displ < 5)) +
  geom_point(
    # I kept this because I like the appearance
    shape = 21,
    stroke = 1
  )
```

![](09_layers_files/figure-commonmark/unnamed-chunk-7-1.png)

Nice, just as I found in exercise 2, mapping an aesthetic in `aes()`
works with at least two values. And the `displ < 5` check resulted in a
vector with `TRUE` and `FALSE` values, which `aes()` in turn could
interpret and map to the `color` aesthetic.

## Geometric objects
