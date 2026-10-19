---
title: FPKMs and counts compared for RNA-seq data
author: Ming Tang
date: '2013-09-12T10:24:00-07:00'
lastmod: '2013-09-12T10:24:00-07:00'
slug: fpkms-and-counts-compared-for-rna-seq-data
tags:
- FPKM
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/fpkms-and-counts-compared-for-rna-seq.html
description: A very nice article comparing FPKMs and raw counts in RNA-seq data analysis
summary: A very nice article comparing FPKMs and raw counts in RNA-seq data analysis
header:
  caption: ''
  image: ''
---

A very nice article comparing  FPKMs and raw counts in RNA-seq data analysis

<http://www.cureffi.org/2013/09/12/counts-vs-fpkms-in-rna-seq/>\
"

1.  **counts** are simply the number of reads overlapping a given feature such as a gene.
2.  **FPKMs** or Fragments Per Kilobase of exon per Million reads are much more complicated.  Fragment means fragment of DNA, so the two reads that comprise a paired-end read count as one.  Per kilobase of exon means the counts of fragments are then normalized by dividing by the total length of all exons in the gene (or transcript).  This bit of magic makes it possible to compare Gene A to Gene B even if they are of different lengths.  Per million reads means this value is then normalized against the library size.  This bit of magic makes it possible to compare Gene A in Sample 1 to Sample 2 even if Sample 1′s RNA-seq library has 60 million pairs of reads and Sample 2′s library has only 30 million pairs of reads. "
