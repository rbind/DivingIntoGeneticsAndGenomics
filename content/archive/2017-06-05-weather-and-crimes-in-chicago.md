---
title: Weather and crimes in Chicago
author: Ming Tang
date: '2017-06-05T18:45:00-07:00'
lastmod: '2017-06-05T18:45:00-07:00'
slug: weather-and-crimes-in-chicago
tags:
- dplyr
- ggplot
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/06/weather-and-crimes-in-chicago.html
description: I was attending 2017 American Society of Clinical Oncology (ASCO) annual meeting in Chicago. It was my first time to do a uber from the airport to our…
summary: I was attending 2017 American Society of Clinical Oncology (ASCO) annual meeting in Chicago. It was my first time to do a uber from the airport to our…
header:
  caption: ''
  image: ''
---

## Weather and crimes in Chicago

#### *Ming Tang*

#### *June 4, 2017*

### How the story starts

I was attending 2017 American Society of Clinical Oncology (ASCO) annual meeting in Chicago. It was my first time to do a uber from the airport to our hotel. We had a nice uber driver and we talked a lot about the city. He said he grew up in the south part of Chicago and there are a lot of crimes there. He knows the city so well that he tries his best to avoid bad districts to pick up customers. When strangers meet, topic will always be weather. I certainly joked about the badness of winter in Chicago and he replied back with the badness of hotness in Houston.\
He said there are around 600 cases of shooting per year in Chicago, and in the holidays such as Memorial day, shooting arises to 40 per day. I asked him why do you think there is such an increase. He said: I think it has to do with the weather. In the Memorial day, it becomes warmer. everybody has access to everybody. that’s why the crime rate is higher as well."\
As a budding data scientist (that’s how I define myself) to interrogate data from DNA sequencing on tumor samples, I should not just take his words, I want to verify this by some evidences. Initially, I was surprised that weather can be associated with crime rate. Then I googled around when I got the hotel and it turns out that this notion is very common.\
See a post on this topic [Chicago Crime vs. Weather: Under the Hood](http://www.static-eric.com/2013/05/chicago-crime-vs-weather.html). The author did a very good job to demonstrate the association of weather and crime rate in Chicago.\
There is even a [Kaggle channel](https://www.kaggle.com/currie32/crimes-in-chicago/discussion/34007) for this topic.\
In the post I linked, the analysis was done in `python`. I want to do some similar and more analysis using `R`. That’s why I am firing up `Rstudio`and start to type.

### Download the crime data

To associate crime rate with weather, I need data sets for them respectively.\
The [city of Chicago has a very nice portol](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2/data) for the crime data.\
I downloaded the crime data with the `csv` format. Note that the data set is relatively big (~1.5G). My computer has enough memory to hold the whole data set.

``` bash
less -S ~/Downloads/Crimes_-_2001_to_present.csv | wc -l
# 6 million lines
```

``` text
##  6346217
```

``` r
# load in the libraries
library(tidyverse)
library(ggplot2)
library(lubridate)

crime<- read_csv("~/Downloads/Crimes_-_2001_to_present.csv", col_names =T)
## split the Date column, keep only the day information

crime<- crime %>% mutate(Day = gsub("([0-9]{2}/[0-9]{2}/[0-9]{4}) .+", "\\1", Date))

## change the Day column to a date type using mdy function from lubridate
crime<- crime %>% mutate(Day_date = mdy(Day)) %>% dplyr::select(-Date, -Day)

## what's the date range for the crime data?
range(crime$Day_date)
```

``` text
## [1] "2001-01-01" "2017-05-27"
```

### Get the weather data

The weather data is a bit harder to get if you follow the linked post above. luckily, I found a prepared weather data set from the kaggle channel (was uploaded two days ago when I wrote this! lucky me).\
UPDATE: unfortunately, the weather data set was not clean and there are many missing dates.\
On twitter, Kevin Johnson @bigkage suggested a package [`weatherData`](https://ram-n.github.io/weatherData/).\
only the developmental branch in github is working, the package on CRAN does not work…\
Take a look at the [`rnoaa`](https://cran.r-project.org/web/packages/rnoaa/index.html) package as well. A post on it <https://recology.info/2015/07/weather-data-with-rnoaa/>

``` r
library("devtools")
#install_github("Ram-N/weatherData")
library(weatherData)

# O'Hare airport code is ORD, worked like a charm!
# other columns can be fetched. ?getWeatherForDate. for me, only temperature is needed
weather <- getWeatherForDate("ORD", "2001-01-01", "2017-05-27")
dim(weather)
```

However, only 388 records were found.\
I opened an [issue](https://github.com/Ram-N/weatherData/issues/33) on github.

> The reason for this is that weatherUnderground doesn’t want us to pull a huge volume of data in a single file. (For some reason, they truncate the number of rows to be 400 rows or less.)

To fetch all the data:

``` r
multi_weather <- lapply(as.character(2001:2017),
                        function(x){getWeatherForYear("ORD", x)})

weather <- do.call(rbind, multi_weather)
```

merge the two data sets

``` r
str(weather)
```

``` text
## 'data.frame':    5977 obs. of  4 variables:
##  $ Date             : POSIXlt, format: "2001-01-01" "2001-01-02" ...
##  $ Max_TemperatureF : int  17 44 50 57 46 54 39 45 46 51 ...
##  $ Mean_TemperatureF: int  6 24 34 42 36 40 32 32 32 40 ...
##  $ Min_TemperatureF : int  -6 3 19 26 26 26 26 18 17 28 ...
```

``` r
str(crime)
```

``` text
## Classes 'tbl_df', 'tbl' and 'data.frame':    6346216 obs. of  22 variables:
##  $ ID                  : int  5437456 5437458 5437459 5437460 5437461 5437462 5437463 5437464 5437465 5437466 ...
##  $ Case Number         : chr  "HN270582" "HN237768" "HN247565" "HN270399" ...
##  $ Block               : chr  "028XX E 90TH ST" "013XX S CHRISTIANA AVE" "024XX S SAWYER AVE" "086XX S LAFLIN ST" ...
##  $ IUCR                : chr  "1822" "2024" "2022" "1320" ...
##  $ Primary Type        : chr  "NARCOTICS" "NARCOTICS" "NARCOTICS" "CRIMINAL DAMAGE" ...
##  $ Description         : chr  "MANU/DEL:CANNABIS OVER 10 GMS" "POSS: HEROIN(WHITE)" "POSS: COCAINE" "TO VEHICLE" ...
##  $ Location Description: chr  "STREET" "SIDEWALK" "SIDEWALK" "OTHER" ...
##  $ Arrest              : chr  "true" "true" "true" "false" ...
##  $ Domestic            : chr  "false" "false" "false" "true" ...
##  $ Beat                : chr  "0423" "1021" "1024" "0614" ...
##  $ District            : chr  "004" "010" "010" "006" ...
##  $ Ward                : int  10 24 22 21 4 10 28 32 16 7 ...
##  $ Community Area      : int  46 29 30 71 35 55 27 7 61 48 ...
##  $ FBI Code            : chr  "18" "18" "18" "14" ...
##  $ X Coordinate        : int  1196743 1154244 1155082 1167841 1180453 1199105 1154131 1165540 1164423 1192802 ...
##  $ Y Coordinate        : int  1845841 1893642 1887583 1847481 1881415 1816541 1900784 1917732 1870859 1843576 ...
##  $ Year                : int  2007 2007 2007 2007 2007 2007 2007 2007 2007 2007 ...
##  $ Updated On          : chr  "04/15/2016 08:55:02 AM" "04/15/2016 08:55:02 AM" "04/15/2016 08:55:02 AM" "04/15/2016 08:55:02 AM" ...
##  $ Latitude            : num  41.7 41.9 41.8 41.7 41.8 ...
##  $ Longitude           : num  -87.6 -87.7 -87.7 -87.7 -87.6 ...
##  $ Location            : chr  "(41.73185728, -87.55483341)" "(41.863979388, -87.709253509)" "(41.84733605, -87.706339649)" "(41.737026255, -87.660665675)" ...
##  $ Day_date            : Date, format: "2007-04-07" "2007-03-20" ...
```

``` r
# change the POSIXlt to date
weather$Date<- as.Date(weather$Date)
crime_weather<- inner_join(crime, weather, by = c("Day_date" = "Date"))

## Note that weather information on some dates were missing
anti_join(crime, weather, by = c("Day_date" = "Date")) %>% .$Day_date %>% unique()
```

``` text
##  [1] "2005-01-23" "2004-07-28" "2001-12-25" "2001-12-24" "2001-12-22"
##  [6] "2001-11-04" "2001-11-03" "2004-08-21" "2001-05-28" "2004-08-22"
## [11] "2002-11-24" "2002-02-05" "2001-07-30" "2001-06-18" "2006-01-07"
## [16] "2001-12-23" "2004-08-20" "2006-01-06" "2006-01-08" "2007-01-01"
## [21] "2007-01-04" "2007-01-05" "2001-07-29"
```

``` text
Continue reading at Rpub.
```
