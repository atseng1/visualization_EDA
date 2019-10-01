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

<img src="Visualization2_files/figure-gfm/unnamed-chunk-1-1.png" width="90%" />
Should always add labels if you want to show your plot to someone else.
ggplot has a variety of default settings. There are scale x, y, color,
etc. functions.

Customizing x-axis tick marks:

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.5) +
  labs (
    title = "Temperature plot",
    x = "Minimum Temp (C)",
    y = "Maximum Temp (C)",
    caption = "Data from NOAA via rnoaa package"
  ) +
  scale_x_continuous(
    breaks = c(-15, -5, 20)
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-2-1.png" width="90%" />

Adding labels to each tick mark:

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.5) +
  labs (
    title = "Temperature plot",
    x = "Minimum Temp (C)",
    y = "Maximum Temp (C)",
    caption = "Data from NOAA via rnoaa package"
  ) +
  scale_x_continuous(
    breaks = c(-15, -5, 20),
    labels = c("-15C", "-5(still too cold)", "20")
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

Transforming y-axis and changing position of axis:

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package") + 
  scale_x_continuous(
    breaks = c(-15, 0, 15), 
    labels = c("-15ºC", "0", "15"),
    limits = c(-20, 30)) + 
  scale_y_continuous(
    trans = "sqrt", 
    position = "right")
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

## Colors and themes

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package") + 
  scale_color_hue(
    name = "Weather Station", 
    h = c(100, 300))
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

Viridis color hues:

``` r
ggp_base = 
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package"
  ) + 
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE
  )
```

## Themes

``` r
ggp_base + 
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-7-1.png" width="90%" />
Anytime you have a legend with long names, it takes up a lot of plot
space. So, you should put the legend at the bottom horizontally to save
space instead.

White background:

``` r
ggp_base + 
  theme_bw() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-8-1.png" width="90%" />

No border:

``` r
ggp_base + 
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-9-1.png" width="90%" />
Prof prefers minimal plots all the way\!

Only axes borders:

``` r
ggp_base + 
  theme_classic() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-10-1.png" width="90%" />

## Creating plots with more than one dataset

We can split weather\_df into separate datasets for Central Park and
Waikiki. Then we use one in the ggplot() call and another in
geom\_line():

``` r
central_park = 
  weather_df %>% 
  filter(name == "CentralPark_NY")

waikiki = 
  weather_df %>% 
  filter(name == "Waikiki_HA")

ggplot(data = waikiki, aes(x = date, y = tmax, color = name)) + 
  geom_point() + 
  geom_line(data = central_park)
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-11-1.png" width="90%" />

(Brief aside about colors)

``` r
waikiki %>% 
  ggplot (aes(x = date, y = tmax)) +
  geom_point(color = "blue")
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-12-1.png" width="90%" />

## Patchwork

``` r
tmax_tmin_p = 
  weather_df %>% 
  ggplot(aes(x = tmax, y = tmin, color = name)) + 
  geom_point(alpha = .5) +
  theme(legend.position = "none")

prcp_dens_p = 
  weather_df %>% 
  filter(prcp > 0) %>% 
  ggplot(aes(x = prcp, fill = name)) + 
  geom_density(alpha = .5) + 
  theme(legend.position = "none")

tmax_date_p = 
  weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  geom_smooth(se = FALSE) + 
  theme(legend.position = "bottom")

(tmax_tmin_p + prcp_dens_p) / tmax_date_p
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-13-1.png" width="90%" />

## Data manipulation

``` r
weather_df %>% 
  mutate(
    name = factor(name),
    name = fct_relevel(name, "Waikiki_HA",
        "CentralPark_NY"),
    name = fct_reorder(name, tmax)
  ) %>% 
  ggplot(aes(x = name, y = tmax, color = name)) +
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-14-1.png" width="90%" />
There is an implicit ordering that ggplot uses to arrange the data on
the plot. We can change the order of the factors which will then change
the order on our plot.

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-15-1.png" width="90%" />

``` r
pulse_data = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names() %>%
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi") %>%
  select(id, visit, everything()) %>%
  mutate(visit = recode(visit, "bl" = "00m"),
         visit = factor(visit, levels = str_c(c("00", "01", "06", "12"), "m"))) %>%
  arrange(id, visit)

ggplot(pulse_data, aes(x = visit, y = bdi)) + 
  geom_boxplot()
```

    ## Warning: Removed 879 rows containing non-finite values (stat_boxplot).

<img src="Visualization2_files/figure-gfm/unnamed-chunk-16-1.png" width="90%" />

``` r
pup_data = 
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 

litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3) %>%
  mutate(wt_gain = gd18_weight - gd0_weight,
         day_of_tx = as.numeric(day_of_tx))

fas_data = left_join(pup_data, litter_data, by = "litter_number") 

fas_data %>% 
  select(sex, dose, day_of_tx, pd_ears:pd_walk) %>% 
  pivot_longer(
    pd_ears:pd_walk,
    names_to = "outcome", 
    values_to = "pn_day") %>% 
  drop_na() %>% 
  mutate(outcome = forcats::fct_reorder(outcome, day_of_tx, median)) %>% 
  ggplot(aes(x = dose, y = pn_day)) + 
  geom_violin() + 
  facet_grid(day_of_tx ~ outcome)
```

<img src="Visualization2_files/figure-gfm/unnamed-chunk-17-1.png" width="90%" />
Use “drop\_na” to not visualize the NA values
