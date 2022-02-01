Lab 02 - Plastic waste
================
Ella Beiser
January 27, 2022

## Load packages and data

``` r
library(tidyverse) 
```

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

## Exercises

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

    ## Rows: 240 Columns: 10

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (3): code, entity, continent
    ## dbl (7): year, gdp_per_cap, plastic_waste_per_cap, mismanaged_plastic_waste_...

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
ggplot(data = plastic_waste, aes(x = plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.2)
```

    ## Warning: Removed 51 rows containing non-finite values (stat_bin).

![](lab-02_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
plastic_waste %>%
  filter(plastic_waste_per_cap > 3.5)
```

    ## # A tibble: 1 × 10
    ##   code  entity              continent  year gdp_per_cap plastic_waste_p… mismanaged_plas…
    ##   <chr> <chr>               <chr>     <dbl>       <dbl>            <dbl>            <dbl>
    ## 1 TTO   Trinidad and Tobago North Am…  2010      31261.              3.6             0.19
    ## # … with 3 more variables: mismanaged_plastic_waste <dbl>, coastal_pop <dbl>,
    ## #   total_pop <dbl>

Trinidad and Tobago produce the most waste per capita per day in the
world according to a report by Forbes. Additionally, their waste is
improperly managed and likely to end up in the ocean.

### Exercise 1

The density plot below shows that the majority of countries in Africa
have very low plastic waste per capita with the majority producing 0.1
kilograms per capita. In contrast, Europe has a wide range of plastic
waste production with the majority ranging from 0.15-0.27 kilograms per
capita. The plot also indicates that each continent has a number of
outliers who produce significantly more plastic waste per capita than
the rest.

``` r
#Below is a histogram of plastic waste faceted
ggplot(data = plastic_waste, aes(x = plastic_waste_per_cap)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 51 rows containing non-finite values (stat_bin).

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

``` r
#Below is a density plot with color!
ggplot(data = plastic_waste, 
       mapping = aes(x = plastic_waste_per_cap, 
                     color = continent, 
                     fill = continent)) +
  geom_density(alpha = 0.7)
```

    ## Warning: Removed 51 rows containing non-finite values (stat_density).

![](lab-02_files/figure-gfm/plastic-waste-continent-2.png)<!-- -->

### Exercise 2

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = plastic_waste_per_cap, 
                     color = continent, 
                     fill = continent)) +
  geom_density(alpha = 0.4) +
  xlim(0, 1)
```

    ## Warning: Removed 52 rows containing non-finite values (stat_density).

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

### Exercise 3

The color and fill of the curves are defined by the mapping aesthetic
because they are dependent on the data and it is not consistent across
the board. In contrast, the alpha level is a characteristic of the
plotting geom because the alpha is a consistent level for the entire
plot.

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = continent, 
                     y = plastic_waste_per_cap)) +
  geom_boxplot()
```

    ## Warning: Removed 51 rows containing non-finite values (stat_boxplot).

![](lab-02_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

### Exercise 4

The box plot automatically calculates summary statistics like the mean,
outliers etc and indicates them on the graph. Whereas in the violin
plot, the plot shows approximately where the mean would be but it is
less clear.

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = continent, 
                     y = plastic_waste_per_cap)) +
  geom_violin()
```

    ## Warning: Removed 51 rows containing non-finite values (stat_ydensity).

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

### Exercise 5

This scatterplot shows that as plastic waste increases, mismanaged
plastic waste also increases. The rate at which it increases varies
significantly. There is also an outlier with significant plastic waste
but lower mismanaged plastic waste per capita.

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = plastic_waste_per_cap, 
                     y = mismanaged_plastic_waste_per_cap)) +
  geom_point()
```

    ## Warning: Removed 51 rows containing missing values (geom_point).

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

### Exercise 6

With color, it becomes more evident the relationship between plastic
waste and mismanaged plastic waste per capita. For Africa it appears as
if there is a strong positive correlation between the two. The rest of
the continents are less clear.

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = plastic_waste_per_cap, 
                     y = mismanaged_plastic_waste_per_cap, color = continent, fill = continent)) +
  geom_point()
```

    ## Warning: Removed 51 rows containing missing values (geom_point).

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-continent-1.png)<!-- -->

### Exercise 7

The relationship between the coastal populations and plastic waste per
capita appear to be more linearly associated.

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = total_pop, 
                     y = plastic_waste_per_cap)) +
  geom_point()
```

    ## Warning: Removed 61 rows containing missing values (geom_point).

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

``` r
ggplot(data = plastic_waste, 
       mapping = aes(x = coastal_pop, 
                     y = plastic_waste_per_cap)) +
  geom_point()
```

    ## Warning: Removed 51 rows containing missing values (geom_point).

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

### Exercise 8

From this plot we can interpret that until the coastal population
proportion reaches 0.75 the plastic waste per capita increases with it
but after that point it levels off and slowly declines. Additionally,
the vast majority of countries have a coast population proportion below
1.0.

``` r
ggplot(data = plastic_waste) +
  geom_point(mapping = aes(x = coastal_pop/total_pop, 
                     y = plastic_waste_per_cap, color = continent)) +
  geom_smooth(mapping = aes(x = coastal_pop/total_pop, 
                     y = plastic_waste_per_cap)) + ylim(0, 0.7) + xlim(0, 1.5) +
  labs(x = "Coastal population proportion (Coastal / total population)", y = "Plastic waste per capita", title = "Plastic waste vs coastal proportion", subtitle = "by continent", color = "Continent") +
scale_colour_viridis_d()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 64 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 64 rows containing missing values (geom_point).

![](lab-02_files/figure-gfm/recreate-viz-1.png)<!-- -->
