---
title: RPKM/FPKM, TPM and raw counts for RNA-seq
author: Ming Tang
date: '2015-06-23T14:01:00-07:00'
lastmod: '2015-06-23T14:01:00-07:00'
slug: rpkm-fpkm-tpm-and-raw-counts-for-rna-seq
tags:
- FPKM
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/06/rpkmfpkm-tpm-and-raw-counts-for-rna-seq.html
description: There was a hot discussion that RPKM should not be used in the RNA-seq analysis:\ blahah404 Woohoo - death to FPKM and RPKM\
summary: There was a hot discussion that RPKM should not be used in the RNA-seq analysis:\ blahah404 Woohoo - death to FPKM and RPKM\
header:
  caption: ''
  image: ''
---

There was a hot discussion that RPKM should not be used in the RNA-seq analysis:\
[blahah404](http://twitter.com/blahah404 "Richard Smith-Unna")  Woohoo - death to FPKM and RPKM\![t.co/CyR3ht1dqi](https://t.co/CyR3ht1dqi "https://t.co/CyR3ht1dqi")

However [@BioMickWatson](https://twitter.com/BioMickWatson) has a different opinion, he thinks FPKM makes sense.\
[Why FPKM makes sense](https://biomickwatson.wordpress.com/2015/06/18/why-fpkm-makes-sense/)

what is RPKM or FPKM? see blog posts below:\
update on 07/23/2015, [see a youtube video ](https://www.youtube.com/watch?v=TTUrtCY2k-w&feature=youtu.be&a=) for great explanation of the three.\
[What is the FPKM? A review of RNA-Seq expression units](https://haroldpimentel.wordpress.com/2014/05/08/what-the-fpkm-a-review-rna-seq-expression-units/)\
[Counts vs. FPKMs in RNA-seq](http://www.cureffi.org/2013/09/12/counts-vs-fpkms-in-rna-seq/)\
[Paper: RPKM measure is inconsistent among samples](http://blog.nextgenetics.net/?e=51)\
[FPKM/RPKM normalization caveat and upper quartile normalization](http://vinaykmittal.blogspot.com/2013/10/fpkmrpkm-normalization-caveat-and-upper.html)\
Finally a post from Lior Pachter\
[Estimating number of transcripts from RNA-Seq measurements (and why I believe in paywall)](https://liorpachter.wordpress.com/2014/04/30/estimating-number-of-transcripts-from-rna-seq-measurements-and-why-i-believe-in-paywall/)

I personally use raw counts and let DESeq2 to deal with normalization.

update on 07/08/2016.\
Salmon and Kallisto outputs TPM, but if you want to convert counts to TPM, read this [biostar post](https://www.biostars.org/p/171766/), and my post [comparing salmon, kallisto and HTseq](/archive/comparing-salmon-kalliso-and-star-htseq-rna-seq-quantification/).

{{< gist slowkow c6ab0348747f86e2748b >}}

[View this gist on GitHub](https://gist.github.com/slowkow/c6ab0348747f86e2748b)
