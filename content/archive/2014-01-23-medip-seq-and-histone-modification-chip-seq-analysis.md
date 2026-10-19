---
title: MeDIP-seq and histone modification ChIP-seq analysis
author: Ming Tang
date: '2014-01-23T12:39:00-08:00'
lastmod: '2014-01-23T12:39:00-08:00'
slug: medip-seq-and-histone-modification-chip-seq-analysis
tags:
- ChIP-seq
- differential peaks
- diffReps
- MeDIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/01/medip-seq-and-histone-modification-chip.html
description: 'I was reading a cell paper:'
summary: 'I was reading a cell paper:'
header:
  caption: ''
  image: ''
---

I was reading a cell paper:

## Dnmt3L Antagonizes DNA Methylation at Bivalent Promoters and Favors DNA Methylation at Gene Bodies in ESCs

<http://www.sciencedirect.com.lp.hscl.ufl.edu/science/article/pii/S0092867413010878>\
I want to re-analyze the MeDIP-seq data and the histone modification ChIP-seq data.

I downloaded the MeDIP-seq data here: <http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE44644>\
and the H3k4me3 and H3k27me3 ChIP-seq data here: <http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE15519>

The paper claimed that Dnmt3L maintains DNA hypomethylation at bivalent promoters ( with both H3k4me3 and H3k27me3 marks).  The authors used a Dnmt3L deficient mouse ES cell line and did MeDIP-seq on it. They found that many bivalent promoters gain DNA methylation in Dnmt3L negative cells. In contrast, DNA methylation loss mainly occurs in gene body.

I will work on the histone modification ChIP-seq data first.\
I took a look at many different H3k4me3 and H3k27me3 ChIP-seq data sets in mES cells. H3k4me3 ChIP-seq data quality is usually very good with very specific and sharp peaks after MACS peak calling. However, H3k27me3 data are not that great. I did regular H3k4me3 ChIP, the antibody is very good and the enrichment of input is usually above 10% in the active loci I tested.

Similar to H3k36me3, an active mark in gene body, H3k27me3 (repressive) mark usually form broader but less-enriched regions. I played around with the MACS argument setting.

I will skip the process of transforming sra format data to fastq and mapping them to the reference genome. I have several posts for that already:\
[The Backtick in the linux command](/archive/the-backtick-in-the-linux-command/)\
bowtie for ChIp-seq [SRA file and my first Bowtie run on UF HPC (high performance computing center)](/archive/sra-file-and-my-first-bowtie-run-on-uf-hpc-high-performance-computing-center/)\
Tophat for RNA-seq [RNA-seq analysis samtools sort and htseq-count](/archive/rna-seq-analysis-samtools-sort-and-htseq-count/)

For H3k4me3 data, I only downloaded one, and called peaks:\
`[mtang@dev1 H3k4me3]$ macs -t SRR038984.sam -n initial_test -g mm`\
`INFO @ Thu, 23 Jan 2014 13:37:47:`\
`# ARGUMENTS LIST:`\
`# name = initial_test`\
`# format = AUTO`\
`# ChIP-seq file = SRR038984.sam`\
`# control file = None`\
`# effective genome size = 1.87e+09`\
`# band width = 300`\
`# model fold = 10,30`\
`# pvalue cutoff = 1.00e-05`\
`# Large dataset will be scaled towards smaller dataset.`\
`# Range for calculating regional lambda is: 10000 bps`

`INFO @ Thu, 23 Jan 2014 13:37:47: #1 read tag files...`\
`INFO @ Thu, 23 Jan 2014 13:37:47: #1 read treatment tags...`\
`INFO @ Thu, 23 Jan 2014 13:37:47: Detected format is: SAM`\
`INFO @ Thu, 23 Jan 2014 13:37:56: 1000000`\
`INFO @ Thu, 23 Jan 2014 13:38:05: 2000000`\
`INFO @ Thu, 23 Jan 2014 13:38:13: 3000000`\
`INFO @ Thu, 23 Jan 2014 13:38:21: 4000000`\
`INFO @ Thu, 23 Jan 2014 13:38:29: 5000000`\
`INFO @ Thu, 23 Jan 2014 13:38:38: 6000000`\
`INFO @ Thu, 23 Jan 2014 13:38:46: 7000000`\
`WARNING @ Thu, 23 Jan 2014 13:38:56: NO records for chromosome chr16_random, minus strand!`\
`INFO @ Thu, 23 Jan 2014 13:38:56: #1 tag size is determined as 36 bps`\
`INFO @ Thu, 23 Jan 2014 13:38:56: #1 tag size = 36`\
`INFO @ Thu, 23 Jan 2014 13:38:56: #1 total tags in treatment: 5000614`\
`INFO @ Thu, 23 Jan 2014 13:38:56: #1 user defined the maximum tags...`\
`INFO @ Thu, 23 Jan 2014 13:38:56: #1 filter out redundant tags at the same location and the same strand by allowing at most 1 tag(s)`\
`INFO @ Thu, 23 Jan 2014 13:38:57: #1 tags after filtering in treatment: 3982997`\
`INFO @ Thu, 23 Jan 2014 13:38:57: #1 Redundant rate of treatment: 0.20`\
`INFO @ Thu, 23 Jan 2014 13:38:57: #1 finished!`\
`INFO @ Thu, 23 Jan 2014 13:38:57: #2 Build Peak Model...`\
`INFO @ Thu, 23 Jan 2014 13:39:15: #2 number of paired peaks: 22337`\
`INFO @ Thu, 23 Jan 2014 13:39:16: #2 finished!`\
`INFO @ Thu, 23 Jan 2014 13:39:16: #2 predicted fragment length is 164 bps`\
`INFO @ Thu, 23 Jan 2014 13:39:16: #2.2 Generate R script for model : initial_test_model.r`\
`INFO @ Thu, 23 Jan 2014 13:39:16: #3 Call peaks...`\
`INFO @ Thu, 23 Jan 2014 13:39:16: #3 shift treatment data`\
`INFO @ Thu, 23 Jan 2014 13:39:17: #3 merge +/- strand of treatment data`\
`INFO @ Thu, 23 Jan 2014 13:39:18: #3 call peak candidates`\
`INFO @ Thu, 23 Jan 2014 13:40:14: #3 use self to calculate local lambda and filter peak candidates...`\
`INFO @ Thu, 23 Jan 2014 13:40:27: #3 Finally, 32919 peaks are called!`\
`INFO @ Thu, 23 Jan 2014 13:40:27: #4 Write output xls file... initial_test_peaks.xls`\
`INFO @ Thu, 23 Jan 2014 13:40:27: #4 Write peak bed file... initial_test_peaks.bed`\
`INFO @ Thu, 23 Jan 2014 13:40:27: #4 Write summits bed file... initial_test_summits.bed`\
`INFO @ Thu, 23 Jan 2014 13:40:27: #5 Done! Check the output files!`

MACS successfully built the model and identified 32919 peaks.

For H3k27me3 data, there are two replicates, I merged them with samtools merge first

`[mtang@dev1 H3k27me3]$ samtools merge merged_H3k27me3.bam SRR038975.bam SRR038976.bam`

then at HPC@UFL:

`[mtang@dev1 H3k27me3]$ macs -t merged_H3k27me3.bam -n initial_test -g mm`

it gave me only 4941 peaks.

`[mtang@dev1 H3k27me3]$ wc -l merged_H3k27me3_peaks.bed`\
`4941 merged_H3k27me3_peaks.bed`

I changed the arguments:\
`[mtang@dev1 H3k27me3]$ macs -t merge.bam -n no_model -g mm --nomodel --shiftsize 73 --pvalue 1e-3`

note that I specify no peaking model building, shift size 73 (  fragment size 73x2=146 bp, a nuclesome size), and a less stringent pvalue 1e-3 ( the default is 1e-5)

This time I got 24522 peaks.

\[`mtang@dev1 H3k27me3]$ wc -l no_model_peaks.bed`\
`24522 no_model_peaks.bed`

In my next post, I will use those two peak bed files generated by MACS to find the bivalent promoters.

=================================================================

I will switch gear and analyze the MeDIP-seq data. MeDIP-seq data are essentially similar to ChIP-seq data. Although there are specific software to analyze it, MACS  works well with this kind of data.

see a list of programs here <http://seqanswers.com/wiki/Software/list>

I have  MeDIP_shGFP.sam and MeDIP-shDnmt3L.sam files ready. 

To visualize the MeDIP data in IGV, I have the bedgraph files generated by MACS respectively. I also want to have a bedgraph file that with MeDIP-shGFP signal subtracted from MeDIP-shDnmt3L signal.

MACS2 has a bdgcpm program for that purpose\
<https://github.com/taoliu/MACS/wiki/Build-Signal-Track#wiki-Run_MACS2_bdgcmp_to_generate_foldenrichment_and_logLR_track>

`macs2 bdgcmp -t shDnmt3L_treat_afterfiting_all.bdg -c shGFP_treat_afterfiting_all.bdg -o subtracted.bdg -m subtract`

Now, I have almost every file for visualization. Just to reproduce fig4E in the paper:\
The first track is the MeDIP-seq signal for shGFP; the second is the MeDIP-seq signal for shDnmt3L; the third is the shDnmt3L signal subtracted with shGFP signal; the fourth track is the H3k4me3 ChIP-seq signal; the fifth is the H3k4me3 peaks called by MACS; the sixth is the H3k27me3 ChIP-seq signal; the last is the H3k27me3 peaks called by MACS.

![](/img/archive/medip-seq-and-histone-modification-chip-seq-analysis/01-selection_069.png)

![](/img/archive/medip-seq-and-histone-modification-chip-seq-analysis/02-selection_070.png)

![](/img/archive/medip-seq-and-histone-modification-chip-seq-analysis/03-selection_068.png)

The next question is to find the differentially changed peaks after ablating Dnmt3L. I have a post discussing this topic before [compare ChIP-seq data for different conditions and different transcription factors?](/archive/compare-chip-seq-data-for-different-conditions-and-different-transcription/)

MACS2 has some new implements for this kind of purpose, but it requires a IgG or Input control file for each condition. <https://github.com/taoliu/MACS/wiki/Call-differential-binding-events>

I only have two bam files, so I will use <http://code.google.com/p/diffreps/>\
diffReps needs  bed files to be the input. I converted the bam files to bed files with bedtools first:\
<http://code.google.com/p/bedtools/wiki/Usage#bamToBed>

`[mtang@dev1 MeDIP-seq]$ bamToBed -i SRR764931_shGFP.bam > SRR764931_shGFP.bed`\
`[mtang@dev1 MeDIP-seq]$ bamToBed -i SRR764932_shDnmt3L.bam > SRR764932_shDnmt3L.bed`

since I do not have biological replicates, I used G-test 

`[mtang@dev1 MeDIP-seq]$ diffReps.pl -tr SRR764932_shDnmt3L.bed -co SRR764931_shGFP.bed -gn mm9 -re diff.nb.txt -me gt`

It generates three files:\
diff.nb.txt\
diff.nb.txt.annotated\
diff.nb.txt.hotspot

for more information of these three files, see the diffReps link above.
