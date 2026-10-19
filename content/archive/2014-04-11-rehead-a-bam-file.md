---
title: rehead a bam file
author: Ming Tang
date: '2014-04-11T12:00:00-07:00'
lastmod: '2014-04-11T12:00:00-07:00'
slug: rehead-a-bam-file
tags:
- bam
- samtools
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/04/rehead-bam-file.html
description: I downloaded some ChIP-seq bam files from the UCSC genome browser
summary: I downloaded some ChIP-seq bam files from the UCSC genome browser
header:
  caption: ''
  image: ''
---

I downloaded some ChIP-seq bam files from the UCSC genome browser <http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeHaibTfbs/>

and I found some bam files headers  are without chrY information

`samtools view -H my.sorted.bam`\
`@SQ SN:chr1 LN:249250621`\
`@SQ SN:chr2 LN:243199373`\
`@SQ SN:chr3 LN:198022430`\
`@SQ SN:chr4 LN:191154276`\
`@SQ SN:chr5 LN:180915260`\
`@SQ SN:chr6 LN:171115067`\
`@SQ SN:chr7 LN:159138663`\
`@SQ SN:chr8 LN:146364022`\
`@SQ SN:chr9 LN:141213431`\
`@SQ SN:chr10 LN:135534747`\
`@SQ SN:chr11 LN:135006516`\
`@SQ SN:chr12 LN:133851895`\
`@SQ SN:chr13 LN:115169878`\
`@SQ SN:chr14 LN:107349540`\
`@SQ SN:chr15 LN:102531392`\
`@SQ SN:chr16 LN:90354753`\
`@SQ SN:chr17 LN:81195210`\
`@SQ SN:chr18 LN:78077248`\
`@SQ SN:chr19 LN:59128983`\
`@SQ SN:chr20 LN:63025520`\
`@SQ SN:chr21 LN:48129895`\
`@SQ SN:chr22 LN:51304566`\
`@SQ SN:chrX LN:155270560`\
`@SQ SN:chrM LN:16571`

for some downstream analysis, I have to rehead this bam file:

`samtools view -H my.bam | sed '24 a @SQ SN:chrY LN:59373566' | samtools reheader - my.bam > my.reheaded.bam`

sed to append a line "@SQ SN:chrY LN:59373566" at  the 24th line\
ctr+v+tab to insert a tab in the terminal.
