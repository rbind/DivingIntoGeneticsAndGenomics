---
title: DESeq work flow
author: Ming Tang
date: '2013-11-01T19:57:00-07:00'
lastmod: '2013-11-01T19:57:00-07:00'
slug: deseq-work-flow
tags:
- DESeq
- RNAseq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/11/deseq-work-flow.html
description: I was working on a human RNA-seq data set, and I already got the count matrix by HTSeq.\ see my previous post here RNA-seq analysis samtools sort and…
summary: I was working on a human RNA-seq data set, and I already got the count matrix by HTSeq.\ see my previous post here RNA-seq analysis samtools sort and…
header:
  caption: ''
  image: ''
---

I was working on a human RNA-seq data set, and I already got the count matrix by HTSeq.\
see my previous post here [RNA-seq analysis samtools sort and htseq-count](/archive/rna-seq-analysis-samtools-sort-and-htseq-count/)

Now, I am ready to use DESeq to detect the differentially expressed genes.\
I followed the nature protocol from Simon Anders and here <http://dwheelerau.com/2013/04/15/how-to-use-deseq-to-analyse-rnaseq-data/>\
for more details read the DESeq manual.

**  **

{{< gist crazyhottommy 7272522 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/7272522)

``` r
several out put
```

``` r
> head(countsTable)
          a1  b1  a2 b2  a3 b3
0R7H2P    21   9   3  6   1  4
5S_rRNA   55  87  56 56  34 40
7SK      171  89  61 84  57 47
A1BG      15  34  38 10  12 23
A1BG-AS1  65  49 140 41  40 44
A1CF     293 109 417 90 181 84
```

``` r
> head(counts(cds,normalized=TRUE))
                a1        b1         a2        b2         a3
0R7H2P    14.44370  7.348284   2.360913  5.935807   1.583712
5S_rRNA   37.82875 71.033416  44.070367 55.400869  53.846220
7SK      117.61301 72.666368  48.005221 83.101303  90.271605
A1BG      10.31693 27.760186  29.904892  9.893012  19.004548
A1BG-AS1  44.70670 40.007326 110.175917 40.561350  63.348494
A1CF     201.52404 88.995889 328.166838 89.037110 286.651937
                 b3
0R7H2P     5.492968
5S_rRNA   54.929684
7SK       64.542378
A1BG      31.584568
A1BG-AS1  60.422652
A1CF     115.352336
```

``` r
dispersion plot
```

![](/img/archive/deseq-work-flow/01-dispersion_plot.png)

``` r
MAplot: there are not many genes that are differentially expressed. Usually you should see red dots around the edge of the cloud.
```

![](/img/archive/deseq-work-flow/02-maplot.png)

``` r
pvalue histogram, there are not many genes with a low p-value, usually you see a spike near x=0
```

![](/img/archive/deseq-work-flow/03-pvalue.png)

``` r
PCA plot. PCA does not look great, one replicate for each cell type varies too much with the other two replicates, and it looks the same situation from the original PCA figure in the paper below.
```

![](/img/archive/deseq-work-flow/04-pca.png)

``` r
figure2 from the original paper http://www.jci.org/articles/view/66514/figure/2
```

![](/img/archive/deseq-work-flow/05-jci66514.f2.jpg)

``` r
differentially expressed genes  according to figure 2 ( I read the supplement material, those genes are with more than 2 fold changes)
```

``` r
alpha cell specific: IRX2, PCSK2, DPP4,IRX1,GCG, PTPRD, ARX, HNF1A
```

``` r
beta cell specific: MAFA, GLP1R, PCSK1, PDX1, NKX6-1, CDKN1C, KCNQ2, INS, SLC30A8, HDAC9, IAPP, KCNJ11
```

``` r
My DESeq results:
```

``` r
> resSig[resSig$id=="GCG",]
       id baseMean baseMeanA baseMeanB foldChange log2FoldChange        pval padj
15096 GCG 127322.8  245014.6  9630.952 0.03930766      -4.669046 0.008959299    1

similarly I got others:

25252 PCSK2 1605.296  2448.875  761.7172  0.3110478      -1.684792 0.01701203    1
```

``` r
17931 IRX2 437.8337  764.6114  111.0561  0.1452451      -2.783438  0.00364283    1

12803 DPP4  418.095  755.6109  80.57906  0.1066409      -3.229167  0.01849249    1

6374 ARX  104.137  166.6132  41.66077  0.2500449        -1.999741  0.01581663    1

        id baseMean baseMeanA baseMeanB foldChange log2FoldChange        pval padj
20260 MAFA 802.9027  156.4095  1449.396   9.266673       3.212052  0.04860831    1

15303 GLP1R 265.7284  172.2726  359.1841   2.084975        1.06003 0.05890757    1

25250 PCSK1 6597.262  2279.658  10914.87   4.787941       2.259405 0.09637812    1

8682 CDKN1C 581.1144  198.1623  964.0666   4.865035       2.28245 0.009237992    1

18299 KCNQ2 141.9849   86.3116  197.6583   2.290055       1.195382  0.0422248    1

17827 INS 7444.435  1668.456  13220.41    7.92374       2.986181  0.002803966    1

16172 HDAC9 527.0126   260.651  793.3742   3.043818      1.605882 0.004025324    1

17099 IAPP 6943.366  2072.604  11814.13   5.700136       2.510996  0.06844496    1

I only missed IRX1,PTPRD and HNF1A in alpha cells

and missed PDX1, SLC30A8 and KCNJ11 in beta cells.

> res[res$id=="IRX1",]
        id baseMean baseMeanA baseMeanB foldChange log2FoldChange  pval padj

17929 IRX1 115.7047  134.3566  97.05282  0.7223525      -0.469225 0.4556522    1

I checked the other missed ones, they are all with big p-values.  I do note that the P-adj values are all very big, that may due to the big variation among the replicates. The original paper used the FPKM rather than raw counts to detect the differentially expressed genes. However, considering the large overlapping gene list, I am pretty satisfied with my analysis. further reading counts VS FPKM http://www.cureffi.org/2013/09/12/counts-vs-fpkms-in-rna-seq/.I may try to use cuffdiff to do the same analysis some time later.

Alternatively, you can upload your raw count matrix to DEG-Vis http://www.vicbioinformatics.com/dge-vis/index.html to perform similar analysis without the knowledge of R. DEG-Vis uses EdgeR and limma internally.
```
