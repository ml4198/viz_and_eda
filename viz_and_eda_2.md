Module 3 - Visualization and EDA - Part 2
================
Matthew Lawlor

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(patchwork)
```

\#\#Load the weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: /Users/lawlormk/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-10-09 10:18:25 (7.524)

    ## file min/max dates: 1869-01-01 / 2020-10-31

    ## using cached file: /Users/lawlormk/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-09 10:19:17 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: /Users/lawlormk/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-10-09 10:20:33 (0.881)

    ## file min/max dates: 1999-09-01 / 2020-10-31

Remember this plot?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Labels

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum Daily Temperature (C)",
    y = "Maximum Daily Temperature (C)",
    caption = "Data from the rnoaa packaege: temperatures in 2017"
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Scales

Start with the same plot and add x+y scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum Daily Temperature (C)",
    y = "Maximum Daily Temperature (C)",
    caption = "Data from the rnoaa packaege: temperatures in 2017"
  ) +
  scale_x_continuous(
    breaks = c(-15, 0, 15),
    labels = c("-15 C", "0", "15")
  ) +
  scale_y_continuous(
    trans = "log", ##or "sqrt"
    position = "right"
  )
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Look at color scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum Daily Temperature (C)",
    y = "Maximum Daily Temperature (C)",
    caption = "Data from the rnoaa packaege: temperatures in 2017"
  ) +
  scale_color_hue(
    name = "Location",
    h = c(100,300)
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Viridis color scale (one of many different ones available - can
google\!)

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum Daily Temperature (C)",
    y = "Maximum Daily Temperature (C)",
    caption = "Data from the rnoaa packaege: temperatures in 2017"
  ) +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## Themes

Shift the legend

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum Daily Temperature (C)",
    y = "Maximum Daily Temperature (C)",
    caption = "Data from the rnoaa packaege: temperatures in 2017"
  ) +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE) +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Change the overall theme

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum Daily Temperature (C)",
    y = "Maximum Daily Temperature (C)",
    caption = "Data from the rnoaa packaege: temperatures in 2017"
  ) +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE) +
  theme_minimal() + ##or _bw (black and white), _classic
  ##ggthemes::theme_fivethirtyeight()
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

## Setting options

``` r
library(tidyverse)

knitr::opts_chunk$set(
  fig.width = .6,
  fig.asp = .6
  out.width = "90%"
))

theme_set(theme_minimal() + theme(legend_position = "bottom"))

options(
  ggplot2.continuous.color = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_color_discrete = scale_color_viridis_d
scall_fill_discrete = scale_fill_viridis_d
```

## Data args in ‘geom’

``` r
central_park =
  weather_df %>% 
  filter(name == "CentralPark_NY")

waikiki =
  weather_df %>% 
  filter(name == "Waikiki_HA")

ggplot(data = waikiki, aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_line(data = central_park) ##substitute central park dataset for waikiki dataset for this particular geom
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

## `patchwork`

remember faceting?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, fill = name)) +
  geom_density(alpha = 0.5) +
  facet_grid(. ~ name)
```

    ## Warning: Removed 15 rows containing non-finite values (stat_density).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

what happens when you want multipanel plots but can’t facet?

``` r
tmax_tmin_p =
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
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
  geom_point() +
  geom_smooth(se = FALSE) +
  theme(legend.position = "none")

tmax_tmin_p / (prcp_dens_p + tmax_date_p)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## Data manipulation

Control your factors

``` r
weather_df %>% 
  mutate(
    name = factor(name),
    name = forcats::fct_relevel(name, c("Waikiki_HA"))
  ) %>% 
  ggplot(aes(x = name, y = tmax, fill = name)) +
  geom_violin(alpha = .5)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

What if I wanted densities for tmin and tmax simultaneously?

``` r
weather_df %>% 
  filter(name == "CentralPark_NY") %>% 
  pivot_longer(
    tmax:tmin,
    names_to = "observation",
    values_to = "temperatures"
  ) %>% 
  ggplot(aes(x = temperatures, fill = observation)) +
  geom_density(alpha = .5)
```

![](viz_and_eda_2_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
