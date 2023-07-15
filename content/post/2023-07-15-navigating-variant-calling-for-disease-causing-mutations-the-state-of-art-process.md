---
title: 'Navigating Variant Calling for Disease-Causing Mutations: The state-of-art
  process'
author: Ming Tang
date: '2023-07-15'
slug: navigating-variant-calling-for-disease-causing-mutations-the-state-of-art-process
categories:
  - bioinformatics
  - cloud computing
tags:
  - bioinformatics
  - GATK
header:
  caption: ''
  image: ''
---

**Disclaimer: This post is sponsored by [Watershed Omics Bench platform](https://www.watershed.ai/). I have personally tested the platform.**

Variant calling is the process of identifying and categorizing genetic variants in sequencing data. It is a critical step in the analysis of whole-genome sequencing (WGS) and whole-exome sequencing (WES) data, as it allows researchers to identify potential disease-causing mutations.

#### Choice of aligners

The first step in variant calling is to align the sequencing reads to a reference genome. This is done using a program such as [BWA-MEM](https://github.com/lh3/bwa) or [minimap2](https://github.com/lh3/minimap2). Once the reads are aligned, the variant calling algorithm can then identify any differences between the sequencing data and the reference genome.
You will need to choose different aligners depending on the read length of your sequencing experiment.

1.For short reads (70- 100 bp), `BWA-MEM` is particularly effective.

2.For long reads (> 1000 bp, e.g, PacBio or Oxford Nanopore genomic reads), `minimap2` has been shown to be powerful and accurate. It has even been shown to have advantages for short reads > 100 bp, with some reports citing three times the speed of `BWA-MEM` and `Bowtie2` with similar accuracy on simulated data.

#### Choice of variant callers

Variant calling algorithms typically use a number of different methods to identify variants, including:

* Indel calling: Identifying single-nucleotide insertions and deletions in sequencing data.
* SNV calling: Identifying single-nucleotide variants (SNVs), aka, somatic mutations in disease samples or single-nucleotide polymorphisms (SNPs) in germline samples.
* Structural variant calling: Identifying larger-scale alterations in the genome, such as large deletions, duplications, inversions, translocations, and other rearrangements, which can have significant implications for gene function and disease susceptibility.

Variant callers also operate differently based on the experimental design:

* Germline variant calling: typically assumes diploid genome, imposing allele frequencies of 0, 0.5 or 1
* Somatic variant calling: usually uses two samples from the same host, one baseline (normal) and the other with an insult or disease (tumor). This uses the baseline sample to help inform somatic variant calls. Notably, most somatic variant callers do not assume diploidy due to clonality, heterogeneity, or duplications/deletions
* Trio variant calling: particularly useful when phasing variant calls are important or determining heredity
* Joint variant calling: Uses multiple samples from the same population to help inform low-confidence calls

There are a number of different variant calling algorithms available, each with its own strengths and weaknesses. Some of the most popular algorithms include [GATK,](https://gatk.broadinstitute.org/hc/en-us) samtools, bcftools, and [VarScan](https://varscan.sourceforge.net/), though the more recently developed [DRAGEN](https://gatk.broadinstitute.org/hc/en-us/articles/4411716682011-Full-release-of-open-source-DRAGEN-GATK-1-0), Deepvariant, and Sention have been shown to have better performance, and are rapidly growing in popularity.

Benchmarking variant calling is a challenge as the ground truth samples are not readily available. [Genome in a Bottle](https://www.nist.gov/programs-projects/genome-bottle) provides a frequently utilized standard set of files for benchmarking data resources. In this [benchmark](https://www.cell.com/cell-genomics/pdfExtended/S2666-979X(22)00058-1) experiment, [Sention](https://www.sentieon.com/products/), which uses optimizes GATK internally for variant calling, is a top performer. However, real cancer samples are heterogeneous and complex. Low-frequency allele variants are hard to detect when they first arise, but they have been shown repeatedly to undergo clonal expansion in later stages of cancer or when undergoing significant environmental pressure. 

#### Choice of variant annotators

Once variants have been called, they need to be filtered and annotated. Filtering is the process of removing variants that are likely to be false positives. Annotation is the process of assigning meaning to variants, such as whether they are known to be pathogenic or not. To better understand the biological significance of the variants, annotation is needed.

VEP, ANNOVAR, and [SnpEff](https://pcingola.github.io/SnpEff/) are commonly used variant annotation tools. The [OpenCRAVAT](https://opencravat.org/) was developed in Rachel Karchin’s group at Johns Hopkins. Compared with other similar tools, its unique feature lies in its ability to access and integrate an unparalleled range of diverse data resources and computational prediction methods, encompassing germline, somatic, common, rare, coding, and non-coding variants.

>
It was designed to have better annotations for somatic variants in cancer than standard variant annotators. It runs on the command line, but also produces interactive variant dashboards/reports that are shareable.

– Author of OpenCRAVAT, Collin Tokheim.

OpenCRAVAT includes databases such as `GETEx`, `ClinVar`, `COSMIC`, `gnomAD`, `CIVIC`, `1000 genome`, and many others.

Variant calling is a complex and challenging task, but it is essential for the analysis of WGS and WES data. By identifying genetic variants, researchers can gain valuable insights into the causes of disease and develop new treatments. 

**[Watershed](https://www.watershed.ai/) has implemented many variant calling tools and pipelines, including GATK best practices for germline and somatic variant calling that are ready to plug in for your data analysis. Their platform can help you rapidly obtain confident variant calls.**

At Watershed, customers' usage cases include:

* Cell therapy companies verifying no variants have been introduced in the process of producing their products
* Gene editing companies verifying the accuracy and specificity of their approach
* Personalized medicine groups screening cancer samples for druggable variants, novel and existing
* Mutations in the cell line model for a number of companies that need to regularly monitor the mutation profiles of “workhorse” lines used for drug development to ensure comparability of results across time
* Cell-free DNA(cf-DNA) mutation detection

The Watershed Bioinformatics team has extensive experience in WGS/WES data analysis. They can assist you with the downstream interpretation. Watershed has been a trusted partner with many biotech companies, including `SalioGen`, `CargoTx`, and `Benchling`.
