---
title: Three gotchas when using R for Genomic data analysis
author: Ming Tang
date: '2018-11-27'
slug: three-gotchas-when-using-r-for-genomic-data-analysis
categories:
  - R
  - genomics
tags:
  - genomics
  - R
header:
  caption: ''
  image: ''
---
During my daily work with R for genomic data analysis, I encountered several instances that R gives me some (bad) surprises.

#### 1. The devil 1 and 0 coordinate system

read detail here https://github.com/crazyhottommy/DNA-seq-analysis#tips-and-lessons-learned-during-my-dna-seq-data-analysis-journey

some files such as `bed` file is 0 based. Two genomic regions:

```{r}
chr1    0    1000
chr1    1001    2000
```
when you import that bed file into R using `rtracklayer::import()`, it will become

```{r}
chr1     1    1000
chr1    1000    2000
```
The function convert it to 1 based internally (R is 1 based unlike python). 

The problem is that when you read the bed file with `read.table` and use `GenomicRanges::makeGRangesFromDataFrame()` to convert it to a GRanges object, do not forget to add 1 to the start before doing it.

Similarily, when you write a GRanges object to disk using `rtracklayer::export`, you do not need to worry, R will convert it back to 0 based in file.

However, if you make a dataframe out of the GRanges object, you need to remember do `start -1` before writing to a file.



#### 2. `read_tsv` column types

If you use `read_tsv` from [`readr`](https://github.com/tidyverse/readr), it will use the first 1000 rows to determine the column types (integer, charater etc). For genomic data, however, especially for the chromosome column, you may or may not have `chr` prepending. 

```{r}
1    0    1000    
1    1000    2000
.
.
.
X
Y
MT

```
you may fail to read rows for chromosome X, Y and MT. (To make things worse, UCSC uses chrM rather than chrMT...)

The solution is that read in all the data as characters.

```{r}
library(readr)
challenge2 <- read_tsv("my.bed", 
  col_types = cols(.default = col_character())
)
```
see http://r4ds.had.co.nz/data-import.html

#### 3. Scientific notation for genomic coordinates

This is kind of related to 2. `1200000` will be written as `1.2e6` in a dataframe if R thinks it is an integer. So, you will need to read in the columns all as characters, or if you convert the character to numeric and wants to write to a file,
add `options(scipen=500)` on the top of your script.

The scientific notation can not be disabled in `write_csv`: https://github.com/tidyverse/readr/issues/671


#### One more gotcha for rownames and colnames

base R will change the name with `-` to a `.`. e.g. TCGA-06-ABCD will be changed to TCGA.06.ABCD. this can cause troubles when you use the name of the columns to match samples. `readr` will maintain the `-`.


