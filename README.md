This document goes through the steps to retrive meteorological time series using [weathercan](https://github.com/ropensci/weathercan/) R package. Retrived data downloaded from `weathercan` can be digested to a [missing value imputation tool](http://raven.uwaterloo.ca/) devised for filling any missing values and generating [Raven](http://raven.uwaterloo.ca/)  complient `.rvt` files.

## Based on proximity
In this method station(s) are searched based on a given proximity and a radious search. So, user is required to provide a `la` and `lon` coordinates in decimal DMS format and a radious search (Km):

``` r
if(require("weathercan")) install.packages("weathercan",repos = "https://ropensci.r-universe.dev")
library(weathercan)

lat     <-  43.4  # degree
lon     <- -80.5  # degree
dist    <-  30    # Km
interval<- "day"
stn<-stations_search(coords   = c(lat,lon),
                     dist     = dist,
                     interval = interval)
stn<-stn[stn$end>2019,]  # filtering stations with available data after 2019

# download data
data<-weather_dl(station_ids = stn$station_id,
                 start       = as.Date("2019-01-01"),
                 end         = as.Date("2021-12-31"),
                 interval    = interval)

# save data
write.csv(data,"met_data.csv")
```

## Based on name
In this method, searching is perfomed based on the location name:

``` r
if(require("weathercan")) install.packages("weathercan",repos = "https://ropensci.r-universe.dev")
library(weathercan)

name    <- "waterloo"
interval<- "day"
stn<-stations_search(name     = name,
                     interval = interval)
stn<-stn[stn$end>2010,]    # filtering stations with available data after 2000

# download data
data<-weather_dl(station_ids = stn$station_id,
                 start       = as.Date("2010-01-01"),
                 end         = as.Date("2012-12-31"),
                 interval    = interval)

# save data
write.csv(data,"met_data.csv")
```
