HW 1
================
Disheng jiang

# Loading Library

Loading library that going to used in the rest of problems

``` r
library(moderndive)  
weather_df <- early_january_weather # provide data "early_january_weather" and rename it

library(tidyverse) # dplyr/ggplot2/readr
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

# Problem 1

Answer: The data are from the moderndive library. After loading the
library we want to use, we can load the dataset .

This dataset contains 358 rows and 15 columns.

The variable names are origin, year, month, day, hour, temp, dewp,
humid, wind_dir, wind_speed, wind_gust, precip, pressure, visib,
time_hour (15 total).

The mean temperature is about 39.58 ˚F.

## Plot

Visualize the time and temperature by scatter plot and save it

``` r
data("weather_df")
```

    ## Warning in data("weather_df"): data set 'weather_df' not found

``` r
ggplot(weather_df, aes ( x = time_hour ,  y = temp , color = humid )) +
  geom_point(size = 4, alpha = 0.8, stroke = 0.05,) +
  labs(title = "Scatter Plot",
       subtitle = "Temperature(˚F) vs. Time(hours)",
       x = "Temp (˚F)",
       y = "Hour(s)",
       color = "Humid %"
       ) 
```

![](HW-1_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
ggsave("scatter_plot.png", dpi = 600, path = "~/Desktop/💻/按课程类型分类/P8105 Data Science 1/Homeworks/p8105_hw1")
```

    ## Saving 7 x 5 in image

# Problem 2

## 1st Step

Create a dataframe

``` r
data_df = tibble(
  norm_sample = rnorm(10), # A random sample of size 10 from a standard Normal distribution
  
  vec_logical = rnorm(10) > 0, # A logical vector indicating whether elements of the sample are       greater than 0
  
  vec_char = letters[1:10], # A character vector of length 10
  
  vec_factor = factor(rep(c("Super", "Modest", "Inferior"), length.out = 10)) # A factor vector of length 10, with 3   different factor “levels”
)
```

## Q: Try to take the mean of each variable in your dataframe. What works and what doesn’t?

Taking the mean of a variable in a dataframe by using the pull function

``` r
mean(data_df |>  pull(norm_sample)) # Value is working
```

    ## [1] -0.1055911

``` r
mean(data_df |>  pull(vec_logical)) # Logical is working
```

    ## [1] 0.5

``` r
mean(data_df |>  pull(vec_char)) # Character is not working
```

    ## Warning in mean.default(pull(data_df, vec_char)): argument is not numeric or
    ## logical: returning NA

    ## [1] NA

``` r
mean(data_df |>  pull(vec_factor)) # Factor is not working
```

    ## Warning in mean.default(pull(data_df, vec_factor)): argument is not numeric or
    ## logical: returning NA

    ## [1] NA

Conclusion: Only numerical value is working with mean and pull function

## Q: What happens, and why?

Applying the as.numeric function to the logical, character, and factor
variables

``` r
as.numeric(data_df |>  pull(vec_logical)) # Logical is working
```

    ##  [1] 0 0 1 1 0 1 0 0 1 1

``` r
as.numeric(data_df |>  pull(vec_char)) # Character is not working (NA returns)
```

    ## Warning: NAs introduced by coercion

    ##  [1] NA NA NA NA NA NA NA NA NA NA

``` r
as.numeric(data_df |>  pull(vec_factor)) # Factor is partially working (return internal encoding)
```

    ##  [1] 3 2 1 3 2 1 3 2 1 3

Conclusion:

Logical: 1=TRUE, 0=FALSE.

Character: Reports an error message stating “NAs introduced by coercion”
because character values cannot be directly converted to numeric values.

Factor: Returns the internal encoding of the factor (1 and 2), not the
actual names of the levels.

## Q:Does this help explain what happens when you try to take the mean?

Answer: Yes, for numeric columns, mean is the regular mean. But for
logical columns, R automatically interpret TRUE as 1 and FALSE as 0, so
the average is calculated. If the value is more close to 1, the TUREs
are more. And for character columns, mean reports an error because it
cannot convert to numeric values. Last for factor columns, mean reports
an error because factors are categorical and are not automatically
converted to coded numeric values.

## Summary

The mean function is only works on numeric and logical data types.
Character and factor values it won’t automatically converted to numeric
values. When we applied the mean function, it will return an error
message. The as.numeric function reveals how R handles different data
types internally.
