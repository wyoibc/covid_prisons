---
title: COVID-19 Prison Data by NYT
author: Vikram Chhatre
date: June 15, 2021
---



### Note: This report shows summary information from New York Times' COVID-19 data available at the following website: 

- [https://github.com/nytimes/covid-19-data](https://github.com/nytimes/covid-19-data)



## 1. Import the prisons data into R

```r
fac <- read_csv("facilities.csv")

Parsed with column specification:
cols(
  nyt_id = col_character(),
  facility_name = col_character(),
  facility_type = col_character(),
  facility_city = col_character(),
  facility_county = col_character(),
  facility_county_fips = col_character(),
  facility_state = col_character(),
  facility_lng = col_double(),
  facility_lat = col_double(),
  latest_inmate_population = col_double(),
  max_inmate_population_2020 = col_double(),
  total_inmate_cases = col_double(),
  total_inmate_deaths = col_double(),
  total_officer_cases = col_double(),
  total_officer_deaths = col_double(),
  note = col_character()
)

```

```r
fac

# A tibble: 2,639 x 16
   nyt_id facility_name facility_type facility_city facility_county
   <chr>  <chr>         <chr>         <chr>         <chr>          
 1 F3EFE… Alex City Wo… Low-security… Alex City     Coosa          
 2 5B910… Alabama Ther… State rehabi… Columbiana    Shelby         
 3 02FB1… Bibb Correct… State prison  Brent         Bibb           
 4 6378F… Birmingham W… State prison  Birmingham    Jefferson      
 5 EAABF… Bullock Corr… State prison  Bessemer      Bullock        
 6 D19A2… Camden prison State prison  Camden        Wilcox         
 7 F80A4… Childersburg… State prison  Childersburg  Talladega      
 8 F119A… William E. D… State prison  Bessemer      Jefferson      
 9 41B5B… Draper Corre… State prison  Elmore        Elmore         
10 9C1D5… Easterling C… State prison  Cilo          Barbour        
# … with 2,629 more rows, and 11 more variables: facility_county_fips <chr>,
#   facility_state <chr>, facility_lng <dbl>, facility_lat <dbl>,
#   latest_inmate_population <dbl>, max_inmate_population_2020 <dbl>,
#   total_inmate_cases <dbl>, total_inmate_deaths <dbl>,
#   total_officer_cases <dbl>, total_officer_deaths <dbl>, note <chr>
```

## 2. Draw a outline map of USA

```r
library(maps)
library(mapdata)

usa <- map_data('usa')
states <- map_data('state')

usa_base <- ggplot(data=states) + geom_polygon(aes(x=long, y=lat, fill=I("white"), group=group), color="gray") + coord_fixed(1.3) + guides(fill=FALSE)

```

## 3. Print locations of prisons across the USA


```r
usa_base + geom_point(data=fac, aes(x=facility_lng, y=facility_lat), color="salmon", cex=0.5, alpha=3/10) + coord_fixed(xlim=c(-130, -60), ylim=c(25,50), ratio=1.3)
```

<center>
<img src="prisonloc.png" width=750> </img>
</center>



## 4. Scale location points to prison population size

```r
usa_base + geom_point(data=fac %>% filter(facility_state=="Wyoming"), aes(x=facility_lng, y=facility_lat, cex=latest_inmate_population), color="purple", alpha=3/10) + coord_fixed(xlim=c(-130, -60), ylim=c(25,50), ratio=1.3) + guides(fill=FALSE)
```

<center>
<img src="Wyoming_prispop.png" width=750> </img>
</center>












