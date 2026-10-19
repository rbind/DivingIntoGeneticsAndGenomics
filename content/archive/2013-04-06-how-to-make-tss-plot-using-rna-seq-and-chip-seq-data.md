---
title: How to make TSS plot using RNA-seq and ChIP-seq data
author: Ming Tang
date: '2013-04-06T18:33:00-07:00'
lastmod: '2013-04-06T18:33:00-07:00'
slug: how-to-make-tss-plot-using-rna-seq-and-chip-seq-data
tags:
- ChIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/how-to-make-tss-plot-using-rna-seq-and.html
description: many times, we want to plot the ChIP-seq signal across the TSS in a genome wide scale.
summary: many times, we want to plot the ChIP-seq signal across the TSS in a genome wide scale.
header:
  caption: ''
  image: ''
---

many times, we want to plot the ChIP-seq signal across the TSS in a genome wide scale.

I figured out how to do it by several ways. Using  Encode K562 cells H3K4Me3 ChIP-seq data as an example 

1\.  Using HTSeq python package\
<http://www-huber.embl.de/users/anders/HTSeq/doc/tss.html#tss>

1\) Download the H3K4me3 ChIP-seq and RNA-seq data sets.\
<http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeCaltechRnaSeq/>

``` text
 wgEncodeCaltechRnaSeqK562R1x75dAlignsRep1V2.bam                        26-Jul-2010 18:31  1.4G
```

``` text
http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/
```

``` text
wgEncodeBroadHistoneK562H3k4me1StdAlnRep1.bam                   29-Oct-2010 11:03  919M
```

``` text
2) prepare the gtf file for hg19 version
```

``` text
http://genomewiki.ucsc.edu/index.php/Genes_in_gtf_or_gff_format
```

``` text
$ cat $HOME/.hg.conf
db.host=genome-mysql.cse.ucsc.edu
db.user=genomep
db.password=password

And set the permissions:

$ chmod 600 .hg.conf

Now you can use the command to extract GTF files directly from the UCSC database. For example, fetch the UCSC gene track from hg19 into the local file refGene.gtf:

$ genePredToGtf -utr hg19 refGene refGene.gtf

add -utr option to add 5'UTR information which contains the TSS position.

for RNA-seq data analysis we need the refGene.gtf (contains TSS and exons)

for ChIP-seq plotting at TSS, we need to extract TSSs from refGene.gtf

it can be done easily by linux command line

cat refGene.gtf | grep 5UTR > hg19.TSS.gtf

3)  group the genes to high, medium, low expression by analyzing the RNA-seq data

#RNA-Seq analysis

#The following block is to sort the gene expression data into three groups based on level of expression.

#Use python to count the number of tags of genes obtained via RNA-Seq and write that information into a new output file.

bam file is converted to sam file by samtools, because the htseq count script only takes sam file as input.

http://samtools.sourceforge.net/

samtools view -h -o K562RNAseqRep1V2.sam K562RNAseqRep1V2.bam

python -m HTSeq.scripts.count -s no K562RNASeqRep1V2.sam hg19_UTR_exon.gtf > K562_htseq.count.out

#stranded=count of how many tags in each gene. Add up all the counts for each exon.

#Once we have the K562_htseq_count.out file, first cound the number of genes that are there.

cat K562_htseq_count.out | head -n -5 > K562_htseq_count.out.clean # get rid of the last five lines which  are the summary of the count results

cat K562_htseq_count.out.clean | wc -l

#For the working file we had 23705 counts.

#So each group will have 1/3 of the total (7902 genes) and highest tag density will be the top 33% followed by mid 33% and low 33%.

#Now sort the results into three groups depending on tag density in Linux command line.

#Sort the file according to the second column and group into three groups.

cat K562_htseq.count.out.clean | sort -k2,2nr| head -7902 > top33_percent.txt

cat K562_htseq.count.out.clean | sort -k2,2nr| tail -15803 | head -7902 > mid33_percent.txt

cat K562_htseq.count.out.clean | sort -k2,2nr| tail -7902 > low33_percent.txt

4)python code:

update on 10/20/13, I put my code in gist and embed it here
```

{{< gist crazyhottommy 7069590 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/7069590)

``` text
it generates figure below.

2.  use the ngsplot package

http://code.google.com/p/ngsplot/

follow the installation instruction.

at command line

ngs.plot.r -G hg19 -R genebody -C  config.k562.txt -O K562.H3k4me3.genebody -T H3k4me3.genebody -L 3000 -FL 300

# this is different from the plotting in method 1, I am plotting across the gene body,

# you can also use -R tss  to produce the same plot above.

# the bam file must be sorted by samtools first

the config.k562.txt file is like this: tab delimited

K562H3k4me3.sorted.bam          top30_percent.txt           "High"

K562H3k4me3.sorted.bam           medium30_percent.txt   "med"

K562H3k4me3.sorted.bam           low30_percent.txt           "low"

it generates a tss plot and a heat map

===============================
```

``` text
update on 11/19/13
```

see this post on biostar http://www.biostars.org/p/83800/ it is very slow though, I tried it on my desktop (~4Gb Ram one core), it took some time to finish.

{{< gist daler 7041249 >}}

[View this gist on GitHub](https://gist.github.com/daler/7041249)
