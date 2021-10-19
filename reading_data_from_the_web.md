reading\_data\_from\_the\_web
================
Ruiqi Yan
10/19/2021

``` r
library(tidyverse)
library(rvest)
library(httr)

knitr::opts_chunk$set(
  warning = FALSE,
  message = FALSE
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "turbo",
  ggplot2.continuous.fill = "turbo"
)

scale_colour_discrete = scale_colour_viridis_d(option = "turbo")
scale_fill_discrete = scale_fill_viridis_d(option = "turbo")
```

``` r
url <- "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html <- 
  read_html(url) %>% 
   html_table()
```