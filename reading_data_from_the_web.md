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
  read_html(url) 

drug_use_html %>% 
  html_table() %>% 
  first() %>% 
  slice(-1) %>% view
```

## Star wars

Get some star wars data

``` r
sw_url <- "https://www.imdb.com/list/ls070150896/"

sw_html <- read_html(sw_url)

sw_html %>% 
  html_elements(".lister-item-header a") %>% 
  html_text()
```

    ## [1] "Star Wars: Episode I - The Phantom Menace"     
    ## [2] "Star Wars: Episode II - Attack of the Clones"  
    ## [3] "Star Wars: Episode III - Revenge of the Sith"  
    ## [4] "Star Wars: Episode IV - A New Hope"            
    ## [5] "Star Wars: Episode V - The Empire Strikes Back"
    ## [6] "Star Wars: Episode VI - Return of the Jedi"    
    ## [7] "Star Wars: Episode VII - The Force Awakens"    
    ## [8] "Star Wars: Episode VIII - The Last Jedi"       
    ## [9] "Star Wars: The Rise Of Skywalker"

``` r
gross_rev_vec <- 
  sw_html %>%
  html_elements(".text-small:nth-child(7) span:nth-child(5)") %>%
  html_text()
```

``` r
dynamite_url <- "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html <- read_html(dynamite_url) 

dynamite_reviews_title <- 
  dynamite_html %>% 
  html_elements(".a-text-bold span") %>% 
  html_text()

dynamite_stars <- 
  dynamite_html %>% 
  html_elements("#cm_cr-review_list .review-rating") %>% 
  html_text()

dynamite_review_text <- 
  dynamite_html %>%
  html_elements(".review-text-content span") %>%
  html_text()

dynamite_reviews <- tibble(
  title = dynamite_reviews_title,
  stars = dynamite_stars,
  text = dynamite_review_text
)
```

## Try some APIs

Get some data fro on API about water

``` r
water_df <-
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content()

water_df_json <-
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>%
  content("text") %>% 
  jsonlite::fromJSON() %>% 
  as_tibble()
```

BRFSS data via API

``` r
bfrss_df <- 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv",
      query = list("$limit" = 5000)) %>% 
  content()
```

``` r
poke_data <- 
  GET("https://pokeapi.co/api/v2/pokemon/1") %>% 
  content()
poke_data[["name"]]
```

    ## [1] "bulbasaur"

``` r
poke_data[["height"]]
```

    ## [1] 7

``` r
poke_data[["abilities"]]
```

    ## [[1]]
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "overgrow"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/65/"
    ## 
    ## 
    ## [[1]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[1]]$slot
    ## [1] 1
    ## 
    ## 
    ## [[2]]
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "chlorophyll"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/34/"
    ## 
    ## 
    ## [[2]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[2]]$slot
    ## [1] 3
