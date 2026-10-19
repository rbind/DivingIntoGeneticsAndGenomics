---
title: several databases I want to share for RNA-seq data
author: Ming Tang
date: '2016-07-19T09:13:00-07:00'
lastmod: '2016-07-19T09:13:00-07:00'
slug: several-databases-i-want-to-share-for-rna-seq-data
tags:
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/07/several-databases-i-want-to-share-for.html
description: Recently, there are several papers focusing on batch recomputing the public RNA-seq data sets either by local clusters or remote cloud computing. I will put…
summary: Recently, there are several papers focusing on batch recomputing the public RNA-seq data sets either by local clusters or remote cloud computing. I will put…
header:
  caption: ''
  image: ''
---

Recently, there are several papers focusing on batch recomputing the public RNA-seq data sets either by local clusters or remote cloud computing. I will put the links below:

1.  [A cloud-based workflow to quantify transcript-expression levels in public cancer compendia](http://biorxiv.org/content/early/2016/07/12/063552). one can [download](https://osf.io/gqrz9/files/) the TCGA and CCLE RNA-seq data in transcript-level  in counts and TPM units.\
used gencode v24 as reference and kallisto to calculate.

2.  [Rapid and efficient analysis of 20,000 RNA-seq samples with Toil](http://biorxiv.org/content/early/2016/07/07/062497).  Access the data [here](https://genome-cancer.soe.ucsc.edu/proj/site/xena/datapages/?cohort=TCGA%20Pan-Cancer%20(PANCAN)). Note that the data are mapped to hg38 reference genome and gencode v23 as annotation. TCGA/ICGC /GTEx and CCLE data.

3.  `intropolis` is a list of exon-exon junctions found across 21,504 human RNA-seq samples on the [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/sra) (SRA) from spliced read alignment to *hg19* with [Rail-RNA](http://rail.bio/).

4. [RESTful RNA-seq Analysis API](http://www.ebi.ac.uk/about/news/service-news/new-restful-rna-seq-analysis-api) A simple RESTful API to access analysis results of all public RNAseq data for nearly 200 species in European Nucleotide Archive.

5. [ExpressionAtlas bioconductor package](http://www.bioconductor.org/packages/release/bioc/html/ExpressionAtlas.html): This package is for searching for datasets in EMBL-EBI Expression Atlas, and downloading them into R for further analysis. Each Expression Atlas dataset is represented as a SimpleList object with one element per platform. Sequencing data is contained in a SummarizedExperiment object, while microarray data is contained in an ExpressionSet or MAList object.

6. [Bgee](http://bgee.org/)** **as suggested in the comment.

7. [The Digital Expression Explorer](http://dee.bakeridi.edu.au/index.html) The Digital Expression Explorer (DEE) is a repository of digital gene expression profiles mined from public RNA-seq data sets. These data are obtained from NCBI Short Read Archive.

- [blog post for it](http://genomespot.blogspot.com/2015/10/introducing-digital-expression-explorer.html?utm_content=bufferb6214&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)

8. [SHARQ Search public, human, RNA-seq experiments by cell, tissue type, and other features \| Indexing 19807 files](http://sharq.compbio.cs.cmu.edu/)

##
