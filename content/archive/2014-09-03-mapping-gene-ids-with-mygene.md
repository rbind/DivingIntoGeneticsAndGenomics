---
title: mapping gene ids with mygene
author: Ming Tang
date: '2014-09-03T08:14:00-07:00'
lastmod: '2014-09-03T08:14:00-07:00'
slug: mapping-gene-ids-with-mygene
tags:
- id mapping
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/09/mapping-gene-ids-with-mygene.html
description: Mapping gene ids is one of the routine jobs for bioinformatics. I was aware of several ways to do it including Biomart.
summary: Mapping gene ids is one of the routine jobs for bioinformatics. I was aware of several ways to do it including Biomart.
header:
  caption: ''
  image: ''
---

Mapping gene ids is one of the routine jobs for bioinformatics. I was aware of [several ways](https://www.biostars.org/p/22/) to do it including [Biomart](http://davetang.org/muse/2013/11/25/thoughts-converting-gene-identifiers/).

Update on 10/30/14, a mygene bioconductor package is online <http://bioconductor.org/packages/release/bioc/html/mygene.html>

Recently I got to know [mygene](https://pypi.python.org/pypi/mygene), a python wrapper for the [mygene.info](http://mygene.info/) services to map gene ids.\
I found it very handy to convert gene ids.  see a gist below.

{{< gist crazyhottommy bc0a8982df6a22957f3a >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/bc0a8982df6a22957f3a)

To use it,  cat input.txt \| python geneSymbol2Entrez.py \> output.txt\
or python geneSymbol2Entrez.py input.txt \> output.txt  where input.txt contains one gene name in each line. pretty neat!
