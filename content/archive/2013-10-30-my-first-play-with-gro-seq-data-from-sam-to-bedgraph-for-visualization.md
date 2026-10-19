---
title: My first play with GRO-seq data, from sam to bedgraph for visualization
author: Ming Tang
date: '2013-10-30T18:35:00-07:00'
lastmod: '2013-10-30T18:35:00-07:00'
slug: my-first-play-with-gro-seq-data-from-sam-to-bedgraph-for-visualization
tags:
- bedgraph
- bedtools
- GRO-seq
- homer
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/my-first-play-with-gro-seq-data-from.html
description: GRO-seq (Global run on sequencing) is a technique to capture nascent RNAs in a global manner. Different from RNA-seq which sequences the matured mRNA to…
summary: GRO-seq (Global run on sequencing) is a technique to capture nascent RNAs in a global manner. Different from RNA-seq which sequences the matured mRNA to…
header:
  caption: ''
  image: ''
---

GRO-seq (Global run on sequencing) is a technique to capture nascent RNAs in a global manner.  Different from RNA-seq which sequences the matured mRNA to infer gene expression level, GRO-seq sequences the newly generated short RNAs to infer RNApol2 binding or the production of enhancer RNAs.

It was first carried out by John T.Lis at Cornell University. See their science paper in 2008:\
**Nascent RNA Sequencing Reveals Widespread Pausing and Divergent Initiation at Human Promoters**\
<http://www.ncbi.nlm.nih.gov/pubmed/19056941>

Since then, many labs use this technique to study their own subjects.\
[Michael G. Rosenfeld](http://www.nature.com/nature/journal/v498/n7455/full/nature12210.html#auth-14) from UCSD just published a nature paper in June,

## Functional roles of enhancer RNAs for oestrogen-dependent transcriptional activation

## <http://www.nature.com/nature/journal/v498/n7455/full/nature12210.html>

I want to have a look at the data set since I work with MCF7 breast cancer cells a lot.\
They put the data in GEO <http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE45822>\
although they have a bigwig file there which one can direct upload to UCSC genome browser <http://www.biostars.org/p/42844/> or local IGV to visualize, the data were mapped to hg18.

I want to have a hg19 version. So, I went to download the raw sequence sra file

wget <ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR817/SRR817000>/SRR817000.sra

then I used the sra toolkit to dump it to fastq file and mapped the data with bowtie\
in UF HPC terminal:

module load sra\
fastq-dump SRR817000.sra

the resulting fastq file is around 9Gb.\
PBS file for mapping:

`[mtang@dev1 MCF7_GRO_seq]$ cat bowtie.pbs`\
`#!/bin/bash`\
`#`\
`#PBS -N bowtie`\
`#PBS -M mtang@ufl.edu`\
`#PBS -m abe`\
`#PBS -o bowtie.test.out`\
`#PBS -e bowtie.test.err`\
`#PBS -l nodes=1:ppn=8`\
`#PBS -l pmem=3000mb`\
`#PBS -l walltime=10:30:00`\
`#`

`cd $PBS_O_WORKDIR`

`# Load the module for bowtie`\
`module load bowtie`

`# Run bowtie`\
`bowtie -p 8 --best /project/bio/bowtie/hg19 -q SRR817000.fastq -S SRR817000.sam`

Essentially, the sequences are mapped back to genomic DNA as you see.\
It took me around 30 mins to finish the mapping which is pretty fast compared with RNA-seq data. 

using Tophat, same configuration as the pbs file above, it takes me 50hrs to finish mapping. RNA-seq data are bigger (~25Gb fastq) and more complicated when mapping (figure out exon-exon junctions).

Now, I have the sam file after bowtie mapping.\
Next, I want to generate a bedGraph file for visualization.

There are several ways to do it:

1\. use Homer [http://biowhat.ucsd.edu/homer/](http://homer.ucsd.edu/homer/)\
I strongly recommend you read the tutorials to understand the various sequencing techniques. Although the formatting is not that great, it is still pretty informative.

first make Tag directory\
`makeTagDirectory MCF7_gro_seq_ctrl/ SRR817000.sam`

then make the bedgraph file for plus and minus strand respectively:

`makeUCSCfile MCF7_gro_seq_ctrl/ -strand + -o minus.bedGraph`\
`makeUCSCfile MCF7_gro_seq_ctrl/ -strand - -o plus.bedGraph`

2\. use bedtools genomecov

`bedtools genomecov -ibam SRR817000.sorted.bam -bg -strand + > bedtools_plus.bedGraph`\
`bedtools genomecov -ibam SRR817000.sorted.bam -bg -strand - > bedtools_minus.bedGraph`

the bam file needs to be sorted first with samtools

it takes much longer for bedtools compared with Homer (Homer generates the tagDirectory for later quick access). gzip the bedGraph file to .gz

all the files I have:

`[mtang@dev1 MCF7_GRO_seq]$ ls -sh .`\
`total 19G`\
` 87M bedtools_plus.bedGraph.gz 4.0K fetchChromSizes 16M plus.bedGraph.gz 1.5G SRR817000.sorted.bam`\
`4.0K bowtie.pbs 4.0K hg19.chrom.sizes 1.9G SRR817000.bam 1.2G SRR817000.sra`\
`3.1M bowtie.test.err 4.0K MCF7_gro_seq_ctrl 8.6G SRR817000.fastq`\
` 0 bowtie.test.out 14M minus.bedGraph.gz 5.6G SRR817000.sam`

3\. Use HTSeq python package: <http://www-huber.embl.de/users/anders/HTSeq/doc/tour.html#tour>

{{< gist crazyhottommy 7254815 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/7254815)

I then imported the bedgraph files to IGV and had a look at my favourite gene VEGFA.\
I published my first paper on it:) 

## Restraint of angiogenesis by zinc finger transcription factor CTCF-dependent chromatin insulation

 <http://www.ncbi.nlm.nih.gov/pubmed/21896759>

![](/img/archive/my-first-play-with-gro-seq-data-from-sam-to-bedgraph-for-visualization/01-selection_028.png)

The first track is the minus-strand bedgraph got from Homer\
the second track is the plus-strand bedgraph got from Homer\
the third track is the plus-strand bedgraph got from bedtools genomecov

I see clearly short RNAs spiking  at the promoter of VEGFa, suggesting that RNApol2 paused at promoter and does not undergo productive elongation. This gene is actively transcribed with full-length mRNA, but the promoter is still paused. ( read the 2008 science GRO-seq paper for more details of different groups of genes)

one thing I do noticed is that the bedgraph file from bedtools is bigger than the one from Homer 87M vs 16M, and if I zoom in I saw some small differences:\
![](/img/archive/my-first-play-with-gro-seq-data-from-sam-to-bedgraph-for-visualization/02-selection_029.png)

It looks like the bedgraph from bedtools exhibits a more detailed view of the data, but overall either bedgraph file would be fine for a quick inspection of interested genes.( I guess Homer uses some bigger bin for calculating or merge bins with small difference together to save space).

Finally, if you want to make a bigwig file for UCSC genome browser, you need to use bedGraphToBigWig  from here <http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/>\
and follow <http://www.biostars.org/p/42844/>.

further readings: <http://www.biostars.org/p/64495/#64680>\
<http://davetang.org/muse/2012/03/15/visualising-rna-seq-like-data/>\
<http://davetang.org/muse/2012/03/15/ucsc-genome-browser-custom-overlap-tracks/>
