---
title: ChIP-seq analysis programs
author: Ming Tang
date: '2014-01-20T12:49:00-08:00'
lastmod: '2014-01-20T12:49:00-08:00'
slug: chip-seq-analysis-programs
tags:
- ChIP-seq
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/01/chip-seq-analysis-programs.html
description: This is the first blog in the year of 2014. I am graduating in the coming August, there are so many things to do and I do not have much time to blog.…
summary: This is the first blog in the year of 2014. I am graduating in the coming August, there are so many things to do and I do not have much time to blog.…
header:
  caption: ''
  image: ''
---

This is the first blog in the year of 2014.  I am graduating in the coming August, there are so many things to do and I do not have much time to blog. However, I do not want it to be inactive.

I have extensive experience with ChIP-seq data analysis, and I want to list all the programs that I usually use.

With raw Fastq data, one needs to align it to the reference genome by an aligner. I usually use Bowtie <http://bowtie-bio.sourceforge.net/index.shtml>, others like BWA (developed by Heng Li, a legendary bioinformatician), MAQ, SOAP etc are also very popular (<http://massgenomics.org/short-read-aligners>)

Many times, I download data from SRA, sra toolkit <http://eutils.ncbi.nih.gov/Traces/sra/?view=software> is used to convert the sra format to fastq, and then one can align it with bowtie.

After mapping with Bowtie, one get a sam file <http://genome.sph.umich.edu/wiki/SAM>. samtools (also developed by Heng Li) is used to convert it to bam file, a binary form of sam file.

MACS <https://github.com/taoliu/MACS/> is the most widely used peak calling program developed by Tao Li, previously in shirley Liu's lab at Harvard. MACS2 now can detect differentially changed peaks (see the link above). It also generates a bedgraph file to be visualized by IGV <http://www.broadinstitute.org/igv/>.

For motif enrichment analysis:\
Homer  <http://homer.salk.edu/homer/ngs/index.html>\
pscan-ChIP <http://159.149.160.51/pscan_chip_dev/>\
MEME <http://meme.nbcr.net/meme/>\
GREAT for GO analysis <http://bejerano.stanford.edu/great/public/html/>\
Homer, CEAS <http://liulab.dfci.harvard.edu/CEAS/> or PAVIS <http://manticore.niehs.nih.gov:8080/pavis/> to annotate peaks

you might be interested in my previous blogs also:\
[ChIP-seq peaks overlapping significance test](/archive/chip-seq-peaks-overlapping-significance-test/)\
[compare ChIP-seq data for different conditions and different transcription factors?](/archive/compare-chip-seq-data-for-different-conditions-and-different-transcription/)\
[How to make a heatmap based on ChIP-seq data by R](/archive/how-to-make-a-heatmap-based-on-chip-seq-data-by-r/)\
[How to make TSS plot using RNA-seq and ChIP-seq data](/archive/how-to-make-tss-plot-using-rna-seq-and-chip-seq-data/)

2014 will be an exciting year for me! Follow me and many others I follow on twitter <https://twitter.com/tangming2005>.
