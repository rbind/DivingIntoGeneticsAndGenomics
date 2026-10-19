---
title: finding the closest feature given a set of Genomic coordinates
author: Ming Tang
date: '2013-09-24T10:52:00-07:00'
lastmod: '2013-09-24T10:52:00-07:00'
slug: finding-the-closest-feature-given-a-set-of-genomic-coordinates
tags:
- ChIP-seq
- Genomic Interval
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/finding-closest-feature-given-set-of.html
description: I have a bed file which contains peak information for a ChIP-seq data. I want to find the genes that are closest to these peaks.
summary: I have a bed file which contains peak information for a ChIP-seq data. I want to find the genes that are closest to these peaks.
header:
  caption: ''
  image: ''
---

I have a bed file which contains peak information for a ChIP-seq data. I want to find the genes that are closest to these peaks.

I am aware that many softwares can do this job. cistrome-Galaxy pipeline <http://cistrome.org/ap/> from shirley liu's lab in Harvard,  bioconductor package ChIPpeakAnno <http://www.bioconductor.org/packages/2.12/bioc/html/ChIPpeakAnno.html>\
are tools can deal with this kind of task. In addition to them, bedtool is a very good tool for Genomic Interval manipulation.  See a post here <http://www.biostars.org/p/53561/>

However, I want to go a little bit further to learn the Interval Tree data structure to deal with this kind of problem as mentioned in my last post and the post from Biostar.

I will have a look at the bx-python package.\
<http://www.biostars.org/p/99/>
