---
title: RNA-seq analysis tools
author: Ming Tang
date: '2013-05-13T20:09:00-07:00'
lastmod: '2013-05-13T20:09:00-07:00'
slug: rna-seq-analysis-tools
tags:
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/rna-seq-analysis-tools.html
description: Which tool is popular to perform RNA-seq analysis, including read mapping, gene/transcript expression level estimation and differential analysis? Here is…
summary: Which tool is popular to perform RNA-seq analysis, including read mapping, gene/transcript expression level estimation and differential analysis? Here is…
header:
  caption: ''
  image: ''
---

### 

### see here <http://allaboutbioinfo.blogspot.com/>

### [Which tool is used for RNA-seq analysis?](http://allaboutbioinfo.blogspot.com/2013/02/which-tool-is-used-for-rna-seq-analysis.html)

Which tool is popular to perform RNA-seq analysis, including read mapping, gene/transcript expression level estimation and differential analysis? [H](http://www.biostars.org/p/58299/)[ere](http://www.biostars.org/p/58299/) is the survey of which tool to used for RNA-seq analysis.

In summary, the majority will:

- aligns the reads to the genome (or genome+transcriptome) using Tophat;
- count reads using Cufflinks (the second choice is HTSeq-count, which is becoming popular);
- perform differential expression using DESeq/DEXSeq (followed by CuffDiff);
- use Ensemble (followed by Refseq/UCSC) as the annotation resource;
- use GOSeq (followed by IPA and Genego Metacore) for downstream analysis.

-----------------------------------------------------------------------------------------------------------------------------
update 07/16/13\
several packages have been developed to simplify(automate) the process:\
blacktie <http://xguse.github.io/blacktie/index.html>\
Nesoni <http://www.vicbioinformatics.com/software.nesoni.shtml>

I will give Nesoni a try in the near future, it use limma to detect the differentially expressed genes which I like very much, but I have to let the HPC center install it first. RNA-seq\
data are just too big, I can not run the analysis on my own computer, especially when I have multiple replicates.
