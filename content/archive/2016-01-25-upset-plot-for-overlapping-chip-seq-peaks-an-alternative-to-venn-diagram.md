---
title: 'Upset plot for overlapping ChIP-seq peaks: an alternative to Venn diagram'
author: Ming Tang
date: '2016-01-25T19:52:00-08:00'
lastmod: '2016-01-25T19:52:00-08:00'
slug: upset-plot-for-overlapping-chip-seq-peaks-an-alternative-to-venn-diagram
tags:
- ChIP-seq
- R
- upset
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/01/upset-plot-for-overlapping-chip-seq.html
description: I am going to demonstrate how to make Upset plot using UpSetR package for ChIP-seq peaks. After running a peak-calling method for 3 samples, you want to…
summary: I am going to demonstrate how to make Upset plot using UpSetR package for ChIP-seq peaks. After running a peak-calling method for 3 samples, you want to…
header:
  caption: ''
  image: ''
---

## ChIP-seq-upset

#### *Ming Tang*

#### *January 25, 2016*

## 

I am going to demonstrate how to make Upset plot using [UpSetR package](https://github.com/hms-dbmi/UpSetR) for ChIP-seq peaks. After running a peak-calling method for 3 samples, you want to compare how many peaks are overlapped in two samples and in three samples. People may draw [venn diagram](https://en.wikipedia.org/wiki/Venn_diagram) for this kind of problem, but when you have more than 3 samples, the venn diagram is hard to read.

``` r
# install it if you have not
# install.packages("UpSetR")
library(UpSetR)
dummy<- data.frame(peak=c("peak_1", "peak_1", "peak_1","peak_2","peak_2","peak_3","peak_3","peak_4"),
                   sample=c("sample_1","sample_2","sample_3","sample_2","sample_3","sample_2","sample_3","sample_1"))

dummy
```

``` text
##     peak   sample
## 1 peak_1 sample_1
## 2 peak_1 sample_2
## 3 peak_1 sample_3
## 4 peak_2 sample_2
## 5 peak_2 sample_3
## 6 peak_3 sample_2
## 7 peak_3 sample_3
## 8 peak_4 sample_1
```

To get the format that UpsetR accepts, we have to “spread” the sample column to multiple columns. It is like to [change a long format data to wide format](http://www.cookbook-r.com/Manipulating_data/Converting_data_between_wide_and_long_format/). See [here](http://stackoverflow.com/questions/32312852/pivot-column-into-row-titles-in-r) as well.

``` r
library(dplyr)
```

``` text
##
## Attaching package: 'dplyr'
```

``` text
## The following objects are masked from 'package:stats':
##
##     filter, lag
```

``` text
## The following objects are masked from 'package:base':
##
##     intersect, setdiff, setequal, union
```

``` r
library(tidyr)

peaks<- dummy %>% mutate(value =1) %>% spread(sample, value, fill =0 )
peaks
```

``` text
##     peak sample_1 sample_2 sample_3
## 1 peak_1        1        1        1
## 2 peak_2        0        1        1
## 3 peak_3        0        1        1
## 4 peak_4        1        0        0
```

I noticed that one has to convert peaks(local data frame) back to data.frame for UpsetR to work. In this example, I did not convert the peaks to local data frame by `tbl_df(peaks)`.

``` r
upset(peaks, order.by = "freq")
```

![](/img/archive/upset-plot-for-overlapping-chip-seq-peaks-an-alternative-to-venn-diagram/01-inline.png)

##
