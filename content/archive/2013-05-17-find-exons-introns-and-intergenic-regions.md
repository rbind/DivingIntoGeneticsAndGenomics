---
title: Find exons introns and intergenic regions
author: Ming Tang
date: '2013-05-17T11:13:00-07:00'
lastmod: '2013-05-17T11:13:00-07:00'
slug: find-exons-introns-and-intergenic-regions
tags:
- Genomic Interval
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/find-exons-introns-and-intergenic.html
description: Posted on January 18, 2013 by Davo
summary: Posted on January 18, 2013 by Davo
header:
  caption: ''
  image: ''
---

## see post here:

<http://davetang.org/muse/2013/01/18/exons-introns-and-intergenic-regions/>

## 

## Exons, introns and intergenic regions

Posted on [January 18, 2013](http://davetang.org/muse/2013/01/18/exons-introns-and-intergenic-regions/ "8:39 am") by [Davo](http://davetang.org/muse/author/Davo/ "View all posts by Davo")

I'm interested in defining the exons, introns and intergenic regions of the genome based on GENCODE. So I thought about a fuss free way to do this, and BEDTools would be perfect.\
Download the latest GENCODE annotations (version 14):

|  |  |
|----|----|
| 1 | `wget``ftp``:``//ftp``.sanger.ac.uk``/pub/gencode/release_14/gencode``.v14.annotation.gtf.gz` |

I will use mergeBed, subtractBed and complementBed. See [here](http://code.google.com/p/bedtools/wiki/Usage) for usage examples.\
The GTF file defines exons, so I will find all exons, output as a minimal bed file, sort the bed file and merge the overlapping exons:

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
4</p></td>
<td><p><code>zcat gencode.v14.annotation.gtf.gz |</code><code>awk</code> <code>'BEGIN{OFS="\t";} $3=="exon" {print $1,$4,$5}'</code> <code>&gt; gencode_v14_exon.bed</code><br />
<code>sortBed -i gencode_v14_exon.bed &gt; gencode_v14_exon_temp.bed</code><br />
<code>mv</code> <code>-f gencode_v14_exon_temp.bed gencode_v14_exon.bed</code><br />
<code>mergeBed -i gencode_v14_exon.bed &gt; gencode_v14_exon_merged.bed</code></p></td>
</tr>
</tbody>
</table>

To define intronic regions, we need to define the gene i.e. obtain the gene coordinates, and subtract the exons from this.

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
4</p></td>
<td><p><code>zcat gencode.v14.annotation.gtf.gz |</code><code>awk</code> <code>'BEGIN{OFS="\t";} $3=="gene" {print $1,$4,$5}'</code> <code>&gt; gencode_v14_gene.bed</code><br />
<code>sortBed -i gencode_v14_gene.bed &gt; gencode_v14_gene_temp.bed</code><br />
<code>mv</code> <code>-f gencode_v14_gene_temp.bed gencode_v14_gene.bed</code><br />
<code>subtractBed -a gencode_v14_gene.bed -b gencode_v14_exon_merged.bed &gt; gencode_v14_intron.bed</code></p></td>
</tr>
</tbody>
</table>

And finally to define intergenic regions, we use complementBed to find regions not covered by genes. To create a hg19_chrom_info.txt file, use the fetchChromSizes executable available at <http://hgdownload.cse.ucsc.edu/admin/exe/> to create the hg19_chrom_info.txt file for hg19.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2</p></td>
<td><p><code>.</code><code>/fetchChromSizes</code> <code>hg19 &gt; hg19_chrom_info.txt</code><br />
<code>complementBed -i gencode_v14_gene.bed -g hg19_chrom_info.txt &gt; gencode_v14_intergenic.bed</code></p></td>
</tr>
</tbody>
</table>

And that's it.\
[![bedtools](http://i0.wp.com/davetang.org/muse/wp-content/uploads/2013/01/bedtools.png?resize=640%2C670)](http://davetang.org/muse/2013/01/18/exons-introns-and-intergenic-regions/bedtools/)\
Figure 1. The genome is a black box.
