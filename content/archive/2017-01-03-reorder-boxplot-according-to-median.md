---
title: reorder boxplot according to median
author: Ming Tang
date: '2017-01-03T10:57:00-08:00'
lastmod: '2017-01-03T10:57:00-08:00'
slug: reorder-boxplot-according-to-median
tags:
- box-plot
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/01/reorder-boxplot-according-to-median_3.html
description: It is very common that you want to reorder the boxplot according to the medians to see a better trend. I will show you how to do it using ggplot2 and the…
summary: It is very common that you want to reorder the boxplot according to the medians to see a better trend. I will show you how to do it using ggplot2 and the…
header:
  caption: ''
  image: ''
---

## reorder factors for boxplot

## 

It is very common that you want to reorder the boxplot according to the medians to see a better trend. I will show you how to do it using `ggplot2` and the [`forcats`](https://cran.r-project.org/web/packages/forcats/index.html) packages which are developed by Hadely Wickham.\
Read his new R for data science book: <http://r4ds.had.co.nz/factors.html>

``` r
head(iris)
```

|     |
|-----|
|     |

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr>
<th style="text-align: left;"></th>
<th style="text-align: right;"><p>Sepal.Length<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>Sepal.Width<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>Petal.Length<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>Petal.Width<br />
&lt;dbl&gt;</p></th>
<th style="text-align: left;"><p>Species<br />
&lt;fctr&gt;</p></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">1</td>
<td style="text-align: right;">5.1</td>
<td style="text-align: right;">3.5</td>
<td style="text-align: right;">1.4</td>
<td style="text-align: right;">0.2</td>
<td style="text-align: left;">setosa</td>
</tr>
<tr>
<td style="text-align: left;">2</td>
<td style="text-align: right;">4.9</td>
<td style="text-align: right;">3.0</td>
<td style="text-align: right;">1.4</td>
<td style="text-align: right;">0.2</td>
<td style="text-align: left;">setosa</td>
</tr>
<tr>
<td style="text-align: left;">3</td>
<td style="text-align: right;">4.7</td>
<td style="text-align: right;">3.2</td>
<td style="text-align: right;">1.3</td>
<td style="text-align: right;">0.2</td>
<td style="text-align: left;">setosa</td>
</tr>
<tr>
<td style="text-align: left;">4</td>
<td style="text-align: right;">4.6</td>
<td style="text-align: right;">3.1</td>
<td style="text-align: right;">1.5</td>
<td style="text-align: right;">0.2</td>
<td style="text-align: left;">setosa</td>
</tr>
<tr>
<td style="text-align: left;">5</td>
<td style="text-align: right;">5.0</td>
<td style="text-align: right;">3.6</td>
<td style="text-align: right;">1.4</td>
<td style="text-align: right;">0.2</td>
<td style="text-align: left;">setosa</td>
</tr>
<tr>
<td style="text-align: left;">6</td>
<td style="text-align: right;">5.4</td>
<td style="text-align: right;">3.9</td>
<td style="text-align: right;">1.7</td>
<td style="text-align: right;">0.4</td>
<td style="text-align: left;">setosa</td>
</tr>
</tbody>
</table>

6 rows

a basic boxplot:

``` r
library(ggplot2)
ggplot(iris, aes(x = Species, y = Sepal.Width)) + geom_boxplot()
```

![](/img/archive/reorder-boxplot-according-to-median/01-inline.png)\
Now, reorder it according to the median levels.

``` r
ggplot(iris, aes(x = reorder(Species, Sepal.Width, FUN = median), y = Sepal.Width)) + geom_boxplot()
```

![](/img/archive/reorder-boxplot-according-to-median/02-inline.png)\
use the `fct_reorder` function from `forcats`. it has a similar syntax with `reorder`, but note that the argument `fun` is lower case.

``` r
library(forcats)
ggplot(iris, aes(x = fct_reorder(Species, Sepal.Width, fun = median), y = Sepal.Width)) + geom_boxplot()
```

![](/img/archive/reorder-boxplot-according-to-median/03-inline.png)\
You can change the order from high to low

``` r
ggplot(iris, aes(x = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE), y = Sepal.Width)) + geom_boxplot()
```

![](/img/archive/reorder-boxplot-according-to-median/04-inline.png)\
some touch-ups

``` r
ggplot(iris, aes(x = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE), y = Sepal.Width)) +
        geom_boxplot(aes(fill = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE))) +
        geom_jitter(position=position_jitter(0.2)) +
        theme_bw(base_size = 14) +
        xlab("Species") +
        ylab("Sepal width") +
        scale_fill_discrete(guide = guide_legend(title = "Species"))
```

![](/img/archive/reorder-boxplot-according-to-median/05-inline.png)

``` r
NA
```

If you want to fill the colors mannually with colors from `RcolorBrewer`:

``` r
library(RColorBrewer)
ggplot(iris, aes(x = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE), y = Sepal.Width)) +
        geom_boxplot(aes(fill = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE))) +
        scale_fill_manual(values = brewer.pal(3, "Dark2"), guide = guide_legend(title = "Species")) +
        geom_jitter(position=position_jitter(0.2)) +
        theme_bw(base_size = 14) +
        xlab("Species") +
        ylab("Sepal width")
```

![](/img/archive/reorder-boxplot-according-to-median/06-inline.png)\
A different theme:

``` r
ggplot(iris, aes(x = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE), y = Sepal.Width)) +
        geom_boxplot(aes(fill = fct_reorder(Species, Sepal.Width, fun = median, .desc =TRUE))) +
        scale_fill_manual(values = brewer.pal(3, "Dark2"), guide = guide_legend(title = "Species")) +
        geom_jitter(position=position_jitter(0.2)) +
        theme_classic(base_size = 14) +
        xlab("Species") +
        ylab("Sepal width")
```

![](/img/archive/reorder-boxplot-according-to-median/07-inline.png)

![](/img/archive/reorder-boxplot-according-to-median/08-inline.png)
