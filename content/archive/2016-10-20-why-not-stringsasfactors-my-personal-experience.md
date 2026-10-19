---
title: 'why not stringsAsFactors: my personal experience'
author: Ming Tang
date: '2016-10-20T12:33:00-07:00'
lastmod: '2016-10-20T12:33:00-07:00'
slug: why-not-stringsasfactors-my-personal-experience
tags:
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/10/why-not-stringsasfactors-my-personal.html
description: If you are using R and most likely you will encounter stringsAsFactors when read in files. functions such as read.table set defaultstringsAsFactors to TRUE…
summary: If you are using R and most likely you will encounter stringsAsFactors when read in files. functions such as read.table set defaultstringsAsFactors to TRUE…
header:
  caption: ''
  image: ''
---

If you are using R and most likely you will encounter `stringsAsFactors` when read in files. functions such as `read.table` set default`stringsAsFactors` to `TRUE`, which may cause various problems.\
If you want to know the history of this argument, you may want to read a post by Roger Peng:\
[stringsAsFactors: An unauthorized biography](http://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/)\
I just had an unexpected experience with `stringsAsFactors`. I will put down my notes below. This is also my first attempt to use [RNotebook](http://rmarkdown.rstudio.com/r_notebooks.html) in Rstudio :)

``` r
## dummy examples
library(dplyr)
df<- data.frame(chr1=c(1,2,3), start1 = c(10,20,30), end1 = c(30,40,50), chr2=c(1,2,5),
                type = c("BND", "BND", "BND"))
df
```

|     |
|-----|
|     |

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr>
<th style="text-align: left;"><p>chr1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>start1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>end1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>chr2<br />
&lt;dbl&gt;</p></th>
<th style="text-align: left;"><p>type<br />
&lt;fctr&gt;</p></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">1</td>
<td style="text-align: right;">10</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">1</td>
<td style="text-align: left;">BND</td>
</tr>
<tr>
<td style="text-align: left;">2</td>
<td style="text-align: right;">20</td>
<td style="text-align: right;">40</td>
<td style="text-align: right;">2</td>
<td style="text-align: left;">BND</td>
</tr>
<tr>
<td style="text-align: left;">3</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">50</td>
<td style="text-align: right;">5</td>
<td style="text-align: left;">BND</td>
</tr>
</tbody>
</table>

3 rows

Now, I want to creat a new column `type2`. if chr1 is the same as chr2, I set it to `foldbackInversion`, if not, keep it the same as `type`

``` r
df %>% mutate(type2 = ifelse(chr1==chr2, "foldbackInversion", type))
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
<th style="text-align: left;"><p>chr1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>start1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>end1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>chr2<br />
&lt;dbl&gt;</p></th>
<th style="text-align: left;"><p>type<br />
&lt;fctr&gt;</p></th>
<th style="text-align: left;"><p>type2<br />
&lt;chr&gt;</p></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">1</td>
<td style="text-align: right;">10</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">1</td>
<td style="text-align: left;">BND</td>
<td style="text-align: left;">foldbackInversion</td>
</tr>
<tr>
<td style="text-align: left;">2</td>
<td style="text-align: right;">20</td>
<td style="text-align: right;">40</td>
<td style="text-align: right;">2</td>
<td style="text-align: left;">BND</td>
<td style="text-align: left;">foldbackInversion</td>
</tr>
<tr>
<td style="text-align: left;">3</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">50</td>
<td style="text-align: right;">5</td>
<td style="text-align: left;">BND</td>
<td style="text-align: left;">1</td>
</tr>
</tbody>
</table>

3 rows

Did you just see row3 the type2 becomes 1!!!\
This is because type is stroed as factor, and interally R uses intergers to repsent them to save space. If you use `dplyr`’s internal `if_else()`function which is stricter in checking the types, you will get errors.

``` r
df %>% mutate(type2 = if_else(chr1==chr2, "foldbackInversion", type))
```

``` text
Error: `false` has type 'integer' not 'character'
```

How to fix it? change the factors to characters!!

``` r
df$type<- as.character(df$type)
df %>% mutate(type2 = if_else(chr1==chr2, "foldbackInversion", type))
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
<th style="text-align: left;"><p>chr1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>start1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>end1<br />
&lt;dbl&gt;</p></th>
<th style="text-align: right;"><p>chr2<br />
&lt;dbl&gt;</p></th>
<th style="text-align: left;"><p>type<br />
&lt;chr&gt;</p></th>
<th style="text-align: left;"><p>type2<br />
&lt;chr&gt;</p></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">1</td>
<td style="text-align: right;">10</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">1</td>
<td style="text-align: left;">BND</td>
<td style="text-align: left;">foldbackInversion</td>
</tr>
<tr>
<td style="text-align: left;">2</td>
<td style="text-align: right;">20</td>
<td style="text-align: right;">40</td>
<td style="text-align: right;">2</td>
<td style="text-align: left;">BND</td>
<td style="text-align: left;">foldbackInversion</td>
</tr>
<tr>
<td style="text-align: left;">3</td>
<td style="text-align: right;">30</td>
<td style="text-align: right;">50</td>
<td style="text-align: right;">5</td>
<td style="text-align: left;">BND</td>
<td style="text-align: left;">BND</td>
</tr>
</tbody>
</table>

3 rows
