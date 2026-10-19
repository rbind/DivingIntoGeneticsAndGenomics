---
title: Understanding the Forward strand and Reverse strand  and the coordinates systems
author: Ming Tang
date: '2014-08-07T09:05:00-07:00'
lastmod: '2014-08-07T09:05:00-07:00'
slug: understanding-the-forward-strand-and-reverse-strand-and-the-coordinates-systems
tags:
- coordinates
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/08/understanding-forward-strand-and.html
description: we are on day 4 of the MSU NGS course. In the morning, instructor Istvan introduced Genomic Intervals. To understand the coordinates system, one needs to…
summary: we are on day 4 of the MSU NGS course. In the morning, instructor Istvan introduced Genomic Intervals. To understand the coordinates system, one needs to…
header:
  caption: ''
  image: ''
---

we are on day 4 of the [MSU NGS course](http://bioinformatics.msu.edu/ngs-summer-course-2014). In the morning, instructor Istvan introduced Genomic Intervals. To understand the coordinates system, one needs to understand the strandness of DNA.

sense strand is the coding strand\
anti-sense strand is the reverse-complementary strand of the coding strand\
see details below:\
<https://www.biostars.org/p/3423/>\
<https://www.biostars.org/p/3908/>\
again, everything is on biostar :)

I drew a picture to better understand it

![](/img/archive/understanding-the-forward-strand-and-reverse-strand-and-the-coordinates-systems/01-strandness.jpg)

remember:\
coordinates are reported 5'---\> 3'  forward strand\
transcription occurs from 5' to 3'\
forward/plus strand and reverse/reverse strand are designated arbitrarily.\
Imagine that you can flip over the example I drew, then gene A would be in minus strand.

\# 0-based and 1-based coordinates system

0 based and 1 based coordinates  cheat sheet\
<https://www.biostars.org/p/84686/>

various formats: <http://genome.ucsc.edu/FAQ/FAQformat.html>\
GFF3 specification: <http://www.sequenceontology.org/gff3.shtml>\
0-based formats:BED, wiggle, BEDGRAPH\
1-based formats: GFF, GTF, GBK (genebank file), SAM, VCF

\# lift over coordinates\
lift-over between different versions of genome <https://genome.ucsc.edu/util.html>\
Generally do not do it, just map to the right version of interest.\
By the way, the latest human genome GRCh38 is released: <http://www.ensembl.info/blog/2014/08/07/ensembl-76-has-been-released/>
