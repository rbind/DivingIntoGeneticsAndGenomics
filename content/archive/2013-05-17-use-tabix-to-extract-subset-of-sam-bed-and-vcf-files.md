---
title: Use tabix to extract subset of SAM, bed and VCF files
author: Ming Tang
date: '2013-05-17T11:14:00-07:00'
lastmod: '2013-05-17T11:14:00-07:00'
slug: use-tabix-to-extract-subset-of-sam-bed-and-vcf-files
tags:
- sam
- tabix
- VCF
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/use-tabix-to-extract-subset-of-sam-bed.html
description: 'see here: \ and here : \ and here:'
summary: 'see here: \ and here : \ and here:'
header:
  caption: ''
  image: ''
---

see here: <http://davetang.org/muse/2013/02/22/using-tabix/>\
and here : <http://www.biostars.org/p/43609/>\
and here: <http://www.1000genomes.org/category/tabix>

disclaimer: credits go to the original authors, refer to the links above for detailed\
information.

-----------------------------------------------------------------------------------------------

You can use tabix to extract subsets of the vcf files from the 1000genomes websites. Thanks to the fact that tabix uses a index file, you will be able to download only portions of the files, without having to download everything in local.\
Example:

``` text
tabix -h  ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/release/20101123/interim_phase1_release/ALL.chr12.phase1.projectConsensus.genotypes.vcf.gz 1:10000,200000
```

## Using tabix

Posted on [February 22, 2013](http://davetang.org/muse/2013/02/22/using-tabix/ "5:56 am") by [Davo](http://davetang.org/muse/author/Davo/ "View all posts by Davo")

Tabix as described in [the abstract of the paper](http://bioinformatics.oxfordjournals.org/content/27/5/718.full):

> Tabix is the first generic tool that indexes position sorted files in TAB-delimited formats such as GFF, BED, PSL, SAM and SQL export, and quickly retrieves features overlapping specified regions. Tabix features include few seek function calls per query, data compression with gzip compatibility and direct FTP/HTTP access. Tabix is implemented as a free command-line tool as well as a library in C, Java, Perl and Python. It is particularly useful for manually examining local genomic features on the command line and enables genome viewers to support huge data files and remote custom tracks over networks.

When I came across tabix a couple of years ago, I looked at the paper and also the [man page](http://samtools.sourceforge.net/tabix.shtml)and while I felt it could be extremely useful, I didn't really appreciate the role it served. Now that I've seen what a colleague has achieved with it, I can see the light. Here I demonstrate with two simple examples some of the advantages of using tabix, namely speed and compression.

### Testing tabix with SAM files

For the BAM file example, let's use one from [The 1000 Genomes Project](http://davetang.org/muse/2012/08/13/the-1000-genome-project/).

|  |  |
|----|----|
| 1 | `wget``ftp``:``//ftp``.1000genomes.ebi.ac.uk``/vol1/ftp/data/NA18553/alignment/NA18553``.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.bam` |

Now convert to SAM and index:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3</p></td>
<td><p><code>samtools view -h NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.bam &gt; NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.sam</code><br />
<code>bgzip NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.sam</code><br />
<code>tabix -p sam NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.sam.gz</code></p></td>
</tr>
</tbody>
</table>

Of note is the file sizes:\
1717942076 NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.bam\
1624632294 NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.sam.gz\
bgzip compression of a SAM file is smaller than compressing to BAM; I can still use samtools with the bgzipped file though not as quick as processing a BAM file.\
Now let's try some examples with tabix and samtools (samtools can also return reads within coordinates see [this page](http://davetang.org/wiki/tiki-index.php?page=SAMTools#Extracting_SAM_entries_mapping_to_a_specific_loci)) for returning reads mapped within genomic coordinates:

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
12<br />
13<br />
14<br />
15<br />
16<br />
17</p></td>
<td><p><code>tabix NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.sam.gz 11:60000-70000</code><br />
<code>SRR032291.13719341 163 11 60046 0 67M9S = 60358 346 TAAATACACTGAAGCTGCCAAAACAATCTATCGTTTTGCCTACGTACTTATCAACTTCCTCATAGCAACATGGGAG ?DEE?@BFDIHDGGHKHIHJHHF6H?&lt;2HE?E&gt;7:=3D7</code><code>/CB</code><code>=;.D?G@=)16.&gt;@=+18+@(5@9</code><code>########## X0:i:10 X1:i:0 XC:i:67 MD:Z:67 RG:Z:SRR032291 AM:i:0 NM:i:0 SM:i:0 MQ:i:0 XT:A:R BQ:Z:@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@</code><br />
<code>[rest of results snipped]</code><br />
<code>time</code> <code>tabix NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.sam.gz 11:60000-5000000 |</code><code>wc</code><br />
 <code>550668 11647956 203417916</code></p>
<p><code>real 0m3.141s</code><br />
<code>user 0m3.249s</code><br />
<code>sys 0m1.524s</code></p>
<p><code>#testing samtools using test.bed, which contains the coordinates 11 60000 5000000</code><br />
<code>time</code> <code>samtools view -F 4 -L</code><code>test</code><code>.bed NA18553.chrom11.ILLUMINA.bwa.CHB.low_coverage.20120522.bam |</code><code>wc</code><br />
 <code>537327 11475465 200350980</code></p>
<p><code>real 0m30.318s</code><br />
<code>user 0m30.119s</code><br />
<code>sys 0m1.800s</code></p></td>
</tr>
</tbody>
</table>

Of note is the difference in the time taken to perform the calculations and the difference in the line numbers returned. **The line difference is caused by tabix not taking into account the SAM/BAM flags, namely the unmapped flag**. Though I don't show it here, I created a SAM file with one read with a SAM flag 4, which marks a read as being unmapped, but with chromosome coordinates and tabix still returned the read.

### See here <http://davetang.org/muse/2013/02/22/using-tabix/>

and here: <http://www.1000genomes.org/category/tabix>

### 

### Testing tabix with bed files

For the [FANTOM5 project](http://fantom.gsc.riken.jp/5/launch.html) we have well over thousands of [CAGE](http://en.wikipedia.org/wiki/Cap_analysis_gene_expression) libraries. My aforementioned colleague prepared a bed file containing all the CAGE tags in all human samples. Despite having more than half a billion lines in this bed file, making a genomic inquiry took only seconds.

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
6</p></td>
<td><p><code>time</code> <code>tabix hg19.ctss.bed.gz chr11:60000-5000000 |</code><code>wc</code><br />
<code>9577172 57463032 336697101</code></p>
<p><code>real 0m6.276s</code><br />
<code>user 0m7.997s</code><br />
<code>sys 0m3.092s</code></p></td>
</tr>
</tbody>
</table>

For the keen reader, have a look at the paper for [tabix](http://bioinformatics.oxfordjournals.org/content/27/5/718.full).
