---
title: define intronic, exonic and intergenic regions using GRanges
author: Ming Tang
date: '2016-11-14T14:17:00-08:00'
lastmod: '2016-11-14T14:17:00-08:00'
slug: define-intronic-exonic-and-intergenic-regions-using-granges
tags:
- bedtools
- GRanges
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/11/define-intronic-exonic-and-intergenic.html
description: You might want to use the bedtools for solutions. see Dave Tang's post.
summary: You might want to use the bedtools for solutions. see Dave Tang's post.
header:
  caption: ''
  image: ''
---

You might want to use the bedtools for solutions. see [Dave Tang's post](http://davetang.org/muse/2013/01/18/defining-genomic-regions/).

I am going to use GRanges to do exactly the same thing. However, do bear in mind that R uses 1 based coordinate system.  bed files in Granges are 1 based.  bedtools uses 0 based coordinate system for bed files. It could be very confusing and one can make 1 base-off mistake easily.

when you have a bed file in 0 based coordinate system, if you read it into R by using import function in rtracklayer package, it will convert to 1 based internally. However, if you read the 0 based bed file into R by read.table and want to convert the dataframe to GRanges, do not forget to add 1 to the start before converting to GRanges.

{{< gist crazyhottommy 4681a30700b2c0c1ee02cbc875e7c4e9 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/4681a30700b2c0c1ee02cbc875e7c4e9)
