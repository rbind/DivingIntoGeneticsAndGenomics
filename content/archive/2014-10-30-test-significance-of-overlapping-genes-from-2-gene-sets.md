---
title: Test significance of overlapping genes from 2 gene sets
author: Ming Tang
date: '2014-10-30T10:23:00-07:00'
lastmod: '2014-10-30T10:23:00-07:00'
slug: test-significance-of-overlapping-genes-from-2-gene-sets
tags:
- hypergeometric
- python
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/10/test-significance-of-overlapping-genes.html
description: The question is very straightforward:\ I have three microarray or RNA-seq data sets (control, treatment1, treatment2, each should have several replicates…
summary: The question is very straightforward:\ I have three microarray or RNA-seq data sets (control, treatment1, treatment2, each should have several replicates…
header:
  caption: ''
  image: ''
---

The question is very straightforward:\
I have three microarray or RNA-seq data sets (control, treatment1, treatment2, each should have several replicates, otherwise you can not get a pvalue for differential expression!) , I then get two sets of differentially up-regulated genes ( treatment1 vs control, treatment2 vs control).  There are some overlapping genes which are both up-regulated by treatment1 and treatment2. How significant the overlapping is?

The easiest way is to use an online program <http://nemates.org/MA/progs/overlap_stats.html>\
The program calculates the p value by using [Hypergeometric_distribution](http://en.wikipedia.org/wiki/Hypergeometric_distribution).

see below for a python and an R script for this problem.

read  posts here <http://stats.stackexchange.com/questions/16247/calculating-the-probability-of-gene-list-overlap-between-an-rna-seq-and-a-chip-c>\
<http://blog.nextgenetics.net/?e=94>\
<https://www.biostars.org/p/90662/>

list1=3000, list2=400, Total gene number (population)=15000,\
overlapping between list1 and list2 =100

{{< gist crazyhottommy 67179a5f2925794a09d8 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/67179a5f2925794a09d8)

From the stackexchange link:\
"The p-value you want is the probability of getting 100 *or more* white balls in a sample of size 400 from an urn with 3000 white balls and 12000 black balls. Here are four ways to calculate it.

``` text
sum(dhyper(100:400, 3000, 12000, 400))
1 - sum(dhyper(0:99, 3000, 12000, 400))
phyper(99, 3000, 12000, 400, lower.tail=FALSE)
1-phyper(99, 3000, 12000, 400)
```

These give 0.0078.\
`dhyper(x, m, n, k)` gives the probability of drawing exactly `x`. In the first line, we sum up the probabilities for 100 – 400; in the second line, we take 1 minus the sum of the probabilities of 0 – 99.\
`phyper(x, m, n, k)` gives the probability of getting `x` or fewer, so `phyper(x, m, n, k)` is the same as `sum(dhyper(0:x, m, n, k))`.\
The `lower.tail=FALSE` is a bit confusing. `phyper(x, m, n, k, lower.tail=FALSE)` is the same as `1-phyper(x, m, n, k)`, and so is the probability of `x+1` or more "

If I run with the python script, I got the same p-value:

tommy@tommy-ThinkPad-T420 ~/scripts_general_use\
$ ./hyper_geometric.py 15000 3000 400 100 total number in population: 15000 total number with condition in population: 3000 number in subset: 400 number with condition in subset: 100 p-value \<= 100: 0.996049535673 p-value \>= 100: 0.00784035207977

The bottom line is that we need more statistics!!
