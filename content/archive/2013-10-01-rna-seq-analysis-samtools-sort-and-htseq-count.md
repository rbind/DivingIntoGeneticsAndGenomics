---
title: RNA-seq analysis samtools sort and htseq-count
author: Ming Tang
date: '2013-10-01T07:43:00-07:00'
lastmod: '2013-10-01T07:43:00-07:00'
slug: rna-seq-analysis-samtools-sort-and-htseq-count
tags:
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/rna-seq-analysis-samtools-sort-and.html
description: I am following the RNA-seq nature protocol from Simon Anders. See my previous post here:Two nature protocols for RNA-seq analysis
summary: I am following the RNA-seq nature protocol from Simon Anders. See my previous post here:Two nature protocols for RNA-seq analysis
header:
  caption: ''
  image: ''
---

I am following the RNA-seq nature protocol from Simon Anders. See my previous post here:[Two nature protocols for RNA-seq analysis](/archive/two-nature-protocols-for-rna-seq-analysis/)

However, I am using some human data from this paper:\
Epigenomic plasticity enables human pancreatic α to β cell reprogramming\
<http://www.ncbi.nlm.nih.gov/pubmed/23434589>

I downloaded the SRA files, converted to fastq files ( 3 files for α cells 3 for β cells, each is around 24Gb)\
I then mapped them with tophat [The Backtick in the linux command](/archive/the-backtick-in-the-linux-command/)\
it took around 50 hrs for each file [Tophat run time](/archive/tophat-run-time/)\
using 8 threads and 3Gb memory for each thread on the HPC cluster at University of Florida.

After tophat mapping, I got six bam files. each file is around 5Gb. Following the nature protocol, I need to sort them by name with samtools sort and convert to sam file for htseq-count <http://www-huber.embl.de/users/anders/HTSeq/doc/count.html#count>.

The problem I encountered is that the bam file is too big that the merging of the sorted small bam files never occur.

`module load samtools`\
`samtools sort -n my.bam my_sn`

I googled and found <http://www.biostars.org/p/18933/>\
Taking advantage of the multi-thread property of samtools:

`samtools sort -n -@ 8 -m 3G my.bam my_sn`

it worked and was much faster.

There are many ways to get the counts for each gene.\
popular ones:

- [htseq-count](http://www-huber.embl.de/users/anders/HTSeq/doc/count.html)
- [samtools view](http://samtools.sourceforge.net/)
- [GATK countreads](http://www.broadinstitute.org/gatk/gatkdocs/org_broadinstitute_sting_gatk_walkers_qc_CountReads.html)
- [Bams2mx](https://bitbucket.org/fubar/rgalaxy/src/6670fe293778/tools/rgenetics/bams2mx.py)
- [BedTools](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&ved=0CC8QFjAA&url=http%3A%2F%2Fcode.google.com%2Fp%2Fbedtools%2F&ei=DqC6UZGbO8jYigeStYGIDA&usg=AFQjCNGXDExSMO5kCCtRzBLI-2o0-atB3g&sig2=E-B9Z_Rd_uiP85H1m4i5OA&bvm=bv.47883778,d.aGc) multicov

<http://genomespot.blogspot.com/2013/06/generate-rna-seq-count-matrix-part-2.html>\
there are some issues for the bedtools multicov when feeding the bed files converted from gtf file.\
as mentioned here <http://genomespot.blogspot.com/2013/06/generate-rna-seq-count-matrix-part-1.html?showComment=1380634400271#c1772339925972310636>\
and here <http://www.cureffi.org/2013/09/12/counts-vs-fpkms-in-rna-seq/>

" The fact that in the above example and many others throughout the genome that there are overlapping exons. If we don't collapse them, then reads assigned to those overlapping regions could be counted multiple times and provide an inaccurate profile of expression.

The solution is to merge overlapping exons right? Well yes, but there is another major caveat in that exons from different genes can sometimes have overlapping exons and we can't collapse exons from different genes, otherwise we wouldn't be able to distinguish them. The solution is to merge exons which only belong to the same gene. If there is a read which aligns to a region with more than one annotation, then it is counted twice because we can't differentiate between the two genes at that position."

I am going to stick to the nature protocol using htseq-count (Simon Anders wrote this python package HTSeq), but please keep that in mind. Different sources of gtf files are a little bit different.\
commonly we get them from UCSC table browser <http://genomewiki.ucsc.edu/index.php/Genes_in_gtf_or_gff_format>, ENSEMBL <ftp://ftp.ensembl.org/pub/release-70/gtf/homo_sapiens/> or GENCODE <http://www.gencodegenes.org/>

first several lines of the gtf file from UCSC:

`chr6 refGene exon 26924772 26924962 . + . gene_id "LINC00240"; transcript_id "NR_026775"; exon_number "1"; exon_id "NR_026775.1"; gene_name "LINC00240";`\
`chr6 refGene exon 26936237 26936301 . + . gene_id "LINC00240"; transcript_id "NR_026775"; exon_number "2"; exon_id "NR_026775.2"; gene_name "LINC00240";`

first several lines of the gtf file from ENSEMBL:

`HG104_HG975_PATCH retained_intron exon 144766622 144766712 . + . gene_id "ENSG00000261516"; transcript_id "ENST00000561901"; exon_number "1"; gene_name "ZNF707"; gene_biotype "protein_coding"; transcript_name "ZNF707-012"; exon_id "ENSE00002614945";`\
`HG104_HG975_PATCH retained_intron exon 144772227 144772293 . + . gene_id "ENSG00000261516"; transcript_id "ENST00000561901"; exon_number "2"; gene_name "ZNF707"; gene_biotype "protein_coding"; transcript_name "ZNF707-012"; exon_id "ENSE00003062908";`

first several lines of the gtf file from GENCODE:

`chr1 HAVANA gene 11869 14412 . + . gene_id "ENSG00000223972.4"; transcript_id "ENSG00000223972.4"; gene_type "pseudogene"; gene_status "KNOWN"; gene_name "DDX11L1"; transcript_type "pseudogene"; transcript_status "KNOWN"; transcript_name "DDX11L1"; level 2; havana_gene "OTTHUMG00000000961.2";`\
`chr1 HAVANA transcript 11869 14409 . + . gene_id "ENSG00000223972.4"; transcript_id "ENST00000456328.2"; gene_type "pseudogene"; gene_status "KNOWN"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_status "KNOWN"; transcript_name "DDX11L1-002"; level 2; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";`

when using htseq\
<http://www-huber.embl.de/users/anders/HTSeq/doc/count.html#count>, you need to specify the -i flag to gene_name if you prefer the gene names, otherwise, it will give you a count table with the ENSEMBL gene_id  gene_id like "ENSG00000223972.4". Remember htseq is designed to count for genes <http://seqanswers.com/forums/showthread.php?t=18068>.

`-i`` <id attribute>``,``--idattr``=<id attribute>`

GFF attribute to be used as feature ID. Several GFF lines with the same feature ID will be considered as parts of the same feature. The feature ID is used to identity the counts in the output table. The default, suitable for RNA-SEq and Ensembl GTF files, is `gene_id`.

on the HPC cluster:\
`module load htseq`\
`htseq-count -s no -a 10 -t exon -i gene_name my_sn.sam gencode.v18.annotation.gtf > my.count`

the sam file is around 25Gb, it took around 1.5 hr to finish the counting for each file.

Finally, I got the count table for all of them.\
first several lines of the count file

`0R7H2P 20`\
`5S_rRNA 54`\
`7SK 170`\
`A1BG 14`\
`A1BG-AS1 64`\
`A1CF 292`\
`A2M 252`\
`A2M-AS1 14`\
`A2ML1 352`\
`A2ML1-AS1 14`\
`A2ML1-AS2 4`\
`A2MP1 91`

Now, I am ready to use DESeq R package for the downstream analysis.  I will post later.
