---
title: everyone will write a blog post on dplyr
author: Ming Tang
date: '2014-09-26T12:28:00-07:00'
lastmod: '2014-09-26T12:28:00-07:00'
slug: everyone-will-write-a-blog-post-on-dplyr
tags:
- awk
- dplyr
- gff
- unix
- R
- sed
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/09/everyone-will-write-blog-on-dplyr.html
description: 'dply is a nice R package to manipulate big data. There are several good tutorials I came across:'
summary: 'dply is a nice R package to manipulate big data. There are several good tutorials I came across:'
header:
  caption: ''
  image: ''
---

[dply](http://cran.r-project.org/web/packages/dplyr/index.html) is a nice R package to manipulate big data. There are several good tutorials I came across:

<http://www.dataschool.io/dplyr-tutorial-for-faster-data-manipulation-in-r/>\
<http://stat545-ubc.github.io/block009_dplyr-intro.html>\
<http://stat545-ubc.github.io/bit001_dplyr-cheatsheet.html>\
<http://gettinggeneticsdone.blogspot.com/2014/08/do-your-data-janitor-work-like-boss.html>

I followed the example in the early edition of [Bioinformatics Data Skills](http://shop.oreilly.com/product/0636920030157.do) on the dplyr part and put a gist below. The example used dplyr to manipulate and summarize the human annotation gff file downloaded here <http://useast.ensembl.org/info/data/ftp/index.html>.

I downloaded the ensemble 74 build which is different from the 75 build in the example of the book.\
I believe the [gencode v19](http://www.gencodegenes.org/releases/19.html) annotation file is based on ensemble 74.

I also included comments in the R code describing how to do the same job with linux command line.

{{< gist crazyhottommy 6b92d37f2ed2267cc75e >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/6b92d37f2ed2267cc75e)
