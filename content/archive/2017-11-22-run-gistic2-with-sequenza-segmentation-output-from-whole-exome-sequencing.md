---
title: run gistic2 with sequenza segmentation output from whole exome sequencing
author: Ming Tang
date: '2017-11-22T12:35:00-08:00'
lastmod: '2017-11-22T12:35:00-08:00'
slug: run-gistic2-with-sequenza-segmentation-output-from-whole-exome-sequencing
tags:
- CNV
- gistic2
- WES
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/11/run-gistic2-with-sequenza-segmentation.html
description: Gistic was designed for SNP6 array data. I saw many papers use it for whole exome sequencing data as well.\ I have the segment files from sequenza and want…
summary: Gistic was designed for SNP6 array data. I saw many papers use it for whole exome sequencing data as well.\ I have the segment files from sequenza and want…
header:
  caption: ''
  image: ''
---

### Convert sequenza output to gistic input

[Gistic](http://portals.broadinstitute.org/cgi-bin/cancer/publications/pub_paper.cgi?mode=view&paper_id=216&p=t) was designed for SNP6 array data. I saw many papers use it for whole exome sequencing data as well.\
I have the segment files from sequenza and want to convert them to the gistic input.

#### Input format for gistic:

segment file:\
(1) Sample (sample name)\
(2) Chromosome (chromosome number)\
(3) Start Position (segment start position, in bases)\
(4) End Position (segment end position, in bases)\
(5) Num markers (number of markers in segment)\
(6) [Seg.CN](http://seg.cn/) (log2() -1 of copy number)\
see a link <https://groups.google.com/a/broadinstitute.org/forum/?utm_medium=email&utm_source=footer#!msg/gistic-forum/yYxIe58qLkA/4dXWAPuMEgAJ>

1.  The conversion should be log2 (logarithm base 2) - 1, so that copy number 2 is 0.

2.  Every segment start and end in the segments file should appear in the markers file, not the other way around.

> when the copy number is 0 (a homozygous deletion of both copies). You can’t do a log2(0)-1, just put a small number e.g. -5

marker file:\
<https://groups.google.com/a/broadinstitute.org/forum/#!searchin/gistic-forum/marker$20file/gistic-forum/Vq9WWDiy7jU/BSFg2zmBZ1EJ>\
(1) Marker Name\
(2) Chromosome\
(3) Marker Position (in bases)\
Note gistic2 does not require a marker file anymore.

#### output of sequenza

sequenza gives a segment file. Segmentation was done by [copynumber](http://bioconductor.org/packages/release/bioc/html/copynumber.html)bioconductor package.\
13 columns of the `*segments.txt` file

``` text
"chromosome" "start.pos" "end.pos" "Bf" "N.BAF" "sd.BAF" "depth.ratio" "N.ratio" "sd.ratio" "CNt" "A" "B" "LPP"
```

We only need the `chromosome`, `start.pos`, `end.pos`, `N.BAF` and `depth.ratio`columns.\
The `depth.ratio` column is the GC content normalized ratio. a depth ratio of 1 means it has copy number of 2 (the same as the normal blood control in my case).

UPDATED: 12/17/2017. see a comment below. it is not log2(2^ depth.ratio) -1 rather:

To convert to gistic input, I have to do `log2(2 * depth.ratio) - 1`

UPDATED 01/03/2018\
I have a bunch of sgement files in the same folder.\
add the sample name in the final column and do the log2 math in R.

``` r
library(tidyverse)
library(readr)
seg_files<- list.files(".", pattern = "*segments.txt", full.names = F)

seg_dat_list <- lapply(seg_files, function(f) {
        dat<- read_tsv(f, col_names = T, col_types = cols(.default = col_character()))
        sample<- gsub("_vs_.*segments.txt", "", f)
        dat$sample<- sample
        return(dat)
})

seg_dat <- do.call(rbind, seg_dat_list)

gistic_input<- seg_dat %>% select(sample, chromosome, start.pos, end.pos, N.BAF, depth.ratio) %>% mutate(depth.ratio = as.numeric(depth.ratio)) %>% mutate(depth.ratio = log2(2 * depth.ratio) -1)

write_tsv(gistic_input, "all_segments.txt")
```

Back to bash:

``` bash
## marker file:

cat all_segments.txt | sed '1d' | cut -f2,3 > markers.txt
cat all_segments.txt | sed '1d' | cut -f2,4 >> markers.txt

## sort the files by chromosome, take the unique ones and number the markers.

cat markers.txt | sort -V -k1,1 -k2,2nr | uniq | nl > markers_gistic.txt
```

Run gistic

modify the `gistic2` script a bit. e.g. change `MCR_ROOT folder path`

``` bash
#!/bin/sh
## set MCR environment and launch GISTIC executable

## NOTE: change the line below if you have installed the Matlab MCR in an alternative location
MCR_ROOT=/scratch/genomic_med/apps/Matlab_Complier_runTime
MCR_VER=v83

echo Setting Matlab MCR root to $MCR_ROOT

## set up environment variables
LD_LIBRARY_PATH=$MCR_ROOT/$MCR_VER/runtime/glnxa64:$LD_LIBRARY_PATH
LD_LIBRARY_PATH=$MCR_ROOT/$MCR_VER/bin/glnxa64:$LD_LIBRARY_PATH
LD_LIBRARY_PATH=$MCR_ROOT/$MCR_VER/sys/os/glnxa64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH
XAPPLRESDIR=$MCR_ROOT/$MCR_VER/MATLAB_Component_Runtime/v83/X11/app-defaults
export XAPPLRESDIR

## launch GISTIC executable
./gp_gistic2_from_seg $@
```

I removed `./` from the last line since I have put all executables in my path.

``` bash
mkdir gistic_out
gistic2 -b gistic_out -seg all_segments.txt -refgene /scratch/genomic_med/apps/gistic/refgenefiles/hg19.mat -mk markers_gistic.txt  -genegistic 1 -smallmem 1 -broad 1 -brlen 0.5 -conf 0.90 -armpeel 1 -savegene 1 -gcm extreme
```
