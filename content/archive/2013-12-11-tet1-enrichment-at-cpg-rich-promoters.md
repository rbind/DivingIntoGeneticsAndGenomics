---
title: Tet1 enrichment at CpG rich-promoters
author: Ming Tang
date: '2013-12-11T20:51:00-08:00'
lastmod: '2013-12-11T20:51:00-08:00'
slug: tet1-enrichment-at-cpg-rich-promoters
tags:
- ChIP-seq
- CpG
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/12/tet1-enrichment-at-cpg-rich-promoters.html
description: Continue with my last post GEOquery to access GEO datasets, I am going to analyze the Tet1 ChIP-seq data in the paper Dual functions of Tet1 in…
summary: Continue with my last post GEOquery to access GEO datasets, I am going to analyze the Tet1 ChIP-seq data in the paper Dual functions of Tet1 in…
header:
  caption: ''
  image: ''
---

Continue with my last post [GEOquery to access GEO datasets](/archive/geoquery-to-access-geo-datasets/), I am going to analyze the Tet1 ChIP-seq data in the paper [Dual functions of Tet1 in transcriptional regulation in mouse embryonic stem cells](http://www.nature.com/nature/journal/v473/n7347/full/nature09934.html).

To reproduce fig1, I have to group the promoters into CpG-rich and CpG-poor promoters first.

Get the CpG island bed file and the refseq gene table first.

I got the file from UCSC table browser <http://genome.ucsc.edu/cgi-bin/hgTables?command=start>\
choose Expression and regulation and CpG islands track, and download it.

![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/01-selection_056.png)

![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/02-selection_057.png)

similarly, get the refSeq gene table, send the output to Galaxy ( in order to get the -5kb promoter\
regions, one needs to do some manipulation of the file, several options are there: bedtools, awk and Galaxy. The easiest way is through Galaxy if you are not familiar with linux command line)

![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/03-selection_052.png)

![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/04-selection_053.png)

Click get franks, and get the 5kb upstream coordinates, and cut columns that we need:\
`tommy@tommy-ThinkPad-T420:~/Tet1$ head 5kb_promoter_mm9.bed`\
`chr1 134207701 134212701 NM_028778 +`\
`chr1 134207701 134212701 NM_001195025 +`\
`chr1 58752833 58757833 NM_175370 -`\
`chr1 9289811 9294811 NM_027671 -`\
`chr1 25886552 25891552 NM_175642 -`\
`chr1 33726639 33731639 NM_008922 -`\
`chr1 125942136 125947136 NM_199021 -`\
`chr1 75503027 75508027 NM_178884 -`\
`chr1 109057691 109062691 NM_198680 -`\
`chr1 184490137 184495137 NM_130890 +`\
you can do the same thing with CpG islands with Galaxy, but I show you the linux command below:\
`tommy@tommy-ThinkPad-T420:~/Tet1$ head mm9_CpG_island.txt`\
`#bin chrom chromStart chromEnd name length cpgNum gcNum perCpg perGc obsExp`\
`107 chr1 36568608 36569851 CpG: 128 1243 128 766 20.6 61.6 1.09`\
`151 chr1 82313020 82313491 CpG: 56 471 56 319 23.8 67.7 1.06`\
`234 chr1 169213895 169215283 CpG: 145 1388 145 958 20.9 69 0.88`\
`611 chr1 3521705 3521924 CpG: 27 219 27 167 24.7 76.3 0.86`\
`612 chr1 3660700 3661155 CpG: 34 455 34 293 14.9 64.4 0.72`\
`612 chr1 3661735 3662237 CpG: 45 502 45 348 17.9 69.3 0.75`\
`619 chr1 4481782 4483754 CpG: 165 1972 165 1286 16.7 65.2 0.79`\
`619 chr1 4487028 4487689 CpG: 47 661 47 393 14.2 59.5 0.81`\
`619 chr1 4561722 4562156 CpG: 44 434 44 265 20.3 61.1 1.09`\
I only need the second, third and fourth  column of the CpG file:\
`tommy@tommy-ThinkPad-T420:~/Tet1$ cat mm9_CpG_island.txt | cut -f2-4 > mm9_CpG.bed`

`tommy@tommy-ThinkPad-T420:~/Tet1$ head mm9_CpG.bed`\
`chr1 36568608 36569851`\
`chr1 82313020 82313491`\
`chr1 169213895 169215283`\
`chr1 3521705 3521924`\
`chr1 3660700 3661155`\
`chr1 3661735 3662237`\
`chr1 4481782 4483754`\
`chr1 4487028 4487689`\
`chr1 4561722 4562156`\
`chr1 4679265 4679478`

How many CpG island and refGenes are there?

`tommy@tommy-ThinkPad-T420:~/Tet1$ wc -l 5kb_promoter_mm9.bed mm9_CpG.bed`\
` 32555 5kb_promoter_mm9.bed`\
` 16026 mm9_CpG.bed`\
` 48581 total`

Now use bedtools (you can also use the intersect, substract tool in Galaxy) to get promoters that contain CpG island:

`tommy@tommy-ThinkPad-T420:~/Tet1$ bedtools intersect -a 5kb_promoter_mm9.bed -b mm9_CpG.bed -wa -u | wc -l`\
`16171`

`tommy@tommy-ThinkPad-T420:~/Tet1$ bedtools intersect -a 5kb_promoter_mm9.bed -b mm9_CpG.bed -wa -u > CpG_rich_promoters.bed`

promoters without CpG island:

`tommy@tommy-ThinkPad-T420:~/Tet1$ bedtools intersect -a 5kb_promoter_mm9.bed -b mm9_CpG.bed -v | wc -l`\
`16384`

`tommy@tommy-ThinkPad-T420:~/Tet1$ bedtools intersect -a 5kb_promoter_mm9.bed -b mm9_CpG.bed -v > CpG_poor_promoters.bed`

I have mapped the Tet1 ChIP-seq raw sequence data by bowtie at HPC@UFL. I got the sam file and converted it to sorted bam file with samtools. Then, I called peaks with MACS1.4, and got the peaks and bedgraph file that I can visualize in IGV genome browser (see my previous post [ChIP-exo data analysis](/archive/chip-exo-data-analysis/)).

A partial reproduction of fig1b in the paper:\
![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/05-selection_049.png)

the first track is the IgG control, the second is the Tet1 bedgraph signal, the third is the Tet1 peaks called by MACS, and the last one is the mm9_CpG.bed track. I observed many CpG islands are overlapped with the Tet1 peaks. The peaks are very sharp indicating a successful ChIP-seq experiment.

There are total 30946 peaks that are called by MACS. The number is 35,564 in the paper, but they used an older version of MACS. Nevertheless, the numbers are very close.

`tommy@tommy-ThinkPad-T420:~/Tet1$ wc -l Tet1_ChIP-seq_peaks.bed`\
`30946 Tet1_ChIP-seq_peaks.bed`

using CEAS <http://liulab.dfci.harvard.edu/CEAS/usermanual.html> from Shirely Liu's lab, I investigated the distribution of the peaks:\
`tommy@tommy-ThinkPad-T420:~/Desktop/Gene_Annotation_for_CEAS_package$ ceas -g mm9.refGene -b ~/Tet1/Tet1_ChIP-seq_peaks.bed`\
![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/06-selection_058.png)\
![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/07-selection_059.png)\
Most peaks are located at gene-rich regions, only 23.3% of them are located at distal intergenic regions. It will be very interesting to study the function of these distal Tet1 binding sites.\
A significant portion of peaks (33.2%) are at introns, indicating that Tet1 plays a role in DNA methylation in the gene bodies.\
Note that CEAS can also produce figures as in fig1a, but it requires a wig file as input. I will stick to my python script which only need a bam file ( to convert bam/sam files to wig file, see here\
<http://www.biostars.org/p/2699/> and here <http://www.ecseq.com/NGSsnippets.html>)

To reproduce fig1a, I have all the files ready: a sorted Tet1 ChIP-seq bam file, CpG_rich_promoters.bed and CpG_poor_promoters.bed

See the python code below, it uses HTSeq python package written by Simon Anders. <http://www.embl.de/research/units/genome_biology/huber/members/?s_personId=CP-60002977>. He is an expert in RNA-Seq analysis ( He wrote the famous DESeq package)

{{< gist crazyhottommy 8357168 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/8357168)

it generates a figure as shown below:

![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/08-tet1.png)

One can count the mapped the reads number by samtools flagstat first, and then plug in the number in the above script. It is pretty slow to count the mapped reads by the aligned_counts function above.\
I tricked by counting the reads first by samtools and the whole script only took me 3 minutes to run which is pretty fast!\
Different from fig1a in the original paper, I used normalized counts per million for Y axis. I do not know why the authors used the peak P value as Y axis. Anyway, the conclusion is still the same: Tet1 preferentially binds to CpG-rich promoters. A close inspection of the figure generated by ipython ( interactive visualization) reveals that Tet1 peaks at around 100bp downstream of TSS.

I also reproduced part of Fig3a where the authors show Tet1 binds to bivalent promoters:

![](/img/archive/tet1-enrichment-at-cpg-rich-promoters/09-bivalent.png)

Annotate the peaks by Homer:\
`tommy@tommy-ThinkPad-T420:~/Tet1$ annotatePeaks.pl Tet1_ChIP-seq_peaks.bed mm9 -noann > Tet1_targeted_genes.txt`

Homer finds the closest TSS for each peak. If I set the closest distance limit to 10kb upstream and downstream of TSS, there are still around ~12,000 genes that are associated with these peaks (total 30946 peaks).

`the 10th column is the distance to TSS, the 16th column is the gene name`\
`tommy@tommy-ThinkPad-T420:~/Tet1$ cat Tet1_targeted_genes.txt | awk '{ if ($10<10000 && $10>-10000) print}'| cut -f16 | sort | uniq | wc -l`\
`12202`

Summary:\
1. It is a very successful ChIP-seq experiment with sharp peaks over IgG background. Majority of Tet1 binding sites are at gene-rich regions. 23.3% of Tet1 binding sites are in distal intergenic regions with unknow functions. Tet1 tends to bind to CpG-rich promoters.

2. If one assigns the nearest genes to the peaks, ~12,000 genes are associated with Tet1 binding sites. compare with the differentially expressed genes in my last post-only 1292 genes (~10%) are affected by Tet1 KD- the number of genes that are regulated by Tet1 is much smaller than the peak associated ones.

Several reasons: KD is not complete (~20% residue Tet1 remains according to the microarray data), the remaining Tet1 can still maintain expression of some genes. If we knock out Tet1 (by TALENT or CRISPR <http://zlab.mit.edu/publications.html>), it may have a bigger effect on gene expression. In addition, RNA-seq may reveal more differentially expressed genes compared with microarray.

Another reason is that binding does not infer function (especially for weak binding sites) and I think this is probably the mostly likely reason. Several papers have shown that changes of adjacent TF binding poorly correlates with gene expression change:

##

## Extensive Divergence of Transcription Factor Binding in *Drosophila* Embryos with Highly Conserved Gene Expression 

## <http://www.plosgenetics.org/article/info%3Adoi%2F10.1371%2Fjournal.pgen.1003748>

## Transcription Factors Bind Thousands of Active and Inactive Regions in the*Drosophila* Blastoderm

<http://www.plosbiology.org/article/info%3Adoi%2F10.1371%2Fjournal.pbio.0060027>

## The Functional Consequences of Variation in Transcription Factor Binding

## <http://arxiv.org/abs/1310.5166>

" On average, 14.7% of genes bound by a factor were differentially expressed following the knockdown of that factor, suggesting that most interactions between TF and chromatin do not result in measurable changes in gene expression levels of putative target genes. "

To assign functional binding sites to their target genes, one may consider to use BETA developed in Shirely Liu's lab <http://cistrome.org/BETA/>. It integrates ChIP-seq data and gene expression data to infer  the TF target genes.\
You may be also interested in this paper:

## Assessing Computational Methods for Transcription Factor Target Gene Identification Based on ChIP-seq Data 

## <http://www.ploscompbiol.org/article/info:doi/10.1371/journal.pcbi.1003342#pcbi.1003342.s019>

That's all for today! I will continue to analyze the DNA methylation data.
