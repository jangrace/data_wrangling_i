Data Manipulation
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

## Load FAS Litters Data

1.  Load csv data
2.  Clean variables

``` r
litters_df = read_csv("./data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = janitor::clean_names(litters_df)
```

## Dplyr (Select): choose columns of interest

1.  For select, first specify dataframe
2.  Then specify columns you’d like to keep. In this example, we want to
    keep “group” and “litter_number” column.

``` r
select(litters_df, group, litter_number)
```

    ## # A tibble: 49 × 2
    ##    group litter_number  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # … with 39 more rows

1.  select then specify dataframe (litters_df)
2.  specify columns to keep (keep columns from gd0_weight to
    gd_of_birth)

``` r
select(litters_df, group, gd0_weight:gd_of_birth)
```

    ## # A tibble: 49 × 4
    ##    group gd0_weight gd18_weight gd_of_birth
    ##    <chr>      <dbl>       <dbl>       <dbl>
    ##  1 Con7        19.7        34.7          20
    ##  2 Con7        27          42            19
    ##  3 Con7        26          41.4          19
    ##  4 Con7        28.5        44.1          19
    ##  5 Con7        NA          NA            20
    ##  6 Con7        NA          NA            20
    ##  7 Con7        NA          NA            20
    ##  8 Con8        NA          NA            20
    ##  9 Con8        NA          NA            20
    ## 10 Con8        28.5        NA            20
    ## # … with 39 more rows

## Dplyr (Select): choose columns you’d like to remove

1.  specify dataframe (litters_df)
2.  indicate column you’d like to remove by placing “-” before column
    name (ex: -litter_number = remove litter number column)

``` r
select(litters_df, -litter_number)
```

    ## # A tibble: 49 × 7
    ##    group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_…¹ pups_…²
    ##    <chr>      <dbl>       <dbl>       <dbl>           <dbl>        <dbl>   <dbl>
    ##  1 Con7        19.7        34.7          20               3            4       3
    ##  2 Con7        27          42            19               8            0       7
    ##  3 Con7        26          41.4          19               6            0       5
    ##  4 Con7        28.5        44.1          19               5            1       4
    ##  5 Con7        NA          NA            20               6            0       6
    ##  6 Con7        NA          NA            20               6            0       4
    ##  7 Con7        NA          NA            20               9            0       9
    ##  8 Con8        NA          NA            20               9            1       8
    ##  9 Con8        NA          NA            20               8            0       8
    ## 10 Con8        28.5        NA            20               8            0       8
    ## # … with 39 more rows, and abbreviated variable names ¹​pups_dead_birth,
    ## #   ²​pups_survive

## Renaming columns

-   OPTION 1: Use select columns and rename selected columns

``` r
select(litters_df, GROUP = group, LITTer_NUmBer = litter_number)
```

    ## # A tibble: 49 × 2
    ##    GROUP LITTer_NUmBer  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # … with 39 more rows

-   OPTION 2: Use rename to only rename columns and not select

``` r
rename(litters_df, GROUP = group, LITTer_NUmBer = litter_number)
```

    ## # A tibble: 49 × 8
    ##    GROUP LITTer_NUmBer   gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
    ##    <chr> <chr>                <dbl>       <dbl>    <dbl>   <dbl>   <dbl>   <dbl>
    ##  1 Con7  #85                   19.7        34.7       20       3       4       3
    ##  2 Con7  #1/2/95/2             27          42         19       8       0       7
    ##  3 Con7  #5/5/3/83/3-3         26          41.4       19       6       0       5
    ##  4 Con7  #5/4/2/95/2           28.5        44.1       19       5       1       4
    ##  5 Con7  #4/2/95/3-3           NA          NA         20       6       0       6
    ##  6 Con7  #2/2/95/3-2           NA          NA         20       6       0       4
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA         20       9       0       9
    ##  8 Con8  #3/83/3-3             NA          NA         20       9       1       8
    ##  9 Con8  #2/95/3               NA          NA         20       8       0       8
    ## 10 Con8  #3/5/2/2/95           28.5        NA         20       8       0       8
    ## # … with 39 more rows, and abbreviated variable names ¹​gd_of_birth,
    ## #   ²​pups_born_alive, ³​pups_dead_birth, ⁴​pups_survive

## Select variable of interest that starts or ends with certain term

-   select everything that starts with gd

``` r
select(litters_df, starts_with("gd"))
```

    ## # A tibble: 49 × 3
    ##    gd0_weight gd18_weight gd_of_birth
    ##         <dbl>       <dbl>       <dbl>
    ##  1       19.7        34.7          20
    ##  2       27          42            19
    ##  3       26          41.4          19
    ##  4       28.5        44.1          19
    ##  5       NA          NA            20
    ##  6       NA          NA            20
    ##  7       NA          NA            20
    ##  8       NA          NA            20
    ##  9       NA          NA            20
    ## 10       28.5        NA            20
    ## # … with 39 more rows

## Move variable of interest to the front

-   OPTION 1: Using SELECT move column of interest to the front (i.e.,
    make litter_number be the first column)

``` r
select(litters_df, litter_number, everything())
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
    ##    <chr>           <chr>      <dbl>       <dbl>    <dbl>   <dbl>   <dbl>   <dbl>
    ##  1 #85             Con7        19.7        34.7       20       3       4       3
    ##  2 #1/2/95/2       Con7        27          42         19       8       0       7
    ##  3 #5/5/3/83/3-3   Con7        26          41.4       19       6       0       5
    ##  4 #5/4/2/95/2     Con7        28.5        44.1       19       5       1       4
    ##  5 #4/2/95/3-3     Con7        NA          NA         20       6       0       6
    ##  6 #2/2/95/3-2     Con7        NA          NA         20       6       0       4
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA         20       9       0       9
    ##  8 #3/83/3-3       Con8        NA          NA         20       9       1       8
    ##  9 #2/95/3         Con8        NA          NA         20       8       0       8
    ## 10 #3/5/2/2/95     Con8        28.5        NA         20       8       0       8
    ## # … with 39 more rows, and abbreviated variable names ¹​gd_of_birth,
    ## #   ²​pups_born_alive, ³​pups_dead_birth, ⁴​pups_survive

-   OPTION 2: Using RELOCATE make litter_number come to the front

``` r
relocate(litters_df, litter_number)
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
    ##    <chr>           <chr>      <dbl>       <dbl>    <dbl>   <dbl>   <dbl>   <dbl>
    ##  1 #85             Con7        19.7        34.7       20       3       4       3
    ##  2 #1/2/95/2       Con7        27          42         19       8       0       7
    ##  3 #5/5/3/83/3-3   Con7        26          41.4       19       6       0       5
    ##  4 #5/4/2/95/2     Con7        28.5        44.1       19       5       1       4
    ##  5 #4/2/95/3-3     Con7        NA          NA         20       6       0       6
    ##  6 #2/2/95/3-2     Con7        NA          NA         20       6       0       4
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA         20       9       0       9
    ##  8 #3/83/3-3       Con8        NA          NA         20       9       1       8
    ##  9 #2/95/3         Con8        NA          NA         20       8       0       8
    ## 10 #3/5/2/2/95     Con8        28.5        NA         20       8       0       8
    ## # … with 39 more rows, and abbreviated variable names ¹​gd_of_birth,
    ## #   ²​pups_born_alive, ³​pups_dead_birth, ⁴​pups_survive
