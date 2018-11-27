---
title: Merge featureCount table from RNAseq
author: Ming Tang
date: '2018-11-27'
slug: merge-featurecount-table-from-rnaseq
categories: []
tags: []
header:
  caption: ''
  image: ''
---

[featureCounts](http://bioinf.wehi.edu.au/featureCounts/) is a program to fast summarize counts from sequencing data. I use it to get gene-level RNAseq counts by

`featureCounts -p -t exon -g gene_id -a annotation.gtf -o mysample_featureCount.txt mapping_results_PE.bam`

If you have a lot of samples, you will get a lot of `*featureCount.txt` and you will
need to merge them for downstream analysis.

I will show you how to merge the tables using `R`, `python` and `unix` below.

#### R solution 
```r
library(purrr)
library(tidyverse)
f_files<- list.files("results/superEnhancer/rna_expression/MSTC", pattern = "featureCount.txt", full.names = T)

read_in_feature_counts<- function(file){
        cnt<- read_tsv(file, col_names =T, comment = "#")
        cnt<- cnt %>% dplyr::select(-Chr, -Start, -End, -Strand, -Length)
        return(cnt)
}
        
raw_counts<- map(f_files, read_in_feature_counts)
raw_counts_df<- purrr::reduce(raw_counts, inner_join) 
```

#### python solution

I am still very crude with python :)
It works for python2.

```python
import os
import csv
import glob

## after some google https://mail.python.org/pipermail/tutor/2004-November/033475.html
## The idea is to keep the count column into a list.

files = glob.glob("*featureCount.txt")
list_column = []
n = 1
for file in files:
    print n
    column_data = []
    with open(file, 'r') as f:
        reader = csv.reader(f, delimiter = "\t")
            # skip the comment line
        comment = next(reader)
        if n <= 1:
            for row in reader:
                # for the first file, keep the gene column as well
                column_data.append(row[0] + '\t' + row[6])
        else:
            for row in reader:
                column_data.append(row[6])
        n = n + 1
    list_column.append(column_data)


# This creates a list of row lists from the list of column lists
# If any of the column lists are too short they will be padded with None
# map function is a gem :)
rows = map(None, *list_column)

with open('output.txt','w') as f_out:
     for row in rows:
         f_out.write('\t'.join(row))
         f_out.write('\n')

```
### unix command line solution

```bash
# get the count
ls -1  *featureCount.txt | parallel 'cat {} | sed '1d' | cut -f7 {} > {/.}_clean.txt' 
ls -1  *featureCount.txt | head -1 | xargs cut -f1 > genes.txt
paste genes.txt *featureCount_clean.txt > output.txt

```