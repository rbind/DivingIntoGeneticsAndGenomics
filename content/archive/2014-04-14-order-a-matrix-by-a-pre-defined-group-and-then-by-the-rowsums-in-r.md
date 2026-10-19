---
title: order a matrix by a pre-defined group and then by the rowSums in R
author: Ming Tang
date: '2014-04-14T13:54:00-07:00'
lastmod: '2014-04-14T13:54:00-07:00'
slug: order-a-matrix-by-a-pre-defined-group-and-then-by-the-rowsums-in-r
tags:
- heatmap
- matrix
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/04/order-matrix-by-pre-defined-group-and.html
description: I will lay down the R code below
summary: I will lay down the R code below
header:
  caption: ''
  image: ''
---

I will lay down the R code below

\#create a matrix

``` r
> mat<- matrix(sample.int(10, size=30, replace=T), ncol=3)
> mat
      [,1] [,2] [,3]
 [1,]    4    7    6
 [2,]    5    6    1
 [3,]    4    3    1
 [4,]    2    2    9
 [5,]    4    5    4
 [6,]    8    9    8
 [7,]    5    9    7
 [8,]    8    6    7
 [9,]    3    8    5
[10,]    4    3    6
```

``` r
# column bind the mat and the sum of the rows together
```

``` r
> mat1<- cbind(mat, rowSums(mat))
> mat1
      [,1] [,2] [,3] [,4]
 [1,]    4    7    6   17
 [2,]    5    6    1   12
 [3,]    4    3    1    8
 [4,]    2    2    9   13
 [5,]    4    5    4   13
 [6,]    8    9    8   25
 [7,]    5    9    7   21
 [8,]    8    6    7   21
 [9,]    3    8    5   16
[10,]    4    3    6   13

# order the matrix by the rowSums(mat)
> mat[order(rowSums(mat)),]
      [,1] [,2] [,3]
 [1,]    4    3    1
 [2,]    5    6    1
 [3,]    2    2    9
 [4,]    4    5    4
 [5,]    4    3    6
 [6,]    3    8    5
 [7,]    4    7    6
 [8,]    5    9    7
 [9,]    8    6    7
[10,]    8    9    8

# compare with the order by looking at the last column
> mat1[order(rowSums(mat)),]
      [,1] [,2] [,3] [,4]
 [1,]    4    3    1    8
 [2,]    5    6    1   12
 [3,]    2    2    9   13
 [4,]    4    5    4   13
 [5,]    4    3    6   13
 [6,]    3    8    5   16
 [7,]    4    7    6   17
 [8,]    5    9    7   21
 [9,]    8    6    7   21
[10,]    8    9    8   25

# order the mat matrix in a decreasing order by adding a minus sign

> mat[order(-rowSums(mat)),]
      [,1] [,2] [,3]
 [1,]    8    9    8
 [2,]    5    9    7
 [3,]    8    6    7
 [4,]    4    7    6
 [5,]    3    8    5
 [6,]    2    2    9
 [7,]    4    5    4
 [8,]    4    3    6
 [9,]    5    6    1
[10,]    4    3    1

# label the rows into different groups

> rownames(mat)<- c("f","a","d","c","b","d","e","a","f","f")
> mat
  [,1] [,2] [,3]
f    4    7    6
a    5    6    1
d    4    3    1
c    2    2    9
b    4    5    4
d    8    9    8
e    5    9    7
a    8    6    7
f    3    8    5
f    4    3    6

> rownames(mat1)<- c("f","a","d","c","b","d","e","a","f","f")

> mat1
  [,1] [,2] [,3] [,4]
f    4    7    6   17
a    5    6    1   12
d    4    3    1    8
c    2    2    9   13
b    4    5    4   13
d    8    9    8   25
e    5    9    7   21
a    8    6    7   21
f    3    8    5   16
f    4    3    6   13

> groups<- factor(rownames(mat))

> groups
 [1] f a d c b d e a f f
Levels: a b c d e f

# order the matrix by group first, and then within the subgroups, order by the rowSums

> mat1[order(groups,rowSums(mat)),]
  [,1] [,2] [,3] [,4]
a    5    6    1   12
a    8    6    7   21
b    4    5    4   13
c    2    2    9   13
d    4    3    1    8
d    8    9    8   25
e    5    9    7   21
f    4    3    6   13
f    3    8    5   16
f    4    7    6   17

It is extremely useful when I want to order the matrix by a pre-defined way and then plot the heatmap with heatmap2 in gplots package.
```
