---
title: VCF file annotation and manipulation tools
author: Ming Tang
date: '2013-06-29T05:14:00-07:00'
lastmod: '2013-06-29T05:14:00-07:00'
slug: vcf-file-annotation-and-manipulation-tools
tags:
- NGS
- VCF
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/vcf-file-annotation-and-manipulation.html
description: VCF (variant calling format) file, as specified in 1000 Genome \ is a common file to handle the variant information from the NGS sequencing data.
summary: VCF (variant calling format) file, as specified in 1000 Genome \ is a common file to handle the variant information from the NGS sequencing data.
header:
  caption: ''
  image: ''
---

VCF (variant calling format) file, as specified in 1000 Genome <http://www.1000genomes.org/wiki/Analysis/Variant%20Call%20Format/vcf-variant-call-format-version-41>\
is a common file to handle the variant information from the NGS sequencing data.

many tools have been developed to manipulate, extract information with this format.\
some popular ones are:\
tabix <http://samtools.sourceforge.net/tabix.shtml> for fast indexing and extracting certain regions of the whole genome.\
VCFtools <http://vcftools.sourceforge.net/> for many more manipulations including annotating, merging, concatenating, comparing etc.\
bedtools <http://code.google.com/p/bedtools/> for genomic region overlapping calculation. etc\
I have some experience with the above three tools.

by the way, I have not used the GATK tools <http://www.broadinstitute.org/gatk/>from the broad Institute yet, I am sure I will have a try sometime later.

after google, I found several more:\
variationtoolkit <http://code.google.com/p/variationtoolkit/>\
vcflib <https://github.com/ekg/vcflib>\
varianttools <http://varianttools.sourceforge.net/>\
variantannotation <http://www.bioconductor.org/packages/2.12/bioc/html/VariantAnnotation.html>\
Plinkseq <http://atgu.mgh.harvard.edu/plinkseq/>

Taser is the one I just came across <http://www.zhanxw.com/taser/>\
It is based on R and can extract variant info by gene names fairly easily

Again, there are so many tools out there. Depending on your needs, choose the right one for you.
