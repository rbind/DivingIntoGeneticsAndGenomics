---
title: remap the Eland sorted.txt file from hg18 to hg19
author: Ming Tang
date: '2013-06-07T13:51:00-07:00'
lastmod: '2013-06-07T13:51:00-07:00'
slug: remap-the-eland-sorted-txt-file-from-hg18-to-hg19
tags:
- bowtie
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/remap-eland-sortedtxt-file-from-hg18-to.html
description: Sometime ago, I wrote a blog about how to convert Eland sorted.txt file to bam file\ convert Eland sorted.txt to bam file
summary: Sometime ago, I wrote a blog about how to convert Eland sorted.txt file to bam file\ convert Eland sorted.txt to bam file
header:
  caption: ''
  image: ''
---

Sometime ago, I wrote a blog  about how to convert Eland sorted.txt file to bam file\
[convert Eland sorted.txt to bam file](/archive/convert-eland-sorted-txt-to-bam-file/)

the original Eland file was mapped to hg18, but I want to remap it to hg19.

essentially, I need a fastq file to map it by bowtie.

A quick google search:\
<http://seqanswers.com/forums/showthread.php?t=7061>

so, the bamtools <https://github.com/pezmaster31/bamtools> (yes, github again...that's why I want to learn git a little bit, see my last post [My first try on Git](/archive/my-first-try-on-git/))

and picard <http://picard.sourceforge.net/> can do the job.

\# you need to module load bamtools in the HPC terminal

`$ bamtools convert -in hg18.bam -format fastq > my.fastq`

then, I run bowtie as what I did here [SRA file and my first Bowtie run on UF HPC (high performance computing center)](/archive/sra-file-and-my-first-bowtie-run-on-uf-hpc-high-performance-computing-center/)  at UF HPC .

after mapping with bowtie, I got a my_hg19.sam  file, I converted it to bam file and it is ready for downstream analysis.

`$ samtools view -bS my_hg19.sam > hg19.bam`
