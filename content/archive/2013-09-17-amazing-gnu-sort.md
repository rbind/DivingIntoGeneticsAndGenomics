---
title: Amazing GNU sort
author: Ming Tang
date: '2013-09-17T19:32:00-07:00'
lastmod: '2013-09-17T19:32:00-07:00'
slug: amazing-gnu-sort
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/amazing-gnu-sort.html
description: 'I just came across this on Biostar:'
summary: 'I just came across this on Biostar:'
header:
  caption: ''
  image: ''
---

I just came across this on Biostar: <http://www.biostars.org/p/64687/>

Question: How to sort bed format file

2

Is there any tools or commands to sort bed format file? The correct order is chr1, chr2, chr3, chr4,..., chr22, chrX, chrY.

3

GNU sort can now sort in alpha-numeric order. See option "-V, --version-sort" of the following version "sort (GNU coreutils) 8.17".\
Here is an example:

``` text
$ echo -e "chr10\t1\t2\tA\nchr2\t1\t2\tB\nchr1\t1\t2\tC"
chr10   1       2       A
chr2    1       2       B
chr1    1       2       C
```

And the result is:

``` text
$ echo -e "chr10\t1\t2\tA\nchr2\t1\t2\tB\nchr1\t1\t2\tC" | sort -k1,1V
chr1    1       2       C
chr2    1       2       B
chr10   1       2       A
```
