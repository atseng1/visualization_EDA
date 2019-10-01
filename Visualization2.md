`ggplot` 2
================
Ashley Tseng
10/1/2019

Note that `ggplot2` is automatically loaded once we load the tidyverse.
`ggridges` won’t override any of the packages we have already loaded.

## Load dataset

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'crul':
    ##   method                 from
    ##   as.character.form_file httr

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## file path:          /Users/ashleytseng/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2019-09-26 10:24:58

    ## file min/max dates: 1869-01-01 / 2019-09-30

    ## file path:          /Users/ashleytseng/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2019-09-26 10:25:12

    ## file min/max dates: 1965-01-01 / 2019-09-30

    ## file path:          /Users/ashleytseng/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2019-09-26 10:25:16

    ## file min/max dates: 1999-09-01 / 2019-09-30

Inside the rNOAA package, there is a dataset that we want. This lets us
interact with the NOAA servers and tell them what monitors we want data
from. We will recode the IDs with `mutate`.

## Making new plots

Start with an old plot:

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.5)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](Visualization2_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

Add labels:

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.5) +
  labs (
    title = "Temperature plot",
    x = "Minimum Temp (C)",
    y = "Maximum Temp (C)",
    caption = "Data from NOAA via rnoaa package"
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](Visualization2_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
