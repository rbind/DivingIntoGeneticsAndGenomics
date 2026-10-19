---
title: compare ChIP-seq data for different conditions and different transcription factors?
author: Ming Tang
date: '2013-10-03T12:42:00-07:00'
lastmod: '2013-10-03T12:42:00-07:00'
slug: compare-chip-seq-data-for-different-conditions-and-different-transcription
tags:
- ChIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/compare-chip-seq-data-for-different.html
description: I have done a lot of ChIP-seq data analysis, but most of them are just mapping to reference genome and then calling peaks (use MACS from Shirley liu's lab…
summary: I have done a lot of ChIP-seq data analysis, but most of them are just mapping to reference genome and then calling peaks (use MACS from Shirley liu's lab…
header:
  caption: ''
  image: ''
---

I have done a lot of ChIP-seq data analysis, but most of them are just mapping to reference genome and then calling peaks (use MACS from Shirley liu's lab at Harvard). A little bit more is to intersect different peak bed files with bedtools and to make heatmaps.

The reads mapping is much simpler and faster than RNA-seq data. The data are smaller (~5Gb compare to RNA-seq ~25Gb). I recently asked myself how to compare different ChIP-seq data for different conditions ( control vs knockdown, different developmental stages etc).

I quick google search:\
<http://seqanswers.com/forums/showthread.php?t=31048&highlight=compare+ChIP-seq>\
<http://www.biostars.org/p/10609/>\
<http://www.biostars.org/p/42291/>

The available packages are:\
"- ChIPDiff, which you already mentioned but which I haven't tried, but it was actually developed for histone marks so I'd be surprised if it didn't work for those?! I recall someone saying that there was some other issue with it (sorry I can't be more specific)

- DiffBind, which you also mentioned ([http://www.bioconductor.org/packages...iffBind.html);](http://www.bioconductor.org/packages/release/bioc/html/DiffBind.html);) it uses DESeq internally

- DBChIP (<http://pages.cs.wisc.edu/~kliang/DBChIP/>), which appears to use edgeR

- You can also use edgeR and DESeq directly. The DESeq paper shows you how to re-analyze differential TF binding data in the Kasowski et al Science paper (<http://www.sciencemag.org/content/328/5975/232.short>). "

After reading a little bit  <http://seqanswers.com/forums/showthread.php?t=31048&highlight=compare+ChIP-seq>, I decided to give diffReps <http://code.google.com/p/diffreps/> a try.  The author says it is suitable for both histone modifications and transcription factors.\
The paper was published in PLOSONE, it compares the results  with  that from DESeq and others.\
<http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0065598>\
The author Shen Li developed the other package ngsplot.

Another interesting R package is here <http://genomebiology.com/2013/14/4/R38>\
mentioned by me before [jMOSAiCS: joint analysis of multiple ChIP-seq datasets](/archive/jmosaics-joint-analysis-of-multiple-chip-seq-datasets/)\
I will have to give it a try also.

At the same time, I am reading the Nature protocol:\
<http://www.nature.com/nprot/journal/v7/n1/full/nprot.2011.420.html>

[Nat Protoc.](http://www.ncbi.nlm.nih.gov/pubmed/22179591# "Nature protocols.") 2011 Dec 15;7(1):45-61. doi: 10.1038/nprot.2011.420.

## A computational pipeline for comparative ChIP-seq analyses.

it gives you a much better understanding of how to make the comparisons.

=========================
update 11/19/13

A table from the newly published paper

## Practical Guidelines for the Comprehensive Analysis of ChIP-seq Data

<http://www.ploscompbiol.org/article/info:doi/10.1371/journal.pcbi.1003326>

**Table S3.  Software packages for the analysis of differential binding in ChIP-seq**.

|  |  |  |
|----|----|----|
| **Software tool** | **Availability** | **Notes** |
| **ChIPDiff \[36\]** | http://cmb.gis.a-star.edu.sg/ChIPSeq/paperChIPDiff.htm | Differential histone modification sites using a hidden Markov model |
| **Comparative ChIP-seq \[25\]** | http://www.starklab.org/data/bardet_natprotoc_2011/ | Fold change ratio between normalized peak heights |
| **DBChIP \[33\]** | http://pages.cs.wisc.edu/~kliang/DBChIP/ | Assigns uncertainty measures in a test of non-differential binding (uses edgeR) |
| **DESeq**<sup>§</sup> **\[31\]** | http://www.bioconductor.org/packages/release/bioc/html/DESeq.html | Test based on a model using the negative binomial distribution |
| **DiffBind** | http://www.bioconductor.org/packages/release/bioc/html/DiffBind.html | Differential binding affinity analysis (uses edgeR and DESeq) |
| **DIME \[35\]** | http://cran.r-project.org/web/packages/DIME/ | Differential identiﬁcation using mixtures ensemble |
| **edgeR**<sup>§</sup> **\[32\]** | http://www.bioconductor.org/packages/release/bioc/html/edgeR.html | Empirical Bayes estimation and exact tests based on the negative binomial distribution |
| **MACS \[17\] (version 2)** | https://github.com/taoliu/MACS/ | Differential peak detection based on paired four bedGraph files |
| **MAnorm \[34\]** | http://bcb.dfci.harvard.edu/~gcyuan/MAnorm/MAnorm.htm | Robust regression to derive a linear model |
| **MMDiff** | http://bioconductor.org/packages/release/bioc/html/MMDiff.html | Differences in shape using Kernel methods |
| **NarrowPeaks** | http://bioconductor.org/packages/release/bioc/html/NarrowPeaks.html | Shape-based analysis of variation using functional PCA |
| **POLYPHEMUS \[37\]** | http://cran.r-project.org/web/packages/polyphemus/ | Non-linear normalization on RNA Pol II profiling |

<sup>§</sup> Originally developed for gene expression count data.

=====================
updated on 02/06/2014

Two more useful links:\
<http://www.r-bloggers.com/methods-of-calling-differential-region-of-chip-seq/>\
<http://omictools.com/differential-peak-calling/>
