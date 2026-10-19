---
title: bwa aln or bwa mem for short reads (36bp)
author: Ming Tang
date: '2017-06-15T12:01:00-07:00'
lastmod: '2017-06-15T12:01:00-07:00'
slug: bwa-aln-or-bwa-mem-for-short-reads-36bp
tags:
- bwa
- ChIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/06/bwa-aln-or-bwa-mem-for-short-reads-36bp.html
description: My ChIP-seq data are 36bp single end reads. I usually use bowtie1 for mapping ChIP-seq reads, but bowtie1 does not handle indels. Since I want to call…
summary: My ChIP-seq data are 36bp single end reads. I usually use bowtie1 for mapping ChIP-seq reads, but bowtie1 does not handle indels. Since I want to call…
header:
  caption: ''
  image: ''
---

My ChIP-seq data are 36bp single end reads. I usually use bowtie1 for mapping ChIP-seq reads, but bowtie1 does not handle indels. Since I want to call mutations on the ChIP-seq reads, I have to use another aligner [BWA](https://github.com/lh3/bwa), the most popular mapper written by Heng Li.

The github page says if reads \< 70bp, bwa aln should be used. Otherwise, bwa mem should be used.\
bwa mem is a more recent algorithm (should be better?).

I searched on biostar, and found [When and why is bwa aln better then bwa mem?](https://www.biostars.org/p/117225/)

I did a simulation test using [Teaser](https://github.com/Cibiv/Teaser) using default setting for each aligner.

The results are shown below:

![](/img/archive/bwa-aln-or-bwa-mem-for-short-reads-36bp/01-screenshot-2017-06-15-09.01.34.png)

The mapping rate:

![](/img/archive/bwa-aln-or-bwa-mem-for-short-reads-36bp/02-screenshot-2017-06-15-09.01.47.png)

Memory usage:

![](/img/archive/bwa-aln-or-bwa-mem-for-short-reads-36bp/03-screenshot-2017-06-15-09.03.48.png)

Run time:

![](/img/archive/bwa-aln-or-bwa-mem-for-short-reads-36bp/04-screenshot-2017-06-15-09.03.36.png)

Indeed, BWA aln is a little better than BWA mem for short reads.

For a real data set, the samtools flagstat results are shown below:

**bwa aln**:\
282967631 + 0 in total (QC-passed reads + QC-failed reads)\
0 + 0 secondary\
0 + 0 supplementary\
18963259 + 0 duplicates\
240660130 + 0 mapped (85.05% : N/A)\
0 + 0 paired in sequencing\
0 + 0 read1\
0 + 0 read2\
0 + 0 properly paired (N/A : N/A)\
0 + 0 with itself and mate mapped\
0 + 0 singletons (N/A : N/A)\
0 + 0 with mate mapped to a different chr\
0 + 0 with mate mapped to a different chr (mapQ\>=5)

**bwa mem**:\
282967631 + 0 in total (QC-passed reads + QC-failed reads)\
0 + 0 secondary\
0 + 0 supplementary\
18332921 + 0 duplicates\
236558306 + 0 mapped (83.60% : N/A)\
0 + 0 paired in sequencing\
0 + 0 read1\
0 + 0 read2\
0 + 0 properly paired (N/A : N/A)\
0 + 0 with itself and mate mapped\
0 + 0 singletons (N/A : N/A)\
0 + 0 with mate mapped to a different chr\
0 + 0 with mate mapped to a different chr (mapQ\>=5)

Indeed, bwa aln has a moderate higher mapping rate and a shorter run time for short 36bp reads.
