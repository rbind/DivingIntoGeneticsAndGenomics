---
title: Awk Command to Count Total, Unique, and the Most Abundant Read in a FASTQ file
author: Ming Tang
date: '2013-05-16T08:11:00-07:00'
lastmod: '2013-05-16T08:11:00-07:00'
slug: awk-command-to-count-total-unique-and-the-most-abundant-read-in-a-fastq-file
tags:
- awk
- FASTQ
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/awk-command-to-count-total-unique-and.html
description: I was reading through a paper on comparative ChIP-Seq when I found this awk gem that lets you get some very basic stats very quickly on next generation…
summary: I was reading through a paper on comparative ChIP-Seq when I found this awk gem that lets you get some very basic stats very quickly on next generation…
header:
  caption: ''
  image: ''
---

### see post here:

<http://gettinggeneticsdone.blogspot.com/2012/04/awk-command-to-count-total-unique-and.html>

### 

### Awk Command to Count Total, Unique, and the Most Abundant Read in a FASTQ file

I was reading through a paper on comparative ChIP-Seq when I found this awk gem that lets you get some very basic stats very quickly on next generation sequencing reads. To use, simply `cat` the fastq file (or `gunzip -c`) and pipe that to this awk command:

`cat myfile.fq | awk '((NR-2)%4==0){read=$1;total++;count[read]++}END{for(read in count){if(!max||count[read]>max) {max=count[read];maxRead=read};if(count[read]==1){unique++}};print total,unique,unique*100/total,maxRead,count[maxRead],count[maxRead]*100/total}'`

The output would look something like this for some [RNA-seq data](https://main.g2.bx.psu.edu/datasets/0cd1292258e9a9be/display?to_ext=fastqsanger) downloaded from the [Galaxy RNA-seq tutorial](http://gettinggeneticsdone.blogspot.com/2011/11/guide-to-rna-seq-analysis-in-galaxy.html):

`99115 60567 61.1078 ACCTCAGGA 354 0.357161`

This is telling you:

1.  The total number of reads (99,115).
2.  The number of unique reads (60,567).
3.  The frequency of unique reads as a proportion of the total (61%).
4.  The most abundant sequence (useful for finding adapters, linkers, etc).
5.  The number of times that sequence is present (354).
6.  The frequency of that sequence as a proportion of the total number of reads (0.35%).

If you have a handful of fastq files in a directory and you'd like to do this for each of them, you can wrap this in a for loop in bash:

`` for read in `ls *.fq`; do echo -n "$read "; awk '((NR-2)%4==0){read=$1;total++;count[read]++}END{for(read in count){if(!max||count[read]>max) {max=count[read];maxRead=read};if(count[read]==1){unique++}};print total,unique,unique*100/total,maxRead,count[maxRead],count[maxRead]*100/total}' $read; done ``

This does the same thing, but adds an extra field at the beginning for the file name. I haven't yet figured out how to wrap this into GNU parallel, but the for loop should do the trick for multiple files.

Check out [FASTQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) for more extensive quality assessment.
