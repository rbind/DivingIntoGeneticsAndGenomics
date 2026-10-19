---
title: compare boxplot, violin plot and sina plot
author: Ming Tang
date: '2016-11-25T21:29:00-08:00'
lastmod: '2016-11-25T21:29:00-08:00'
slug: compare-boxplot-violin-plot-and-sina-plot
tags:
- ggplot
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/11/compare-boxplot-violin-plot-and-sina.html
description: Sometime ago, I came across sina plot.
summary: Sometime ago, I came across sina plot.
header:
  caption: ''
  image: ''
---

## Popular plots for showing the distribution of the data are boxplot and violin plot. Both have their own advantages and disadvantages. Combination of both can have more power. In a plot, we want to show people all the data, not only the summary statistics such as mean and median.  full article link <http://rpubs.com/crazyhottommy/sina-plot>

## 

Sometime ago, I came across [sina plot](https://cran.r-project.org/web/packages/sinaplot/vignettes/SinaPlot.html).

> sinaplot is inspired by the strip chart and the violin plot. By letting the normalized density of points restrict the jitter along the x-axis the plot displays the same contour as a violin plot, but resemble a simple strip chart for small number of data points. In this way the plot conveys information of both the number of data points, the density distribution, outliers and spread in a very simple, comprehensible and condensed format.

I know that [`ggforce`](https://github.com/thomasp85/ggforce) has a `geom_sina()` for the same purpose and want to try it out.\
Let’s load the libraries first:

``` r
library(ggforce)
library(ggplot2)
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

A violin plot:

``` r
p<- ggplot(iris, aes(x=Species, y=Sepal.Length)) + geom_violin(aes(fill=Species))
p
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/01-inline.png)\
we can add the mean and standard deviation for a bit more information:

``` r
p + stat_summary(fun.data=mean_sdl, fun.args = list(mult=1),
                 geom="pointrange", color="red")
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/02-inline.png)\
Very nice! How about if I want to see all the points?

``` r
# violin plot with dot plot
p + geom_dotplot(binaxis='y', stackdir='center', dotsize=0.5)
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/03-inline.png)\
How about showing the jittered points instead?

``` r
# violin plot with jittered points
# 0.2 : degree of jitter in x direction
p + geom_jitter(shape=16, position=position_jitter(0.2))
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/04-inline.png)\
Let’s combine boxplot with violin plot

``` r
# violin plot with boxplot
p +  geom_boxplot(width = 0.2) + geom_jitter(shape=16, position=position_jitter(0.2))
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/05-inline.png)\
compare with sina plot

``` r
## sina plot
ggplot(iris, aes(x=Species, y=Sepal.Length)) + geom_sina(aes(color=Species))
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/06-inline.png)\
mean, median and std can be overlayed

``` r
ggplot(iris, aes(x=Species, y=Sepal.Length)) + geom_sina(aes(color=Species)) +
        stat_summary(fun.data=mean_sdl, fun.args = list(mult=1), geom="errorbar", color="red", width=0.2) +
        stat_summary(fun.y=mean, geom="point", color="red") +
        ggtitle("sina with mean +- standard deviation")
```

![](/img/archive/compare-boxplot-violin-plot-and-sina-plot/07-inline.png)\
You can decide which plot to use for your data. I will argue that they are all better than bar plot with error bars. Showing all the data points (distribution of the data points can be judged) with summary statistics is preferred.
