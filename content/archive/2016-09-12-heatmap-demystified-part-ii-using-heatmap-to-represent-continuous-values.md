---
title: 'Heatmap demystified part II: Using heatmap to represent continuous values'
author: Ming Tang
date: '2016-09-12T08:37:00-07:00'
lastmod: '2016-09-12T08:37:00-07:00'
slug: heatmap-demystified-part-ii-using-heatmap-to-represent-continuous-values
tags:
- heatmap
- PCA
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/09/heatmap-demystified-part-ii-using.html
description: In my last blog post, I showed an example to use heatmap to represent discrete values. I am going to continue the theme to introduce heatmap to represent…
summary: In my last blog post, I showed an example to use heatmap to represent discrete values. I am going to continue the theme to introduce heatmap to represent…
header:
  caption: ''
  image: ''
---

### Using heatmap to represent continuous values.

### 

In my last blog post, I showed an example to use heatmap to represent discrete values. I am going to continue the theme to introduce heatmap to represent continuous values. As I mentioned before, I will stress on three main points:

1.  Whether or not scale your data (center/standardize your data or not).
2.  Make sure you know the range of the data and do reasonable color mapping.
3.  How to perform the clustering. (which distance measure and linkage method to use).

In order to make the example reproducible and interesting to biologists, I am going to use an RNAseq data set downloaded by the `recount`package. The data contains 347 single cell RNA-seq data of 11 different distinct populations. Please read this Nature biotechnology paper: [Low-coverage single-cell mRNA sequencing reveals cellular heterogeneity and activated signaling pathways in developing cerebral cortex](http://www.nature.com/nbt/journal/v32/n10/full/nbt.2967.html). I will try to reproduce the figures in the paper.

#### Download the data by recount

#### 

``` r
#source("https://bioconductor.org/biocLite.R")

## use development branch of bioc
# useDevel()
#biocLite("recount")
library(recount)
library(dplyr)

## if you go to the paper you can find the project name
project = 'SRP041736'

# I already downloaded it, so comment it out
# download_study(project)

## load the data
load(file.path(project, 'rse_gene.Rdata'))

#We can explore a bit this RangedSummarizedExperiment
rse_gene
```

``` text
## class: RangedSummarizedExperiment
## dim: 23779 694
## metadata(0):
## assays(1): counts
## rownames(23779): 1 10 ... 9994 9997
## rowData names(3): gene_id bp_length symbol
## colnames(694): SRR1274192 SRR1274193 ... SRR1275349 SRR1275353
## colData names(21): project sample ... title characteristics
```

``` r
colData(rse_gene) %>% head()
```

``` text
## DataFrame with 6 rows and 21 columns
##                project      sample  experiment         run
##            <character> <character> <character> <character>
## SRR1274192   SRP041736   SRS603194   SRX534477  SRR1274192
## SRR1274193   SRP041736   SRS603194   SRX534477  SRR1274193
## SRR1274194   SRP041736   SRS603195   SRX534478  SRR1274194
## SRR1274195   SRP041736   SRS603195   SRX534478  SRR1274195
## SRR1274196   SRP041736   SRS603196   SRX534479  SRR1274196
## SRR1274197   SRP041736   SRS603196   SRX534479  SRR1274197
##            read_count_as_reported_by_sra reads_downloaded
##                                <integer>        <integer>
## SRR1274192                      16260602         16260602
## SRR1274193                         76776            76776
## SRR1274194                      13225500         13225500
## SRR1274195                         64552            64552
## SRR1274196                      16521630         16521630
## SRR1274197                         83534            83534
##            proportion_of_reads_reported_by_sra_downloaded paired_end
##                                                 <numeric>  <logical>
## SRR1274192                                              1       TRUE
## SRR1274193                                              1       TRUE
## SRR1274194                                              1       TRUE
## SRR1274195                                              1       TRUE
## SRR1274196                                              1       TRUE
## SRR1274197                                              1       TRUE
##            sra_misreported_paired_end mapped_read_count        auc
##                             <logical>         <integer>  <numeric>
## SRR1274192                      FALSE          16090529 1541642379
## SRR1274193                      FALSE             69879    2016766
## SRR1274194                      FALSE          13092059 1258982469
## SRR1274195                      FALSE             59210    1708001
## SRR1274196                      FALSE          16368219 1580611206
## SRR1274197                      FALSE             76808    2217637
##            sharq_beta_tissue sharq_beta_cell_type
##                  <character>          <character>
## SRR1274192                NA                   NA
## SRR1274193                NA                   NA
## SRR1274194                NA                   NA
## SRR1274195                NA                   NA
## SRR1274196                NA                   NA
## SRR1274197                NA                   NA
##            biosample_submission_date biosample_publication_date
##                          <character>                <character>
## SRR1274192   2014-04-22T17:28:44.407    2014-08-02T00:44:06.583
## SRR1274193   2014-04-22T17:28:44.407    2014-08-02T00:44:06.583
## SRR1274194   2014-04-22T17:28:45.247    2014-08-02T00:44:07.100
## SRR1274195   2014-04-22T17:28:45.247    2014-08-02T00:44:07.100
## SRR1274196   2014-04-22T17:28:45.343    2014-08-02T00:44:07.150
## SRR1274197   2014-04-22T17:28:45.343    2014-08-02T00:44:07.150
##              biosample_update_date avg_read_length geo_accession
##                        <character>       <integer>   <character>
## SRR1274192 2014-08-02T00:44:06.583             202            NA
## SRR1274193 2014-08-02T00:44:06.583              60            NA
## SRR1274194 2014-08-02T00:44:07.100             202            NA
## SRR1274195 2014-08-02T00:44:07.100              60            NA
## SRR1274196 2014-08-02T00:44:07.150             202            NA
## SRR1274197 2014-08-02T00:44:07.150              60            NA
##              bigwig_file       title characteristics
##              <character> <character> <CharacterList>
## SRR1274192 SRR1274192.bw          NA              NA
## SRR1274193 SRR1274193.bw          NA              NA
## SRR1274194 SRR1274194.bw          NA              NA
## SRR1274195 SRR1274195.bw          NA              NA
## SRR1274196 SRR1274196.bw          NA              NA
## SRR1274197 SRR1274197.bw          NA              NA
```

``` r
## At the gene level, the row data includes the gene ENTREZ ids, the gene
## symbols and the sum of the reduced exons widths, which can be used for
## taking into account the gene length.
rowData(rse_gene)
```

``` text
## DataFrame with 23779 rows and 3 columns
##           gene_id bp_length      symbol
##       <character> <integer> <character>
## 1               1      4027        A1BG
## 2              10      1317        NAT2
## 3             100      1532         ADA
## 4            1000      4473        CDH2
## 5       100008589      5071     RNA28S5
## ...           ...       ...         ...
## 23775        9991      8234       PTBP3
## 23776        9992       803       KCNE2
## 23777        9993      4882       DGCR2
## 23778        9994      6763    CASP8AP2
## 23779        9997      1393        SCO2
```

``` r
# browse_study(project)
```

#### Preparing the metadata

#### 

It turns out that the metadata returned by `recount` are many NAs. I have to go to [SRA run selector](http://trace.ncbi.nlm.nih.gov/Traces/study/) to find the metadata. great thanks to@Ayush.\
search `SRP041736` in the search box, and it will [return all the runs](http://trace.ncbi.nlm.nih.gov/Traces/study/?acc=SRP041736&go=go) for the project, and I downloaded the `RunInfo Table`.\
[SRAdb](http://www.bioconductor.org/packages/release/bioc/html/SRAdb.html) bioconductor package maybe able to search and download the metadata programmatically.\
Aso check [sra-search](https://github.com/ncbi/ngs-tools) from NCBI.\
Now, read in the metadata downloaded.

``` r
library(readr)
sra.runs<- read_tsv("~/Downloads/SraRunTable.txt")

## much more info I need!
head(sra.runs)
```

``` text
## Source: local data frame [6 x 35]
##
##    BioSample_s Experiment_s Library_Name_s LoadDate_s MBases_l MBytes_l
##          (chr)        (chr)          (chr)     (date)    (int)    (int)
## 1 SAMN02731786    SRX534477         2338_1 2014-05-08     1566     1094
## 2 SAMN02731786    SRX534477         2338_1 2014-05-08        2        1
## 3 SAMN02731795    SRX534478        2338_10 2014-05-08     1273      892
## 4 SAMN02731795    SRX534478        2338_10 2014-05-08        1        1
## 5 SAMN02731796    SRX534479        2338_11 2014-05-08     1591     1114
## 6 SAMN02731796    SRX534479        2338_11 2014-05-08        2        1
## Variables not shown: Run_s (chr), SRA_Sample_s (chr), Sample_Name_s (chr),
##   age_s (chr), biomaterial_provider_s (chr), cell_line_s (chr), disease_s
##   (chr), disease_stage_s (chr), population_s (chr), sex_s (chr), tissue_s
##   (chr), Assay_Type_s (chr), AssemblyName_s (chr), BioProject_s (chr),
##   BioSampleModel_s (chr), Center_Name_s (chr), Consent_s (chr),
##   InsertSize_l (int), LibraryLayout_s (chr), LibrarySelection_s (chr),
##   LibrarySource_s (chr), Organism_s (chr), Platform_s (chr), ReleaseDate_s
##   (date), SRA_Study_s (chr), g1k_analysis_group_s (chr), g1k_pop_code_s
##   (chr), isolate_s (chr), source_s (chr)
```

``` r
# View(sra.runs)
```

The function scale_counts() helps you scale them in a way that is tailored to `Rail-RNA` output.

``` r
## Scale counts by taking into account the total coverage per sample
rse <- scale_counts(rse_gene)

## check the count table

assay(rse)[1:6, 1:20]
```

``` text
##           SRR1274192 SRR1274193 SRR1274194 SRR1274195 SRR1274196
## 1                330          0          6          0        265
## 10                94          0          0          0          0
## 100             1905       3451          0          0          4
## 1000               3          0          8          0          6
## 100008589       2026       1567        921       1358       1622
## 100009613          0          0          0          0          0
##           SRR1274197 SRR1274198 SRR1274199 SRR1274200 SRR1274201
## 1                  0          1          0        430          0
## 10                 0          0          0          0          0
## 100                0        730          0          0          0
## 1000               0          6          0          6          0
## 100008589       1046       1445       1809       1283       2364
## 100009613          0          0          0          0          0
##           SRR1274202 SRR1274203 SRR1274204 SRR1274205 SRR1274206
## 1                 54          0        931        959          0
## 10                 0          0          0          0          0
## 100               12          0          0          0        452
## 1000               0          0          0          0         12
## 100008589       1703       1455       1812        843       2942
## 100009613          0          0          0          0          0
##           SRR1274207 SRR1274208 SRR1274209 SRR1274210 SRR1274211
## 1                  0          5          0        141          0
## 10                 0          0          0          0          0
## 100             2400         23          0        218          0
## 1000               0          3          0          2          0
## 100008589       4737       1047          0       3030       1993
## 100009613          0          0          0          0          0
```

match the `sra.runs$Run_s` to the `colData(rse)$run`

``` r
sra.runs$Run_s %in% colData(rse)$run %>% table()
```

``` text
## .
## FALSE  TRUE
##     2   694
```

``` r
colData(rse)$run  %in% sra.runs$Run_s %>% table()
```

``` text
## .
## TRUE
##  694
```

``` r
# which runs are not avaiable in the count matrix?
sra.runs$Run_s[! sra.runs$Run_s %in% colData(rse)$run]
```

``` text
## [1] "SRR1274250" "SRR1274344"
```

``` r
## there should be two same samples, one is deep sequenced, the other is shallow sequenced (downsampled)
colData(rse) %>% as.data.frame() %>% group_by(sample) %>% summarise(n=n()) %>% arrange(n) %>% head()
```

``` text
## Source: local data frame [6 x 2]
##
##      sample     n
##       (chr) (int)
## 1 SRS603223     1
## 2 SRS603270     1
## 3 SRS603194     2
## 4 SRS603195     2
## 5 SRS603196     2
## 6 SRS603197     2
```

``` r
sra.runs %>% dplyr::filter(Run_s == "SRR1274250") %>% select(SRA_Sample_s)
```

``` text
## Source: local data frame [1 x 1]
##
##   SRA_Sample_s
##          (chr)
## 1    SRS603223
```

``` r
sra.runs %>% dplyr::filter(Run_s == "SRR1274344") %>% select(SRA_Sample_s)
```

``` text
## Source: local data frame [1 x 1]
##
##   SRA_Sample_s
##          (chr)
## 1    SRS603270
```

``` r
colData(rse) %>% as.data.frame() %>% dplyr::filter(sample == "SRS603223")
```

``` text
##     project    sample experiment        run read_count_as_reported_by_sra
## 1 SRP041736 SRS603223  SRX534506 SRR1274251                         76576
##   reads_downloaded proportion_of_reads_reported_by_sra_downloaded
## 1            76576                                              1
##   paired_end sra_misreported_paired_end mapped_read_count     auc
## 1       TRUE                      FALSE             68808 1984754
##   sharq_beta_tissue sharq_beta_cell_type biosample_submission_date
## 1              <NA>                 <NA>   2014-04-22T17:28:47.330
##   biosample_publication_date   biosample_update_date avg_read_length
## 1    2014-08-02T00:44:08.360 2014-08-02T00:44:08.360              60
##   geo_accession   bigwig_file title characteristics
## 1          <NA> SRR1274251.bw  <NA>              NA
```

``` r
colData(rse) %>% as.data.frame() %>% dplyr::filter(sample == "SRS603270")
```

``` text
##     project    sample experiment        run read_count_as_reported_by_sra
## 1 SRP041736 SRS603270  SRX534553 SRR1274345                        169528
##   reads_downloaded proportion_of_reads_reported_by_sra_downloaded
## 1           169528                                              1
##   paired_end sra_misreported_paired_end mapped_read_count      auc
## 1       TRUE                      FALSE            150720 14263581
##   sharq_beta_tissue sharq_beta_cell_type biosample_submission_date
## 1              <NA>                 <NA>   2014-04-22T17:29:04.727
##   biosample_publication_date   biosample_update_date avg_read_length
## 1    2014-08-02T00:44:25.317 2014-08-02T00:44:25.317             196
##   geo_accession   bigwig_file title characteristics
## 1          <NA> SRR1274345.bw  <NA>              NA
```

These two samples are shallow sequenced ones according to the `mapped_read_count` number above. select out the smaller one to be the shallow sequenced ones.

``` r
shallow.run<- colData(rse) %>% as.data.frame() %>% group_by(sample) %>% dplyr::slice(which.min(mapped_read_count)) %>% ungroup()

boxplot(shallow.run$mapped_read_count)
title(main = "number of mapped reads for the shallow sequenced samples")
```

![](/img/archive/heatmap-demystified-part-ii-using-heatmap-to-represent-continuous-values/01-inline.png)

``` r
## merge the meta data
shallow.run <- shallow.run %>% left_join(sra.runs, by= c("run" = "Run_s"))

## summary of mapped_read_count in shallow ones. median of  ~0.2 million reads
summary(shallow.run$mapped_read_count)
```

``` text
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
##   11540  128900  201600  239800  323400  917300
```

``` r
## exclude the shallow runs to find the deep runs
shallow.runs<- shallow.run$run

deep.run<- colData(rse) %>% as.data.frame() %>% dplyr::filter(! run %in% shallow.runs) %>% ungroup()

boxplot(deep.run$mapped_read_count)
title(main = "number of mapped reads for the deep sequenced samples")
```

![](/img/archive/heatmap-demystified-part-ii-using-heatmap-to-represent-continuous-values/02-inline.png)

``` r
## merge the meta data
deep.run <- deep.run %>% left_join(sra.runs, by = c("run" = "Run_s"))
```

The data are always messy, the cell line name is spread to different columns. I will use regular expression to extract the first word until the space from the `population_s` column.

``` r
shallow.run %>% mutate(cell_line = gsub("([^ ]+).+", "\\1", population_s)) %>% select(cell_line) %>% table()
```

``` text
## .
##   2338   2339     BJ   chip   GW16   GW21 GW21+3   HL60    iPS   K562
##     22     17     37     46     26      8     16     54     24     42
##   Kera    NPC  plate
##     40     15      1
```

The names start with `chip` are K562 cells if you check `cell_line_s` column. The one starts with `plate` is the bulk sample (100 cells for K562 cells).\
from the method session of the paper:

> Hierarchical clustering of the top 500 PCA genes across 301 cells was also performed in the Fluidigm SINGuLAR package. Genes are clustered on the basis of Pearson correlation. Samples are clustered on the basis of a Euclidian distance matrix with complete linkage.

excluding the samples start with `chip` and `plate`:

``` r
22 + 17 + 37 + 26 + 8 + 16 + 54 + 24 + 42 + 40 + 15
```

``` text
## [1] 301
```

It is `301`, the same number of samples used in the paper, so I guess they excluded those k562 cells\
exclude those k562 cells to get only 301 single cells.

``` r
shallow.run <- shallow.run %>% mutate(cell_line = gsub("([^ ]+).+", "\\1", population_s))
shallow.run<- shallow.run %>% filter(! cell_line %in% c("chip", "plate"))
```

To reproduce figure 2C in the paper, I need to make a PCA plot. I will use `SVD` sigular value decomposition to accomplish that. I had a pretty detailed write-up for SVD and PCA [here](https://rpubs.com/crazyhottommy/PCA_MDS). Of course, one can use the functions `prcomp()` and `princomp()` from the built-in R stats package or`PCA()` from `FactoMineR` package.

``` r
## a peek into the count matrix
assay(rse)[1:6, 1:6]
```

``` text
##           SRR1274192 SRR1274193 SRR1274194 SRR1274195 SRR1274196
## 1                330          0          6          0        265
## 10                94          0          0          0          0
## 100             1905       3451          0          0          4
## 1000               3          0          8          0          6
## 100008589       2026       1567        921       1358       1622
## 100009613          0          0          0          0          0
##           SRR1274197
## 1                  0
## 10                 0
## 100                0
## 1000               0
## 100008589       1046
## 100009613          0
```

``` r
## select out only the counts for shallow ones
shallow.counts<- assay(rse)[, colnames(rse) %in% shallow.run$run]

## I need the cell type, tissue type info for each run in the order of runs (each column is a run) in the matrix

shallow.run.cell<- left_join(data.frame(run = colnames(shallow.counts)), shallow.run) %>% select(run, cell_line, tissue_s)
```

``` text
## Joining by: "run"
```

``` text
## Warning in left_join_impl(x, y, by$x, by$y): joining character vector and
## factor, coercing into character vector
```

``` r
## tidy the dataframe a bit, set the NPC tissue from N/A to brain for comparisons with GW lines, and iPS tissue from N/A to pluripotent

shallow.run.cell<- shallow.run.cell %>%
        mutate(tissue_s = gsub("(mammary gland);.+", "\\1", tissue_s)) %>%
        mutate(tissue_s = replace(tissue_s, tissue_s== "N/A" & cell_line == "iPS", "pluripotent")) %>%
        mutate(tissue_s = replace(tissue_s, tissue_s== "N/A" & cell_line == "NPC", "brain"))
```

#### PCA in action

#### 

``` r
## use log2 count for PCA and center the data for each sample.
X<- t(scale(t(log2(shallow.counts+1)),center=TRUE,scale=FALSE))
sv<- svd(t(X))
U<- sv$u
V<- sv$v
D<- sv$d
Z<- t(X)%*%V

## put PC1 and PC2 into a dataframe and add the cell line info side by side
pc_dat<- data.frame(cell.line = shallow.run.cell$cell_line, PC1 = Z[,1], PC2 = Z[,2])

## make figure with ggplot2
library(ggplot2)
library(ggthemes)
ggplot(pc_dat, aes(x=PC1, y=PC2, col=cell.line)) +
        geom_point() +
        theme_bw() +
        theme(panel.border = element_blank(),
              panel.grid.major = element_blank(),
              panel.grid.minor = element_blank(),
              axis.line.x = element_line(color="black", size = 0.6),
              axis.line.y = element_line(color="black", size = 0.6))
```

![](/img/archive/heatmap-demystified-part-ii-using-heatmap-to-represent-continuous-values/03-inline.png)Well, it is very similar to the original figure in the paper except the color and shape of the points. I mannually selected colors from<http://www.color-hex.com/>

``` r
p<- ggplot(pc_dat, aes(x=PC1, y=PC2)) +
        geom_point(aes(color = cell.line, shape = cell.line)) +
        scale_colour_manual(values=c("#24cdd4","#9e379f","#6673b7","#9FD175", "#9FD175",
                                     "#9FD175", "#ea018d", "#0d060f", "#eede51", "#6673b7", "#9FD175")) +
        scale_shape_manual(values=c(19, 19, 4, 19, 8, 3,19,19,19,19, 4))+
        theme_bw() +
        theme(panel.border = element_blank(),
              panel.grid.major = element_blank(),
              panel.grid.minor = element_blank(),
              axis.line.x = element_line(color="black", size = 0.6),
              axis.line.y = element_line(color="black", size = 0.6))

p
```

![](/img/archive/heatmap-demystified-part-ii-using-heatmap-to-represent-continuous-values/04-inline.png)\
Let’s fix the legend a bit to mimic the original figure.

``` r
p + guides(col = guide_legend(ncol=2)) + theme(legend.position = c(.9, .75))
```

![](/img/archive/heatmap-demystified-part-ii-using-heatmap-to-represent-continuous-values/05-inline.png)\
It is still not quite the same as the original figure as the legend of cell types should be grouped together. I will leave it to you to `google` and find a way to change the order of the legend. I usually import the figure to [`inkscape`](https://inkscape.org/en/) and mannually change the positions and/or orders. You may also notice that the X and Y axis scales are different from the orignal paper, I guess it is because that the authors used `RSEM` to process the RNA-seq data and used `TPM` (rather than raw counts in my example) to plot PCA. Nevertheless, the distribution of the clusters are very similar.\
Now it is time to reproduce figure 3b.\
From the paper:

> Hierarchical clustering of 65 single cells across 500 genes with the strongest PC1–PC3 loading scores identifies four major groups of cells (I–IV), and\
> k-means clustering identifies three clusters of genes (red, yellow and green)

So figure 3b only used cells that are from brain tissues. I want to show you how to do clustering for all the 11 cell types first then we can adventure ourselves by focusing on the brain tissue cells only (NPC, GW16, GW21, GW21+3). It is just a matter of subsetting only the cell lines that are of brain origin.\
Before drawing a biclustered heatmap, one needs to select features (genes) first becasue hierachincal clustering can be quite time/memory consuming if you have too many features. In addition, many features do not contribute to the clustering, so we can drop them.\
There are many ways to select the features, one of the most commonly used method is to use the most variable genes across samples.

``` r
library(genefilter)
rv<- rowVars(X)
## select the top 250 most variable genes for clustering
idx<- order(-rv)[1:250]

library(ComplexHeatmap) ## a very nice package from Zuguang Gu, he is very responsive.
Heatmap(X[idx,], name = "log2 RNAseq\ncounts scaled",
        show_row_names = FALSE, show_column_names = FALSE,
        row_dend_reorder = TRUE, column_dend_reorder = TRUE)
```

Continue reading at <https://rpubs.com/crazyhottommy/heatmap_demystified>

###
