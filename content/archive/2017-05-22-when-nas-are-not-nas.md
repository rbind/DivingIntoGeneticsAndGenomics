---
title: when NAs are not NAs
author: Ming Tang
date: '2017-05-22T13:43:00-07:00'
lastmod: '2017-05-22T13:43:00-07:00'
slug: when-nas-are-not-nas
tags:
- NAs
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/05/when-nas-are-not-nas.html
description: Using TCGAbiolinks, I downloaded RNAseq data for LUAD and LUSC
summary: Using TCGAbiolinks, I downloaded RNAseq data for LUAD and LUSC
header:
  caption: ''
  image: ''
---

## Downloading data

Using TCGAbiolinks, I downloaded RNAseq data for `LUAD` and `LUSC`

``` r
library(TCGAbiolinks)
library(SummarizedExperiment)

# query_rna_LUAD.hg38 <- GDCquery(project = "TCGA-LUAD", data.category = "Transcriptome Profiling",
#                   data.type = "Gene Expression Quantification",
#                   workflow.type = "HTSeq - Counts")
#
#
# query_rna_LUSC.hg38 <- GDCquery(project = "TCGA-LUSC", data.category = "Transcriptome Profiling",
#                   data.type = "Gene Expression Quantification",
#                   workflow.type = "HTSeq - Counts")
#
# GDCdownload(query_rna_LUAD.hg38, method = "client")
# GDCdownload(query_rna_LUSC.hg38, method = "client")
#
# LUAD_rna_data <- GDCprepare(query_rna_LUAD.hg38)
# LUSC_rna_data <- GDCprepare(query_rna_LUSC.hg38)

# I have saved both R objects into disk
load("~/projects/mix_histology/data/TCGA_rna/TCGA_lung_rna.rda")
# a RangedSummarizedExperiment object
LUSC_rna_data
```

``` text
## class: RangedSummarizedExperiment
## dim: 57035 551
## metadata(0):
## assays(1): HTSeq - Counts
## rownames(57035): ENSG00000000003 ENSG00000000005 ...
##   ENSG00000281912 ENSG00000281920
## rowData names(3): ensembl_gene_id external_gene_name
##   original_ensembl_gene_id
## colnames(551): TCGA-77-8009-01A-11R-2187-07
##   TCGA-34-5239-01A-21R-1820-07 ... TCGA-NK-A7XE-01A-12R-A405-07
##   TCGA-43-6773-11A-01R-1949-07
## colData names(69): patient barcode ...
##   subtype_Homozygous.Deletions subtype_Expression.Subtype
```

The problem is with the meta data

``` r
LUSC_coldata<- colData(LUSC_rna_data)
LUAD_coldata<- colData(LUAD_rna_data)
```

### we will see the different representations of NAs

``` r
table(LUAD_coldata$subtype_Smoking.Status, useNA = "ifany")
```

``` text
##
##      Current reformed smoker for > 15 years
##                                          78
## Current reformed smoker for < or = 15 years
##                                          77
##                              Current smoker
##                                          47
##                         Lifelong Non-smoker
##                                          32
##                             [Not Available]
##                                          11
##                                        <NA>
##                                         349
```

``` r
table(LUSC_coldata$subtype_Smoking.Status, useNA = "ifany")
```

``` text
##
##      Current reformed smoker for > 15 years
##                                          51
## Current reformed smoker for < or = 15 years
##                                          87
##                              Current smoker
##                                          28
##                         Lifelong Non-smoker
##                                           7
##                                         N/A
##                                           6
##                                        <NA>
##                                         372
```

we see NAs are represented either as real `<NA>`, `[Not Avaiable]` or `N/A`. The first thing to do is to tidy the metadata changing all `NAs` to `<NA>`:\
I will use `stringr` from the `tidyverse` packages.

``` r
library(stringr)
# one needs to \\ to escape [ and ]
str_replace(LUAD_coldata$subtype_Smoking.Status, "\\[Not Available\\]", NA)
```

``` text
## Error: `replacement` must be a character vector
```

This does not quite work. stringr enforces that the replacement is the same type as a character. instead:

``` r
str_replace(LUAD_coldata$subtype_Smoking.Status, "\\[Not Available\\]", NA_character_) %>% table(useNA = "ifany")
```

``` text
## .
## Current reformed smoker for < or = 15 years
##                                          77
##      Current reformed smoker for > 15 years
##                                          78
##                              Current smoker
##                                          47
##                         Lifelong Non-smoker
##                                          32
##                                        <NA>
##                                         360
```

similarily:

``` r
str_replace(LUSC_coldata$subtype_Smoking.Status, "N/A", NA_character_) %>% table(useNA = "ifany")
```

``` text
## .
## Current reformed smoker for < or = 15 years
##                                          87
##      Current reformed smoker for > 15 years
##                                          51
##                              Current smoker
##                                          28
##                         Lifelong Non-smoker
##                                           7
##                                        <NA>
##                                         378
```

to test if a vector contains any NAs:

``` r
# this is invalid
# LUAD_coldata$subtype_Smoking.Status == NA, instead, use is.na()
is.na(LUAD_coldata$subtype_Smoking.Status) %>% table()
```

``` text
## .
## FALSE  TRUE
##   245   349
```

The traditional way to change a string to NA is:

``` r
myvector<- c("NA", "NA", "a", "b", "c", NA)
myvector
```

``` text
## [1] "NA" "NA" "a"  "b"  "c"  NA
```

``` r
myvector[myvector =="NA"]<- NA
myvector
```

``` text
## [1] NA  NA  "a" "b" "c" NA
```

### Conclusions

1.  Data are messy, even if the data are packaged from a big project such as TCGA.
2.  Different representation of `NA` could be a devil in your data, tidy it to R’s representation is needed.
3.  `stringr` is a great tool in your belt for tidying data.
