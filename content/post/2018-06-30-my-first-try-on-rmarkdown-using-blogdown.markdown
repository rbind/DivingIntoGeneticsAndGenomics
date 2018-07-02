---
title: my first try on Rmarkdown using blogdown
author: Ming Tang
date: '2018-06-30'
slug: my-first-try-on-rmarkdown-using-blogdown
categories:
  - R
tags:
  - blogdown
header:
  caption: ''
  image: ''
---

I have used blogdown writing regular markdown posts, but the real power is from the Rmarkdown! let me try it for this post.

Note that you do not knit the Rmarkdown by yourself, rather you let blogdown do the heavy lift.


```r
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Warning: package 'tibble' was built under R version 3.4.3
```

```
## Warning: package 'purrr' was built under R version 3.4.4
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
library(ggplot2)
head(mtcars)
```

```
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```


```r
ggplot(mtcars, aes(x = disp, y = mpg)) + geom_point()
```

<img src="/post/2018-06-30-my-first-try-on-rmarkdown-using-blogdown_files/figure-html/unnamed-chunk-2-1.png" width="672" />

```r
ggplot(mtcars, aes(x= as.factor(cyl), y = hp)) + geom_boxplot(aes(color = as.factor(cyl))) + geom_jitter()
```

<img src="/post/2018-06-30-my-first-try-on-rmarkdown-using-blogdown_files/figure-html/unnamed-chunk-2-2.png" width="672" />

It is awesome! blogdown will knit and render the code and the output into a html file. How cool is that!!
