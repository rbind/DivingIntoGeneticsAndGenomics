---
title: qseq to fastq conversion
author: Ming Tang
date: '2014-03-18T19:08:00-07:00'
lastmod: '2014-03-18T19:08:00-07:00'
slug: qseq-to-fastq-conversion
tags:
- FASTQ
- qseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/03/qseq-to-fastq-conversion.html
description: I was looking at some old ChIP-seq data with raw file in qseq format. I want to convert them to fastq file for mapping with bowtie.
summary: I was looking at some old ChIP-seq data with raw file in qseq format. I want to convert them to fastq file for mapping with bowtie.
header:
  caption: ''
  image: ''
---

I was looking at some old ChIP-seq data with raw file in qseq format. I want to convert them to fastq file for mapping with bowtie.

A quick google I found:\
<http://www.biostars.org/p/6682/>

the qseqtofastq C++ program <http://www.dna.bio.keio.ac.jp/~krisp/qseq2fastq/> has to be compiled by scons, and I encountered some compiling problem. I gave it up and used this java program <http://sourceforge.net/projects/snpeff/files/qseq2fastq.jar/download>

Usage:

`zcat myfile.qseq.tgz | java -jar qseqtofastq.jar -phred64 > myfile.fastq`

it took me around 1 hour to finish the conversion of a 600MB tgz file on the computing cluster (single cpu 4GB ram).

I did have a problem at the end:

`Exception in thread "main" java.lang.RuntimeException: java.lang.ArrayIndexOutOfBoundsException: 8`\
` at ca.mcgill.mcb.pcingola.Qseq2Fastq.main(Qseq2Fastq.java:51)`\
`Caused by: java.lang.ArrayIndexOutOfBoundsException: 8`\
` at ca.mcgill.mcb.pcingola.Qseq2Fastq.main(Qseq2Fastq.java:43)`

I counted the lines of the fastq file (divided by 4) and the original qseq file  , and they are equal.  So, I went ahead and mapped the fastq file with bowtie.
