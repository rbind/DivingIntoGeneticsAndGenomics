---
title: count how many mapped reads in a bam file
author: Ming Tang
date: '2013-06-18T19:09:00-07:00'
lastmod: '2013-06-18T19:09:00-07:00'
slug: count-how-many-mapped-reads-in-a-bam-file
tags:
- bam
- NGS
- samtools
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/count-how-many-mapped-reads-in-bam-file.html
description: I wanted to count how many mapped reads are in a ChIP-seq bam file downloaded from UCSC ECODE project.
summary: I wanted to count how many mapped reads are in a ChIP-seq bam file downloaded from UCSC ECODE project.
header:
  caption: ''
  image: ''
---

I wanted to count how many mapped reads are in a ChIP-seq bam file downloaded from UCSC ECODE project. <http://genome.ucsc.edu/ENCODE/downloads.html>

A quick google search:\
<http://left.subtree.org/>\
and [http://seqanswers.com/forums/showthread.php?t=891](http://seqanswers.com/forums/showthread.php?t=8911)

The output from short read aligners like [Bowtie](http://bowtie-bio.sourceforge.net/index.shtml) and [BWA](http://bio-bwa.sourceforge.net/) is commonly stored in [SAM/BAM](http://samtools.sourceforge.net/) format. When presented with one of these files a common first task is to calculate the total number of alignments (reads) captured in the file. In this post I show some examples for finding the total number of reads using samtools and directly from Java code. For the examples below, I use the HG00173.chrom11 BAM file from the 1000 genomes project which can be downloaded [here](ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/data/HG00173/alignment/).\
First, we look at using the samtools command directly. One way to get the total number of alignments is to simply dump the entire SAM file and tell samtools to count instead of print (`-c` option):

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2</p></td>
<td><p><code>$ samtools view -c HG00173.chrom11.ILLUMINA.bwa.FIN.low_coverage.20111114.bam</code><br />
<code>5218322</code></p></td>
</tr>
</tbody>
</table>

If we’re only interested in counting the total number of mapped reads we can add the `-F 4` flag. Alternativley, we can count only the unmapped reads with `-f 4`:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7</p></td>
<td><p><code># Mapped reads only</code><br />
<code>$ samtools view -c -F 4 HG00173.chrom11.ILLUMINA.bwa.FIN.low_coverage.20111114.bam</code><br />
<code>5068340</code></p>
<p><code># Unmapped reads only</code><br />
<code>$ samtools view -c -f 4 HG00173.chrom11.ILLUMINA.bwa.FIN.low_coverage.20111114.bam</code><br />
<code>149982</code></p></td>
</tr>
</tbody>
</table>

To understand how this works we first need to inspect the SAM format. The SAM format includes a bitwise FLAG field described [here](http://samtools.sourceforge.net/samtools.shtml#5). The `-f/-F` options to the samtools command allow us to query based on the presense/absence of bits in the FLAG field. So `-f 4` only output alignments that are unmapped (flag 0×0004 is set) and `-F 4` only output alignments that are *not* unmapped (i.e. flag 0×0004 is not set), hence these would only include mapped alignments.\
An example for paired end reads you could do the following. To count the number of reads having both itself and it’s mate mapped:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2</p></td>
<td><p><code>$ samtools view -c -f 1 -F 12 HG00173.chrom11.ILLUMINA.bwa.FIN.low_coverage.20111114.bam</code><br />
<code>4906035</code></p></td>
</tr>
</tbody>
</table>

The `-f 1` switch only includes reads that are paired in sequencing and `-F 12` only includes reads that are *not*unmapped (flag 0×0004 is not set) and where the mate is *not* unmapped (flag 0×0008 is not set). Here we add `0x0004 + 0x0008 = 12` and use the `-F` (bits not set), meaning you want to include all reads where neither flag 0×0004 or 0×0008 is set. For help understanding the values for the SAM FLAG field there’s a handy web tool [here](http://picard.sourceforge.net/explain-flags.html).\
There’s also a nice command included in samtools called `flagstat` which computes various summary statistics. However, I wasn’t able to find much documentation describing the output and it’s not mentioned anywhere in the man page. This [post](http://biostar.stackexchange.com/questions/12502/what-does-samtools-flagstat-results-mean) examines the C code for the flagstat command which provides some insight into the output.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12</p></td>
<td><p><code>$ samtools flagstat HG00173.chrom11.ILLUMINA.bwa.FIN.low_coverage.20111114.bam</code><br />
<code>5218322 + 0 in total (QC-passed reads + QC-failed reads)</code><br />
<code>273531 + 0 duplicates</code><br />
<code>5068340 + 0 mapped (97.13%:-nan%)</code><br />
<code>5205999 + 0 paired in sequencing</code><br />
<code>2603248 + 0 read1</code><br />
<code>2602751 + 0 read2</code><br />
<code>4881994 + 0 properly paired (93.78%:-nan%)</code><br />
<code>4906035 + 0 with itself and mate mapped</code><br />
<code>149982 + 0 singletons (2.88%:-nan%)</code><br />
<code>19869 + 0 with mate mapped to a different chr</code><br />
<code>15271 + 0 with mate mapped to a different chr (mapQ&gt;=5)</code></p></td>
</tr>
</tbody>
</table>

So, samtools flagstat  is what I need.

`tommy@tommy-ThinkPad-T420:~/Desktop$ time samtools flagstat Mcf7Ctcf.sorted.bam`\
`20769652 + 0 in total (QC-passed reads + QC-failed reads)`\
`0 + 0 duplicates`\
`20769652 + 0 mapped (100.00%:-nan%)`\
`0 + 0 paired in sequencing`\
`0 + 0 read1`\
`0 + 0 read2`\
`0 + 0 properly paired (-nan%:-nan%)`\
`0 + 0 with itself and mate mapped`\
`0 + 0 singletons (-nan%:-nan%)`\
`0 + 0 with mate mapped to a different chr`\
`0 + 0 with mate mapped to a different chr (mapQ>=5)`

`real 0m12.472s`\
`user 0m11.237s`\
`sys 0m0.384s`

it is pretty fast.  with 12 seconds it finished counting a bam file with 20 million reads.

I used to use HTSeq python package <http://www-huber.embl.de/users/anders/HTSeq/doc/overview.html>\
 to count the mapped reads, but it is much slower (several mins to count the same file) than samtools.

`def aligned_counts(ifile1):`\
` '''count how many alignments are aligned back to genome, ifile1 is a sorted bam file'''`\
` import HTSeq`\
` sortedbamfile= HTSeq.BAM_Reader(ifile1)`\
` aligned_counts=0`\
` unaligned_counts=0`\
` for almnt in sortedbamfile:`\
` if almnt.aligned:`\
` aligned_counts+= 1`\
` else:`\
` unaligned_counts+=1`\
` print "number of aligned tags of %s is %d " % (ifile1, aligned_counts)`\
` print "number of unaligned tags of %s is %d "% (ifile1, unaligned_counts)`\
` return aligned_counts`

Again, I am happy with the existing tools. Do not re-invent the wheels !
