Tidy Data
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

## ‘povot_longer’

Load the PULSE data

``` r
pulse_data =
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
```

Wide format to long format…

1.  pivot_longer: change wide format to long format. Here, we want to
    combine bdi columns into one row
2.  names_to: place bdi variables to “visit” column
3.  names_prefix: remove prefix “bdi_score\_”
4.  values_to: move bdi values to “bdi” column

``` r
pulse_data_tidy =
  pulse_data %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  )
```

rewrite, combined, and extend (to add a mutate)

1.  use relocate so id & visit columns appear first
2.  use mutate to change visit values to numbers via `recode`

``` r
pulse_data =
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
) %>% 
    relocate(id, visit) %>% 
    mutate(visit = recode(visit, "bl" = "00m"))
```
