---
title: how to get a genome-wide motif bed file
author: Ming Tang
date: '2014-02-13T07:13:00-08:00'
lastmod: '2014-02-13T07:13:00-08:00'
slug: how-to-get-a-genome-wide-motif-bed-file
tags:
- bed
- ChIP-seq
- motif
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/02/how-to-get-genome-wide-motif-bed-file.html
description: Someone was asking this question on Seqanswers
summary: Someone was asking this question on Seqanswers
header:
  caption: ''
  image: ''
---

Someone was asking this question on Seqanswers <http://seqanswers.com/forums/showthread.php?t=40762&highlight=genome+motif+bed>

For motif analysis, the most popular program is MEME <http://meme.nbcr.net/meme/>\
There are a bunch of tools in the suites including some useful ones for ChIP-seq\
I also saw RAST <http://rsat01.biologie.ens.fr/rsa-tools/index.html> and oPOSSUM were mentioned <http://opossum.cisreg.ca/oPOSSUM3/>

I analyze ChIP-seq data a lot. Usually, one gets a bed file containing the positions of the peaks. However, many motif analysis programs require fasta file as input.

There are many ways to get fasta file based on coordinates.\
See:\
<http://www.biostars.org/p/7481/>\
and my previous post [batch converting coordinates to sequences](/archive/batch-converting-coordinates-to-sequences/)

Going back to the question, I've found several ways to get a bed file containing the coordinates for the motifs.

1. Homer homepage\
<http://homer.salk.edu/homer/>\
at the very bottom, there are several links for human and mouse .The file contains all the known motif coordinates for the whole genome. Files are big (several Gb). one can get the specific motifs occurrences by grep.

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/01-selection_069.png)

2.  from UCSC software <http://genome.ucsc.edu/ENCODE/analysisTools.html>\
ENCODE-motifs  <http://compbio.mit.edu/encode-motifs/>\
at the bottom of the page:

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/02-selection_071.png)

only the human data are available.

3. Motif-map <http://motifmap.ics.uci.edu/>\
click motif search

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/03-selection_072.png)

Several more organisms are supported

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/04-selection_073.png)

search the motif you want ( I use CTCF as an example)

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/05-selection_074.png)

Click save on the bottom right.

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/06-selection_075.png)

Click my motifs on the upper right.

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/07-selection_076.png)

export to bed or other format files.

![](/img/archive/how-to-get-a-genome-wide-motif-bed-file/08-selection_077.png)
