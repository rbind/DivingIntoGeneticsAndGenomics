---
title: Cutting out 500 columns from a 26G file using command line
author: Ming Tang
date: '2016-10-02T19:39:00-07:00'
lastmod: '2016-10-02T19:39:00-07:00'
slug: cutting-out-500-columns-from-a-26g-file-using-command-line
tags:
- big data
- unix
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/10/cutting-out-500-columns-from-26g-file.html
description: I have a 26 G tsv file with several thousand columns. I want to extract 500 columns from it based on the column names in another file.
summary: I have a 26 G tsv file with several thousand columns. I want to extract 500 columns from it based on the column names in another file.
header:
  caption: ''
  image: ''
---

I have a 26 G tsv file with several thousand columns. I want to extract 500 columns from it based on the column names in another file.

How should I do it? Reading into R may take forever, although one may recommend using [data.table](https://cran.r-project.org/web/packages/data.table/index.html) to fread in the data to save some time. However,  R is notorious for having to read in the data into memory. 26G is very big and my desktop does not have that power. [Handling large data sets in R](https://rpubs.com/msundar/large_data_analysis) may give you some alternatives to work with big data in R.

I decided to turn to the all-mighty unix commands.

{{< gist crazyhottommy ad479528c2cbc2679c3afb6439453dbd >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/ad479528c2cbc2679c3afb6439453dbd)

Since I may use it very often, I made it to a shell script and one can specify the separator to be comma or tab.

{{< gist crazyhottommy 039aa3818b1f0a9e3e9bf052ee5114e5 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/039aa3818b1f0a9e3e9bf052ee5114e5)

## 

Again, Unix commands are awesome!
