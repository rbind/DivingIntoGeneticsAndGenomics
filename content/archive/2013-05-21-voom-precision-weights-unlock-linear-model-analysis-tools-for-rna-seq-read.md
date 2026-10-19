---
title: Voom! Precision weights unlock linear model analysis tools for RNA-Seq read counts
author: Ming Tang
date: '2013-05-21T18:19:00-07:00'
lastmod: '2013-05-21T18:19:00-07:00'
slug: voom-precision-weights-unlock-linear-model-analysis-tools-for-rna-seq-read
tags:
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/voom-precision-weights-unlock-linear.html
description: 'Voom: variance modelling at the observation-level\ In the past few years, RNA-seq has emerged as a revolutionary new technology for expression profiling.…'
summary: 'Voom: variance modelling at the observation-level\ In the past few years, RNA-seq has emerged as a revolutionary new technology for expression profiling.…'
header:
  caption: ''
  image: ''
---

<http://www.rna-seqblog.com/data-analysis/expression-tools/voom-precision-weights-unlock-linear-model-analysis-tools-for-rna-seq-read-counts/>

Voom: variance modelling at the observation-level\
In the past few years, RNA-seq has emerged as a revolutionary new technology for expression profiling. RNA-seq expression data consists of read counts, and many recent publications have argued therefore that RNA-seq data should be analysed by statistical methods designed specifically for counts. Yet all the statistical methods developed for RNA-seq counts rely on approximations of various kinds.\
This article revisits the idea of applying normal-based microarray-like statistical methods to RNA-seq read counts, with the idea that it is more important to model the mean-variance relationship correctly than it is to specify the exact probabilistic distribution of the counts. Log-counts per million are used as expression values. The voom method estimates the mean-variance relationship robustly and generates a precision weight for each individual normalized observation. The normalized log-counts per million and associated precision weights are then entered into the limma analysis pipeline, or indeed into any statistical pipeline for microarray data that is precision weight aware. This opens access for RNA-seq analysts to a large body of methodology developed for microarrays, allowing RNA-seq and microarray data to be analysed in closely comparable ways. The performance of voom and related limma-based pipelines is compared to that of edgeR, DESeq, baySeq, TSPM, PoissonSeq, and DSS. Simulation studies show that voom out-performs previous RNA-seq methods even when the data is generated according to the assumptions of the earlier methods. This is especially true when the sequence depths vary between RNA samples. Several data sets are also analysed to demonstrate how voom can handle heterogeneous data and complex experiments as well as facilitating pathway analysis and gene set testing methods.
