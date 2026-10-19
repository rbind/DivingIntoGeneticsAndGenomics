---
title: speed up grep
author: Ming Tang
date: '2014-10-22T06:40:00-07:00'
lastmod: '2014-10-22T06:40:00-07:00'
slug: speed-up-grep
tags:
- grep
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/10/speed-up-grep.html
description: I had a list of gene names in txt file. There are around 500 genes with one gene name in one line, and I want to filter the gtf file from ensemble…
summary: I had a list of gene names in txt file. There are around 500 genes with one gene name in one line, and I want to filter the gtf file from ensemble…
header:
  caption: ''
  image: ''
---

I had a list of gene names in txt file. There are around 500 genes with one gene name in one line, and I want to filter the gtf file from ensemble [Homo_sapines.GRCh37.74gtf.gz](ftp://ftp.ensembl.org/pub/release-74/gtf/homo_sapiens/Homo_sapiens.GRCh37.74.gtf.gz)

the gtf file contains 2244857 lines. I used grep to do it, but it takes very long (~1 hour).

what I used:

zcat Homo_sapines.GRCh37.74gtf.gz \| grep -f gene_names.txt -w \> my_genes.gtf

I searched on line, and found several posts in stackoverflow to speed up grep:\
<http://stackoverflow.com/questions/14602963/faster-grep-function-for-big-27gb-files>\
<http://stackoverflow.com/questions/13913014/grepping-a-huge-file-80gb-any-way-to-speed-it-up>\
<http://stackoverflow.com/questions/9066609/fastest-possible-grep>

options to speed up:

1\) Prefix your grep command with `LC_ALL=C` to use the C locale instead of UTF-8.\
2) Use `fgrep` because you're searching for a fixed string, not a regular expression

I then used:

zcat Homo_sapines.GRCh37.74gtf.gz \| LC_ALL=C fgrep -f gene_names.txt -w \> my_genes.gtf

It runs much faster!
