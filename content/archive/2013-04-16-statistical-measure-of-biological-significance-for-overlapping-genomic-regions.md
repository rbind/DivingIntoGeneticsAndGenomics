---
title: Statistical measure of biological significance for overlapping genomic regions
author: Ming Tang
date: '2013-04-16T18:44:00-07:00'
lastmod: '2013-04-16T18:44:00-07:00'
slug: statistical-measure-of-biological-significance-for-overlapping-genomic-regions
tags:
- ChIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/statistical-measure-of-biological.html
description: updated 06/25/13 look at this python package, it answered this question
summary: updated 06/25/13 look at this python package, it answered this question
header:
  caption: ''
  image: ''
---

updated 06/25/13  look at this python package, it answered this question

<http://www.cgat.org/~andreas/documentation/gat/>

the paper published in bioinformatics

## GAT: a simulation framework for testing the association of genomic intervals

--------------------------------------------------------------------------------------------------------

I had the same question as the title of this blog.

simply google I found here:\
<http://www.biostars.org/p/8480/>\
and here:\
<http://www.biostars.org/p/5484/>

<https://github.com/brentp/bio-playground/blob/master/utils/list_overlap_p.py>\
the python script for the hypergeometric test

``` text
import optparse

import sys

import os.path as op

import scipy.stats as ss

def hypergeom(m, n, n1, n2, p=False):

    """

    >>> hypergeom(1, 1000, 1000, 1000) # has to be shared.

    1.0

    >>> all(hypergeom(i, 1000, 1000, 1000) == 1.0 for i in range(100))

    True

    >>> hypergeom(1, 30000, 20, 20)

    0.013253396616299651

    >>> hypergeom(2, 30000, 20, 20)

    7.9649366037104485e-05

    >>> hypergeom(11, 30000, 20, 20)

    4.516176321800458e-11

    >>> hypergeom(10, 30000, 20, 20) # very low prob.

    4.516176321800458e-11

    >>> hypergeom(20, 30000, 20, 20) # very low chance that all are shared.

    4.516176321800458e-11

    """

    if m <= 0: return 1.0

    mmin = m - 1

    mmax = min(n1, n2)

    return ss.hypergeom.cdf(mmax, n, n1, n2) - ss.hypergeom.cdf(mmin, n, n1, n2)

"Also, checkout randomstats in pybedtools by Ryan Dale."

R package ChIPpeakAnno can do similar job

http://www.bioconductor.org/packages/2.11/bioc/html/ChIPpeakAnno.html

R code:

library(ChIPpeakAnno)
TF1.bed<-  read.table(file="TF1.txt", header=FALSE)
TF1.rangedData=BED2RangedData(TF1.bed)

TF2.bed<- read.table(file="TF2.txt", header=FALSE)
TF2.rangedData=BED2RangedData(TF2.bed)

t1<- findOverlappingPeaks(TF1.rangedData, TF2.rangedData, maxgap=200, NameOfPeaks1="TF1", NameOfPeaks="TF2", select="all")
makeVennDiagram(RangedDataList(TF1.rangedData, TF2.rangedData), NameOfPeaks=c("TF1", "TF2"), maxgap=0, totalTest=50000, cex=1, counts.col="red",useFeature=FALSE)

here a discussion about the choose of number of totaltest

https://stat.ethz.ch/pipermail/bioconductor/2010-November/036493.html

"It would be helpful to describe your problem and post to the whole message board.  (There are many experts who probably can be more helpful than myself :-))  That said, I think you are referring to the "NaN" error and below are my thoughts (Julie Zhu also answered this a couple of times and her reply is probably in the archives).

When calling the makeVennDiagram function you want to set the totalTest number to something that is larger than the experimentally determined peak number.  As far as I know, the totalTest number is used for the hypergeometric sampling that is used to determine if the overlap between two datasets is more than would be expected by chance.  So one way to sort this out using biological information is to think about the maximum number of possible binding events and use that as the totalTest number.  For example, if you are studying a sequence-specific DNA binding protein with a known motif you could count that number of times that motif occurs in the genome and compare that to the number of peaks you have experimentally determined.

Motifs = 500
Peaks = 200
Peaks w/ motif = 180 (90%)
"upper limit"    = 500
new "upper limit" for totalTest = .9 x 500 = 450

Now if your working with a sequence-independent binding factor it can get tricky.  One approach would be to determine the mean peak width.  Then divide the whole genome sequence by this number to get an upper limit.  This is probably way to high so using additional information such as if the protein binds intergenic or ORFs could bring the number down but make it more relevant to the biological experiment.  For example:

peaks   = 75
intergenic peaks = 70
ORF peaks  = 5
mean peak width = 50 base pairs
genome size  = 10000 base pairs
"upper limit"   = 10000/ 50 = 200 (possible peaks)
intergenic seq  = 4000 base pairs
new "upper limit" =  4000/50 = 80 (possible intergenic peaks)

I was working with something more like the second case and I felt the totalTest based on the total genome was quite relaxed and based on the intergenic sequence only was quite stringent so somewhere in the middle might be better but most importantly I feel I am standing on some solid biological reasoning  for determining the amount of sampling.

Hope this helps and I would be interested to here if anybody has some critiques of this approach or additional suggestions.

Best,

Noah

"
```
