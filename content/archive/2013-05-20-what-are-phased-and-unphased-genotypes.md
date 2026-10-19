---
title: what are phased and unphased genotypes?
author: Ming Tang
date: '2013-05-20T17:22:00-07:00'
lastmod: '2013-05-20T17:22:00-07:00'
slug: what-are-phased-and-unphased-genotypes
tags:
- variant calling
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/what-are-phased-and-unphased-genotypes.html
description: I was reading about the format of the VCF file\
summary: I was reading about the format of the VCF file\
header:
  caption: ''
  image: ''
---

I was reading about the format of the VCF file\
<http://www.1000genomes.org/node/101>

" Genotype data are given for three samples, two of which are phased and the third unphased, with per sample genotype quality, depth and haplotype qualities (the latter only for the phased samples) given as well as the genotypes. The microsatellite calls are unphased."

what do phased and unphased mean?

a google search:\
<http://www.biostars.org/p/7846/>

Phased data are ordered along one chromosome and so from these data you know the haplotype. Unphased data are simply the genotypes without regard to which one of the pair of chromosomes holds that allele.

Hi\
actually (I think) phased or unphased status is not related to any measure of quality. For each individual, there are two chromosomes labelled (arbitrarily when you do not have genotypes of the parents) paternal and maternal. The names are self-explanatory.\
For a haterozyguous genotype at a SNP position (which is called conditional on some quality score), you may know which allele is on the maternal chromosome and which one is on the paternal chromosome. The genotyped is "ordered". If you are able to assign, for a heterozyguous call (still conditional on the quality) at another SNP position which allele is on the paternal chromosome and which one is on the maternal, then you are able to phase these two SNPs - or more precisely, to phase the alleles at this SNPs. You then get an haplotype - or a suite of "ordered" SNPs.\
In this context, having ordered 0/1 at SNP1 and 1/0 at SNP 2 is not the same as having 0/1 at SNP 1 and 1/0 at SNP 2.\
First gives : 0 1 while second gives 0 0 ***\_\_ ***\_\_

``` text
           1   0                          1   1
```

Now, one could use some pre-estimated phase information on a panel population - typically different from the population where you call your alleles - to help calling an allele when the quality is low. This is what BEAGLECALL do, usually in a chip genotyping context.\
As for the 1000 G data, having the phased data helps getting a better estimate of linkage disequilibrium. This also means that the format may differ so you need to take care when you take this as an input. But besides input format and more info about LD, the way you may use phased and unphased here are not really different.\
Christian\
PS : sorry if I went too far to the basics
