---
title: Detect mutations from RNA-seq data
author: Ming Tang
date: '2013-09-17T06:19:00-07:00'
lastmod: '2013-09-17T06:19:00-07:00'
slug: detect-mutations-from-rna-seq-data
tags:
- mutation
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/detect-mutation-from-rna-seq-data.html
description: Usually we detect mutations by DNA-sequencing or whole exome sequencing, GATK is the commonly used pipeline for this purpose.
summary: Usually we detect mutations by DNA-sequencing or whole exome sequencing, GATK is the commonly used pipeline for this purpose.
header:
  caption: ''
  image: ''
---

Usually we detect mutations by DNA-sequencing or whole exome sequencing, GATK <http://www.broadinstitute.org/gatk/guide/best-practices> is the commonly used pipeline for this purpose.

But RNA-seq data can also be used to call mutations in addition to detect the differentially expressed genes. However, the pipelines like GATK is not suitable for this kind of data.

A software

## **[rnaseqmut](https://github.com/davidliwei/rnaseqmut)**

was developed by Wei Li in Harvard Dana-Farber cancer Insitute for this purpose

See the post here\
<http://allaboutbioinfo.blogspot.com/2013/09/detecting-mutations-from-rna-seq.html?showComment=1379423505178#c719553107057687822>

github <https://github.com/davidliwei/rnaseqmut>
