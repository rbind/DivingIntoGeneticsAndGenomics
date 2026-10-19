---
title: read in all the files in a folder to R
author: Ming Tang
date: '2016-02-07T20:14:00-08:00'
lastmod: '2016-02-07T20:14:00-08:00'
slug: read-in-all-the-files-in-a-folder-to-r
tags:
- bed
- lapply
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/02/read-in-all-files-in-folder-to-r.html
description: see a post\ I downloaded some broadpeak data from UCSC and put them in a folder /Users/mtang1/projects/ENCODE_ChIPseq/data/\ I cut only first four columns…
summary: see a post\ I downloaded some broadpeak data from UCSC and put them in a folder /Users/mtang1/projects/ENCODE_ChIPseq/data/\ I cut only first four columns…
header:
  caption: ''
  image: ''
---

#### *Ming Tang*

#### *February 7, 2016*

### read in all the files in a folder with a loop

#### 

see a [post](http://stackoverflow.com/questions/9564489/opening-all-files-in-a-folder-and-applying-a-function)\
I downloaded some broadpeak data from [UCSC](http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeHaibTfbs/) and put them in a folder `/Users/mtang1/projects/ENCODE_ChIPseq/data/`\
I cut only first four columns of those files and How can I read in all the bed files as GRanges?

``` r
# the base of the path
cancer.base<- "/Users/mtang1/projects/ENCODE_ChIPseq/data"

# list.files will print out all the files in the folder ending with broadPeak
cancer.lines.peak.files<- list.files(cancer.base, pattern="*.broadPeak$", full.names=TRUE)
cancer.lines.peak.files
```

``` text
## [1] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsEcc1Pol2V0416102Dm002p1hPkRep2.broadPeak"
## [2] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsHct116Pol24h8V0416101PkRep2.broadPeak"
## [3] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsHelas3Pol2Pcr1xPkRep2.broadPeak"
## [4] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsHepg2Pol2Pcr2xPkRep2.broadPeak"
## [5] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsK562Pol2V0416101PkRep2.broadPeak"
## [6] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsPanc1Pol24h8V0416101PkRep1.broadPeak"
## [7] "/Users/mtang1/projects/ENCODE_ChIPseq/data/wgEncodeHaibTfbsU87Pol24h8V0416101PkRep2.broadPeak"
```

``` r
# using a for loop to read in all the files and assign the cell-line name as the varaible name of the GRanges
# import function in rtracklayer will read in bed file as GRanges

library(rtracklayer)
for (file in cancer.lines.peak.files){
        # extract the cell line name
        cell.line.name <- gsub(".+wgEncodeHaibTfbs(.+)Pol2.+", "\\1", file)
        print (sprintf("reading in %s peak file", cell.line.name))
        file.handler<- file
        print (cell.line.name)
        # assign the cell-line name as the varaible name of the GRanges
        assign(cell.line.name, import(file.handler, format = "BED"))

}
```

``` text
## [1] "reading in Ecc1 peak file"
## [1] "Ecc1"
## [1] "reading in Hct116 peak file"
## [1] "Hct116"
## [1] "reading in Helas3 peak file"
## [1] "Helas3"
## [1] "reading in Hepg2 peak file"
## [1] "Hepg2"
## [1] "reading in K562 peak file"
## [1] "K562"
## [1] "reading in Panc1 peak file"
## [1] "Panc1"
## [1] "reading in U87 peak file"
## [1] "U87"
```

``` r
Ecc1
```

``` text
## GRanges object with 35977 ranges and 1 metadata column:
##           seqnames                 ranges strand   |        name
##              <Rle>              <IRanges>  <Rle>   | <character>
##       [1]     chr1       [713816, 714479]      *   |       peak1
##       [2]     chr1       [762384, 763181]      *   |       peak2
##       [3]     chr1       [859112, 859462]      *   |       peak3
##       [4]     chr1       [878092, 878373]      *   |       peak4
##       [5]     chr1       [878472, 878807]      *   |       peak5
##       ...      ...                    ...    ... ...         ...
##   [35973]     chrX [154299651, 154300068]      *   |   peak35973
##   [35974]     chrX [154444181, 154444945]      *   |   peak35974
##   [35975]     chrX [154493584, 154493918]      *   |   peak35975
##   [35976]     chrX [154841987, 154842828]      *   |   peak35976
##   [35977]     chrX [155110716, 155111326]      *   |   peak35977
##   -------
##   seqinfo: 23 sequences from an unspecified genome; no seqlengths
```

``` r
U87
```

``` text
## GRanges object with 42355 ranges and 1 metadata column:
##           seqnames                 ranges strand   |        name
##              <Rle>              <IRanges>  <Rle>   | <character>
##       [1]     chr1       [ 10081,  10449]      *   |       peak1
##       [2]     chr1       [713834, 714370]      *   |       peak2
##       [3]     chr1       [752419, 752810]      *   |       peak3
##       [4]     chr1       [762601, 763171]      *   |       peak4
##       [5]     chr1       [842211, 842561]      *   |       peak5
##       ...      ...                    ...    ... ...         ...
##   [42351]     chrX [154996980, 154997243]      *   |   peak42351
##   [42352]     chrX [154997284, 154997491]      *   |   peak42352
##   [42353]     chrX [155110793, 155111216]      *   |   peak42353
##   [42354]     chrX [155117874, 155118167]      *   |   peak42354
##   [42355]     chrX [155259636, 155260008]      *   |   peak42355
##   -------
##   seqinfo: 23 sequences from an unspecified genome; no seqlengths
```

### Use lapply

#### 

It is awkward to use for loop in R, let’s just use lapply function. see a [post](http://brianmannmath.github.io/blog/2014/01/20/using-lapply-to-import-files-to-r/)

``` r
# full path to every file

## lapply will use import to read in every bed file and keep them in a list.
data<- lapply(cancer.lines.peak.files, import, format = "BED")
names(data)<- gsub(".+wgEncodeHaibTfbs(.+)Pol2.+", "\\1", cancer.lines.peak.files)

data$U87
```

``` text
## GRanges object with 42355 ranges and 1 metadata column:
##           seqnames                 ranges strand   |        name
##              <Rle>              <IRanges>  <Rle>   | <character>
##       [1]     chr1       [ 10081,  10449]      *   |       peak1
##       [2]     chr1       [713834, 714370]      *   |       peak2
##       [3]     chr1       [752419, 752810]      *   |       peak3
##       [4]     chr1       [762601, 763171]      *   |       peak4
##       [5]     chr1       [842211, 842561]      *   |       peak5
##       ...      ...                    ...    ... ...         ...
##   [42351]     chrX [154996980, 154997243]      *   |   peak42351
##   [42352]     chrX [154997284, 154997491]      *   |   peak42352
##   [42353]     chrX [155110793, 155111216]      *   |   peak42353
##   [42354]     chrX [155117874, 155118167]      *   |   peak42354
##   [42355]     chrX [155259636, 155260008]      *   |   peak42355
##   -------
##   seqinfo: 23 sequences from an unspecified genome; no seqlengths
```
