---
title: 23  tools to work with (single-cell) TCR/BCR-seq immune repertoire data
author: Ming Tang
date: '2022-12-07T20:15:00-08:00'
lastmod: '2022-12-07T20:15:00-08:00'
slug: 23-tools-to-work-with-single-cell-tcr-bcr-seq-immune-repertoire-data
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2022/12/23-tools-to-work-with-single-cell.html
description: 3/ dandelion python package for analyzing single cell BCR/TCR data from 10x Genomics 5’ solution!
summary: 3/ dandelion python package for analyzing single cell BCR/TCR data from 10x Genomics 5’ solution!
header:
  caption: ''
  image: ''
---

1/  \[immunarch\](https://immunarch.com/index.html) 

2/ \[scRepertoire\](https://github.com/ncborcherding/scRepertoire) 

3/ \[dandelion\](https://sc-dandelion.readthedocs.io/en/latest/)  python package for analyzing single cell BCR/TCR data from 10x Genomics 5’ solution! 

4/ \[TRUST4\](https://www.nature.com/articles/s41592-021-01142-2) developed in Shirley Liu's group. Use it to extract TCR/BCR information from bulk RNAseq or 5' scRNAseq data.

5/  a dramatic speedup for one of the core computations for adaptive immune receptor repertoire (AIRR) analysis - the discovery and counting of receptors that overlap between repertoires! Check out  \[CompAIRR\](https://github.com/uio-bmi/compairr). With 10^4 repertoires of 10^5 sequences each, CompAIRR ran in 17 minutes while the fastest existing tool took 10 days, amounting to a ~1000x speedup

6/ \[ClusTCR\](https://svalkiers.github.io/clusTCR/): a Python interface for rapid clustering of large sets of CDR3 sequences with unknown antigen specificity;

7/ \[GLIPH2\](https://www.nature.com/articles/s41587-020-0505-4)

8/  \[GIANA allows computationally-efficient TCR clustering and multi-disease repertoire classification by isometric transformation\](https://www.nature.com/articles/s41467-021-25006-7) from Bo Li.

9/  \[tcrdist3\](https://github.com/kmayerb/tcrdist3) is a python API-enabled toolkit for analyzing T-cell receptor repertoires

10/ \[TCRex\](https://tcrex.biodatamining.be/): a web tool for the prediction of TCR–epitope recognition

11/  \[ImRex\](https://github.com/pmoris/ImRex) TCR-epitope recognition prediction using combined sequence input represention for convolutional neural networks.

12/  \[NetTCR - 2.0\](https://services.healthtech.dtu.dk/service.php?NetTCR-2.0) Sequence-based prediction of peptide-TCR binding

13/  \[CellaRepertorium\](https://github.com/amcdavid/CellaRepertorium)

14/  \[enclone\](https://10xgenomics.github.io/enclone/) from 10x. we should give this a try if we want to cluster TCR and BCR clonotypes.

15/  \[migec\](https://github.com/mikessh/migec):A RepSeq processing swiss-knife.

16/  \[MiXCR\](https://github.com/milaboratory/mixcr) is a universal software for fast and accurate analysis of T- and B- cell receptor repertoire sequencing data.

17/ \[ImReP\](https://sergheimangul.wordpress.com/imrep/) is a computational method for rapid and accurate profiling of the adaptive immune repertoire from regular RNA-Seq data.

18/ \[TcellMatch\](https://github.com/theislab/tcellmatch): Predicting T-cell to epitope specificity. cellMatch is a collection of models to predict antigen specificity of **single T cells** based on CDR3 sequences and other single cell modalities, such as RNA counts and surface protein counts

19/ \[scirpy\](https://github.com/icbi-lab/scirpy): A scanpy extension for single-cell TCR analysis. 

20/  \[Tessa\](https://github.com/jcao89757/tessa) is a Bayesian model to integrate T cell receptor (TCR) sequence profiling with transcriptomes of T cells. Enabled by the recently developed single cell sequencing techniques, which provide both TCR sequences and RNA sequences of each T cell concurrently, Tessa maps the functional landscape of the TCR repertoire, and generates insights into understanding human immune response to diseases. 

21/ \[DeepTCR\](https://github.com/sidhomj/DeepTCR) Deep Learning Methods for Parsing T-Cell Receptor Sequencing (TCRSeq) Data

https://twitter.com/John_Will_I_Am/status/1570837756787691527

https://www.science.org/doi/10.1126/sciadv.abq5089

22/  \[Integrating T cell receptor sequences and transcriptional profiles by clonotype neighbor graph analysis (CoNGA)\](https://www.nature.com/articles/s41587-021-00989-2)

23/ \[Echidna: Integrated simulations of single-cell immune receptor repertoires and transcriptomes\](https://academic.oup.com/bioinformaticsadvances/advance-article/doi/10.1093/bioadv/vbac062/6687122?login=false)
