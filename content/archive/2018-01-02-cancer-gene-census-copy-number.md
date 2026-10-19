---
title: cancer gene census copy number
author: Ming Tang
date: '2018-01-02T11:00:00-08:00'
lastmod: '2018-01-02T11:00:00-08:00'
slug: cancer-gene-census-copy-number
tags:
- COSMIC
- dplyr
- oncogene
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2018/01/cancer-gene-census-copy-number.html
description: 'knitr will force changing current working directory: \ \ or use the ezkintr package'
summary: 'knitr will force changing current working directory: \ \ or use the ezkintr package'
header:
  caption: ''
  image: ''
---

## *Ming Tang*

#### *January 2, 2018*

### Set up

knitr will force changing current working directory: <https://philmikejones.wordpress.com/2015/05/20/set-root-directory-knitr/>\
<https://github.com/yihui/knitr/issues/277>\
or use the ezkintr package <https://deanattali.com/blog/ezknitr-package/>

``` r
library(knitr)
library(here)
```

``` text
## here() starts at /Users/mtang1/projects/mixing_histology_lung_cancer
```

``` r
root.dir<- here()
opts_knit$set(root.dir = root.dir)
```

### read in the data

I am going to check the COSMIC database on cancer genes. Specifically, I want to know which cancer-related genes are found amplified and which are deleted.\
The data can be downloaded from <http://cancer.sanger.ac.uk/cosmic/census?genome=37#cl_search>

``` r
library(tidyverse)
```

``` text
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

``` text
## Conflicts with tidy packages ----------------------------------------------
```

``` text
## filter(): dplyr, stats
## lag():    dplyr, stats
```

``` r
library(janitor)
library(stringr)
cancer_gene_census<- read_csv("data/COSMIC_Cancer_gene_Census/cancer_gene_census.csv", col_names = T)
```

``` text
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   `Entrez GeneId` = col_integer(),
##   Tier = col_integer()
## )
```

``` text
## See spec(...) for full column specifications.
```

### tidy the data with janitor::clean_names(), dplyr::unnest

The column names have spaces, and in many columns there are mulitple strings separated by `,`.

``` r
## dplyr after 0.7.0 use pull to get one column out as a vector, I was using .$
# https://stackoverflow.com/questions/27149306/dplyrselect-one-column-and-output-as-vector

#cancer_gene_census %>% pull(`Gene Symbol`) %>% unique()
#cancer_gene_census %>% distinct(`Gene Symbol`)

## extract genes that are amplified or deleted in cancers. in the `mutation types` column search A for amplificaton and D for large deletion.

## use janitor::clean_names() to clean the column names with space.
cancer_gene_census %>%
        clean_names() %>%
        mutate(mutation_type =strsplit(as.character(mutation_types), ",")) %>%
        unnest(mutation_type) %>% tabyl(mutation_type)
```

``` text
##    mutation_type   n      percent valid_percent
## 1              N   1 0.0008257638  0.0008264463
## 2              A   8 0.0066061107  0.0066115702
## 3              D   8 0.0066061107  0.0066115702
## 4              F 139 0.1147811726  0.1148760331
## 5            Mis  68 0.0561519405  0.0561983471
## 6              N 147 0.1213872832  0.1214876033
## 7              O  37 0.0305532618  0.0305785124
## 8              S  76 0.0627580512  0.0628099174
## 9              T  19 0.0156895128  0.0157024793
## 10             A  18 0.0148637490  0.0148760331
## 11             D  35 0.0289017341  0.0289256198
## 12             F  32 0.0264244426  0.0264462810
## 13        F; Mis   1 0.0008257638  0.0008264463
## 14             M   3 0.0024772915  0.0024793388
## 15           Mis 240 0.1981833196  0.1983471074
## 16        Mis. N   1 0.0008257638  0.0008264463
## 17             N  24 0.0198183320  0.0198347107
## 18             O   7 0.0057803468  0.0057851240
## 19  Promoter Mis   1 0.0008257638  0.0008264463
## 20             S   2 0.0016515277  0.0016528926
## 21             T 343 0.2832369942  0.2834710744
## 22          <NA>   1 0.0008257638            NA
```

It turns out that single mutation_types column is more messy than you think… Multiple entries of A and D in different rows. spaces in the column are the devil.

### trim the spaces with stringr::str_trim()

Abbreviations can be found <http://cancer.sanger.ac.uk/cosmic/census?genome=37#cl_download>

``` r
# trim the white space
cancer_gene_census %>%
        clean_names() %>%
        mutate(mutation_type =strsplit(as.character(mutation_types), ",")) %>%
        unnest(mutation_type) %>%
        mutate(mutation_type = str_trim(mutation_type)) %>%
        filter(mutation_type == "A" | mutation_type == "D") %>% tabyl(mutation_type)
```

``` text
##   mutation_type  n   percent
## 1             A 26 0.3768116
## 2             D 43 0.6231884
```

``` r
cancer_gene_census %>%
        clean_names() %>%
        mutate(mutation_type =strsplit(as.character(mutation_types), ",")) %>%
        unnest(mutation_type) %>%
        mutate(mutation_type = str_trim(mutation_type)) %>%
        filter(mutation_type == "A" | mutation_type == "D") %>% count(mutation_type)
```

``` text
## # A tibble: 2 x 2
##   mutation_type     n
##           <chr> <int>
## 1             A    26
## 2             D    43
```

### Sanity check

according to the website <http://cancer.sanger.ac.uk/cosmic/census?genome=37#cl_sub_tables> there are should be 40 deletions and 24 amplifications while I am getting 43 and 26, respectively.

``` r
cancer_gene_census %>%
        clean_names() %>%
        mutate(mutation_type =strsplit(as.character(mutation_types), ",")) %>%
        unnest(mutation_type) %>%
        mutate(mutation_type = str_trim(mutation_type)) %>%
        filter(mutation_type == "A")
```

``` text
## # A tibble: 26 x 21
##    gene_symbol
##          <chr>
##  1        AKT2
##  2        AKT3
##  3         ALK
##  4       CCNE1
##  5      DROSHA
##  6        EGFR
##  7       ERBB2
##  8         ERG
##  9        FLT4
## 10        GRM3
## # ... with 16 more rows, and 20 more variables: name <chr>,
## #   entrez_geneid <int>, genome_location <chr>, tier <int>,
## #   hallmark <chr>, chr_band <chr>, somatic <chr>, germline <chr>,
## #   tumour_types_somatic <chr>, tumour_types_germline <chr>,
## #   cancer_syndrome <chr>, tissue_type <chr>, molecular_genetics <chr>,
## #   role_in_cancer <chr>, mutation_types <chr>,
## #   translocation_partner <chr>, other_germline_mut <chr>,
## #   other_syndrome <chr>, synonyms <chr>, mutation_type <chr>
```

I checked by eyes, AKT3 and GRM3 are missing from the table online <http://cancer.sanger.ac.uk/cosmic/census/tables?name=amp> but when I checked the downloaded table, both AKT3 and GRM3 has Amplification in the mutation types column. I am bit confused, but for now I will stick to the downloaded table.\
It teaches me an important lesson.

1.  Do not trust data (blindly) from any resource even COSMIC.

2.  Data are always messy. Tidying data is the big job.

3.  Becareful with the data, check with sanity. Browse the data with your eyes may reveal some unexpected things.

### oncogenes are amplified and tumor suppressor genes are always deleted?

In cancer, we tend to think oncogenes are amplified and tumor suppressor genes are deleted to promote tumor progression. Is it true in this setting?

``` r
#devtools::install_github("haozhu233/kableExtra")
library(kableExtra)
library(knitr)
cancer_gene_census %>%
        clean_names() %>%
        mutate(mutation_type =strsplit(as.character(mutation_types), ",")) %>%
        unnest(mutation_type) %>%
        mutate(mutation_type = str_trim(mutation_type)) %>%
        filter(mutation_type == "A" | mutation_type == "D") %>%
        mutate(role = strsplit(as.character(role_in_cancer), ",")) %>%
        unnest(role) %>%
        mutate(role = str_trim(role)) %>%
        filter(role == "TSG" | role == "oncogene") %>%
        count(role, mutation_type) %>% kable()
```

| role     | mutation_type |   n |
|:---------|:--------------|----:|
| oncogene | A             |  25 |
| oncogene | D             |   7 |
| TSG      | A             |   3 |
| TSG      | D             |  42 |

#### what are those genes?

``` r
cancer_gene_census %>%
        clean_names() %>%
        mutate(mutation_type =strsplit(as.character(mutation_types), ",")) %>%
        unnest(mutation_type) %>%
        mutate(mutation_type = str_trim(mutation_type)) %>%
        filter(mutation_type == "A" | mutation_type == "D") %>%
        mutate(role = strsplit(as.character(role_in_cancer), ",")) %>%
        unnest(role) %>%
        mutate(role = str_trim(role)) %>%
        filter(role == "TSG" | role == "oncogene") %>%
        filter((role == "TSG" & mutation_type == "A") | (role == "oncogene" & mutation_type == "D")) %>%
        select(gene_symbol, role_in_cancer, role, mutation_types, mutation_type) %>%
        kable(format = "html", booktabs = T, caption = "TSG and oncogene") %>%
        kable_styling(latex_options = c("striped", "hold_position"),
                full_width = F)
```

| gene_symbol | role_in_cancer        | role     | mutation_types     | mutation_type |
|-------------|-----------------------|----------|--------------------|---------------|
| APOBEC3B    | oncogene, TSG         | oncogene | D                  | D             |
| BIRC3       | oncogene, TSG, fusion | oncogene | D, F, N, T, Mis    | D             |
| DROSHA      | TSG                   | TSG      | A, Mis, N, F       | A             |
| GPC3        | oncogene, TSG         | oncogene | D, Mis, N, F, S    | D             |
| KDM6A       | oncogene, TSG         | oncogene | D, N, F, S         | D             |
| MAP2K4      | oncogene, TSG         | oncogene | D, Mis, N          | D             |
| NKX2-1      | oncogene, TSG         | TSG      | A                  | A             |
| NTRK1       | oncogene, TSG, fusion | TSG      | T, A               | A             |
| PAX5        | oncogene, TSG, fusion | oncogene | T, Mis, D, F, S    | D             |
| WT1         | oncogene, TSG, fusion | oncogene | D, Mis, N, F, S, T | D             |

TSG and oncogene

majority of the genes from the output can function as either oncogene or tumor suppressor genes (TSG), which is not totally suprising. Cancer is such a complex disease that a function of the gene is dependent on the context (e.g. cancer types). Interestingly, `DROSHA` is the only TSG and is amplified.
