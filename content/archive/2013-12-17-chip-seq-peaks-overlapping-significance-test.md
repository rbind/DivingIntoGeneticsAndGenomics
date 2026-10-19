---
title: ChIP-seq peaks overlapping significance test
author: Ming Tang
date: '2013-12-17T19:06:00-08:00'
lastmod: '2013-12-17T19:06:00-08:00'
slug: chip-seq-peaks-overlapping-significance-test
tags:
- ChIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/12/chip-seq-peaks-overlapping-significance.html
description: 'The question is straightforward:'
summary: 'The question is straightforward:'
header:
  caption: ''
  image: ''
---

The question is straightforward:

I have two ChIP-seq bed files (transcription factor binding) obtained by MACS peak calling, and I want to know whether these two transcription factors co-bind to the same sites with a probability greater than expected.

The answer is not trivial.\
See a  post here <http://www.biostars.org/p/5484/>

update on 10/30/2014: one can use an R package for this [genometricorr](http://genometricorr.sourceforge.net/)

One can use bedtools shuffle <http://bedtools.readthedocs.org/en/latest/content/tools/shuffle.html> developed by Aaron Quinlan@UVA as suggested by the link above,  but it is not automatic.\
The coming bedtools2 seems to have this function implemented <http://www.ashg.org/2013meeting/abstracts/fulltext/f130122205.htm>

"In the context of biological discovery, the most exciting new functionality in bedtools2 is a comprehensive set of statistical measures for revealing associations between sets of genomic features (e.g., do these transcription factors co-associate more than expected by chance?). Here we present the new statistical tests in bedtools2, compare our tests to existing approaches such as the ENCODE Genome Structure Correction (GSC) metric, and provide needed insights into which metrics are most appropriate to common biological questions. We demonstrate typical misuses of these metrics and illustrate how our tests and associated visualization tools can reveal new biological insights. Given the speed and analytical flexibility of bedtools2, we anticipate that our new toolkit will be an invaluable resource for geneticists studying the impact of genetic variation and regulatory elements on human disease phenotypes."

I am looking forward to it.\
Luckily, I found other two tools that automate this process and are very handy to use.

poverlap <https://github.com/brentp/poverlap>\
and\
Genomic Association Tester (GAT) <http://www.cgat.org/~andreas/documentation/gat/contents.html>

I will use GAT as a demonstration since it is very well documented and seems to be more powerful.

Following the tutorial here <http://www.cgat.org/~andreas/documentation/gat/tutorialIntervalOverlap.html>\
I will need three bed files ready: a.bed file for TF1, b.bed file for TF2 and a workspace (genome) bed file that specifies the chromosome coordinates limits.

I have peak bed files for TFs, and I need to get the workspace bedfile.\
See a post here <http://www.biostars.org/p/16396/#16399>\
I downloaded the  [chromInfo.txt.gz](http://hgdownload.soe.ucsc.edu/goldenPath/mm9/database/chromInfo.txt.gz) file here ( I am working with mouse data)\
 <http://hgdownload.soe.ucsc.edu/goldenPath/mm9/database/>

Some manipulation to get the bed file:\
`tommy@tommy-ThinkPad-T420:~/mm9_genome_info$ cat chromInfo.txt`\
`chr1 197195432 /gbdb/mm9/mm9.2bit`\
`chr2 181748087 /gbdb/mm9/mm9.2bit`\
`chr3 159599783 /gbdb/mm9/mm9.2bit`\
`chr4 155630120 /gbdb/mm9/mm9.2bit`\
`chr5 152537259 /gbdb/mm9/mm9.2bit`\
`chr6 149517037 /gbdb/mm9/mm9.2bit`\
`chr7 152524553 /gbdb/mm9/mm9.2bit`\
`chr8 131738871 /gbdb/mm9/mm9.2bit`\
`chr9 124076172 /gbdb/mm9/mm9.2bit`\
`chr10 129993255 /gbdb/mm9/mm9.2bit`\
`chr11 121843856 /gbdb/mm9/mm9.2bit`\
`chr12 121257530 /gbdb/mm9/mm9.2bit`\
`chr13 120284312 /gbdb/mm9/mm9.2bit`\
`chr14 125194864 /gbdb/mm9/mm9.2bit`\
`chr15 103494974 /gbdb/mm9/mm9.2bit`\
`chr16 98319150 /gbdb/mm9/mm9.2bit`\
`chr17 95272651 /gbdb/mm9/mm9.2bit`\
`chr18 90772031 /gbdb/mm9/mm9.2bit`\
`chr19 61342430 /gbdb/mm9/mm9.2bit`\
`chrX 166650296 /gbdb/mm9/mm9.2bit`\
`chrY 15902555 /gbdb/mm9/mm9.2bit`\
`chrM 16299 /gbdb/mm9/mm9.2bit`\
`chr13_random 400311 /gbdb/mm9/mm9.2bit`\
`chr16_random 3994 /gbdb/mm9/mm9.2bit`\
`chr17_random 628739 /gbdb/mm9/mm9.2bit`\
`chr1_random 1231697 /gbdb/mm9/mm9.2bit`\
`chr3_random 41899 /gbdb/mm9/mm9.2bit`\
`chr4_random 160594 /gbdb/mm9/mm9.2bit`\
`chr5_random 357350 /gbdb/mm9/mm9.2bit`\
`chr7_random 362490 /gbdb/mm9/mm9.2bit`\
`chr8_random 849593 /gbdb/mm9/mm9.2bit`\
`chr9_random 449403 /gbdb/mm9/mm9.2bit`\
`chrUn_random 5900358 /gbdb/mm9/mm9.2bit`\
`chrX_random 1785075 /gbdb/mm9/mm9.2bit`\
`chrY_random 58682461 /gbdb/mm9/mm9.2bit`

`tommy@tommy-ThinkPad-T420:~/mm9_genome_info$ cat chromInfo.txt | cut -f1-2 | awk '{print $1"\t"0"\t"$2}'`\
`chr1 0 197195432`\
`chr2 0 181748087`\
`chr3 0 159599783`\
`chr4 0 155630120`\
`chr5 0 152537259`\
`chr6 0 149517037`\
`chr7 0 152524553`\
`chr8 0 131738871`\
`chr9 0 124076172`\
`chr10 0 129993255`\
`chr11 0 121843856`\
`chr12 0 121257530`\
`chr13 0 120284312`\
`chr14 0 125194864`\
`chr15 0 103494974`\
`chr16 0 98319150`\
`chr17 0 95272651`\
`chr18 0 90772031`\
`chr19 0 61342430`\
`chrX 0 166650296`\
`chrY 0 15902555`\
`chrM 0 16299`\
`chr13_random 0 400311`\
`chr16_random 0 3994`\
`chr17_random 0 628739`\
`chr1_random 0 1231697`\
`chr3_random 0 41899`\
`chr4_random 0 160594`\
`chr5_random 0 357350`\
`chr7_random 0 362490`\
`chr8_random 0 849593`\
`chr9_random 0 449403`\
`chrUn_random 0 5900358`\
`chrX_random 0 1785075`\
`chrY_random 0 58682461`

`tommy@tommy-ThinkPad-T420:~/Tet1/mm9_genome_info$ cat chromInfo.txt | cut -f1-2 | awk '{print $1"\t"0"\t"$2}' > mm9_contigs.bed`

Notice the weird chromosome names like chrUn and chr4_random.  See a post here <http://www.biostars.org/p/9577/>

- The chr\*\_random sequences are unplaced sequence on those reference chromosomes.
- The chrUn\_\* sequences are unlocalized sequences where the corresponding reference chromosome has not been determined

Now, I have all the files ready, I will do the significance test by GAT:\
On HPC@UFL:

`[mtang@dev1 mm9_genome_info]$ gat-run.py --segments=a.bed --annotations=b.bed --workspace=mm9_contigs.bed --ignore-segment-tracks --num-samples=1000 --log=gat.log > gat.tsv`

a.bed file contains about 30,000 peaks and  b.bed file contains 80,000 peaks. It took me 1 min to finish 1000 simulations:

|  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|
| observed | expected | CI95low | CI95high | stddev | fold | l2fold | pvalue | qvalue |
| 3998559 | 282473.979 | 267444 | 298581 | 9379.0988 | 14.1554 | 3.8233 | 1.0000e-03 | 1.0000e-03 |

For 1000 simulation, I got a minimal p value 0.001 indicating that these two TFs co-occupy the same genomic sites not randomly.
