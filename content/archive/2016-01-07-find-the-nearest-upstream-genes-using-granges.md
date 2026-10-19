---
title: find the nearest upstream genes using GRanges
author: Ming Tang
date: '2016-01-07T13:11:00-08:00'
lastmod: '2016-01-07T13:11:00-08:00'
slug: find-the-nearest-upstream-genes-using-granges
tags:
- Bioconductor
- GRanges
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/01/find-nearest-upstream-genes-using.html
description: Make sure you read my note for the “+” and “-” strandness of bedpe for structural variants. In a word, + means the region is at the 5’ of the breakpoint and…
summary: Make sure you read my note for the “+” and “-” strandness of bedpe for structural variants. In a word, + means the region is at the 5’ of the breakpoint and…
header:
  caption: ''
  image: ''
---

## I am using `nearest` in the GRanges package to annotate my genomic intervals (breakpoints from whole genome sequencing). Those are not ChIP-seq peaks and the strandness for each interval is critical to me. I want to find the closest gene that is upstream of the breakpoint.

Make sure you read [my note](https://github.com/crazyhottommy/DNA-seq-analysis/blob/master/vcf_and_bedpe_sv_format.md) for the “+” and “-” strandness of bedpe for structural variants. In a word, `+` means the region is at the 5’ of the breakpoint and `-` means the region is at the 3’ of the breakpoint. In other words, I need to annotate the breakpoints with the closest genes that are UPSTREAM of the breakpoints. I want the nearest gene that is upstream of the breakpoint no matter what the strand of the gene is. However, when consider which gene is more closer to the breakpoint, the strandness of the gene needs to be considered.\
Note that, I am not saying the strandness of the genes (as mentioned here <http://ygc.name/2014/01/14/bug-of-r-package-chippeakanno/>), but rather the strandness of the intervals.\
It looks like `distanceToNearest()` always returns [an obsolute value](https://www.blogger.com/distance%20to%20TSS%20upstream/downstream) for distance no matter it is upstream or downstream of the subject hits. We can [get around with it](https://github.com/genomicsclass/labs/blob/master/bioc/grangesERBSExample.Rmd#finding-nearest-gene-for-each-binding-event).\
The bumphunter bioconductor package by Rafael A. Irizarry et.al has a function called [`annoateNearest`](http://finzi.psych.upenn.edu/library/bumphunter/html/annotateNearest.html) can better annotate the nearest distances considering the strandness, but it DID NOT restrict nearest to find only upstream or downstream relative to the query.\
Do not make me wrong, `bedtools` is a great tool and I use it everyday. I was trying to avoid using bedtools and put everything in R for the sake of reproducibility, although [bedtools closest](http://bedtools.readthedocs.org/en/latest/content/tools/closest.html) documentation has a much better explannation of all the cases I want to use.\
In a word, for GRanges, is it possible to find the upstream nearest or downstream nearest as `bedtools closest -D -id` and`bedtools closest -D -iu`?\
Let me make some toy examples and demonstrate the usage of `follow` and `precede`. Read the [post](https://support.bioconductor.org/p/76512/) and answers there as well.

``` text
             -----> start
```

————–\|————————\|——————— geneA in plus strand\
——-\|———————–\|—————————– geneB in minus strand

``` text
                 <-----start

                                         |-----|         breakpoint in *plus* strand
```

``` r
suppressMessages(library(GenomicRanges))
breakpoint<- GRanges(seqnames = "chr1", ranges=IRanges(start=8, width=2), strand = "+")

genes<- GRanges(seqnames = "chr1", ranges=IRanges(start=c(3,1,12,10), end=c(6,4,15,13)), strand = c("+", "-", "+","+"))
mcols(genes)<- c("geneA", "geneB", "geneC", "geneD")

breakpoint
```

``` text
## GRanges object with 1 range and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chr1    [8, 9]      +
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
genes
```

``` text
## GRanges object with 4 ranges and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1  [ 3,  6]      + |       geneA
##   [2]     chr1  [ 1,  4]      - |       geneB
##   [3]     chr1  [12, 15]      + |       geneC
##   [4]     chr1  [10, 13]      + |       geneD
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

I want to return geneB which is upstream of the breakpoint. if one uses follow:

``` r
follow(breakpoint, genes)
```

``` text
## [1] 1
```

``` r
genes[follow(breakpoint, genes)]
```

``` text
## GRanges object with 1 range and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1    [3, 6]      + |       geneA
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

It returns geneA. However, I want to return geneB. geneB is on the minus strand and the transcription start site is closer to the breakpoint.\
From the help page:

> follow: The opposite of precede, follow returns the index of the range in subject that is directly followed by the range in x. Overlapping ranges are excluded. NA is returned when there are no qualifying ranges in subject.

> Orientation and Strand: The relevant orientation for precede and follow is 5’ to 3’, consistent with the direction of translation. Because positional numbering along a chromosome is from left to right and transcription takes place from 5’ to 3’, precede and follow can appear to have ‘opposite’ behavior on the + and - strand. Using positions 5 and 6 as an example, 5 precedes 6 on the + strand but follows 6 on the - strand.

In my case, follow means that the gene is directly followed by the breakpoint considering the strandness of the gene. Because geneA is on plus strand, and the breakpoint is on plus strand, `follow(breakpoint, genes)` will return geneA, not geneB.\
To get geneB, I have to get around by first resize the gene to size 1 (the transcrtiption start site) and then unstrand it.\
on the help page:

> A range with strand \* can be compared to ranges on either the + or - strand. Below we outline the priority when ranges on multiple strands are compared. When ignore.strand=TRUE all ranges are treated as if on the + strand.

> x on + strand can match to ranges on both + and \* strands. In the case of a tie the first range by order is chosen.

> x on - strand can match to ranges on both - and \* strands. In the case of a tie the first range by order is chosen.

> x on \* strand can match to ranges on any of +, - or \* strands. In the case of a tie the first range by order is chosen.

``` r
follow(breakpoint, unstrand(resize(genes,width=1)))
```

``` text
## [1] 2
```

``` r
genes[follow(breakpoint, unstrand(resize(genes,width=1)))]
```

``` text
## GRanges object with 1 range and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1    [1, 4]      - |       geneB
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

### Now, let’s change the breakpoint to minus strand:

``` text
           |-----|                              breakpoint in *minus* strand
```

———————————-\|———\|—– geneC in plus strand\
——————————-\|———–\|—— geneD in plus strand\
In this case, I want to return gene D. If one uses follow, it will return NA:

``` r
breakpoint1<- GRanges(seqnames = "chr1", ranges=IRanges(start=8, width=2), strand = "-")
follow(breakpoint1, genes)
```

``` text
## [1] NA
```

Because, follow requires that the genes are on the same strand with the breakpoint and the breakpoint follows genes or the genes are followed by the breakpoint. The breakpoint is on the minus strand while geneC and geneD are both on plus strand.\
Instead, try

``` r
follow(breakpoint1, unstrand(resize(genes,width=1)))
```

``` text
## [1] 4
```

``` r
genes[follow(breakpoint1, unstrand(resize(genes,width=1)))]
```

``` text
## GRanges object with 1 range and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1  [10, 13]      + |       geneD
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

### Let’s change geneD to minus strand:

``` text
           |-----|                              breakpoint in *minus* strand
```

———————————-\|———\|—– geneC in plus strand\
——————————-\|———–\|—— geneD in minus strand\
In this case, I want to return gene C, because geneC is on the plus strand, and its transcription start site is closer to the breakpoint.

``` r
genes1<- genes<- GRanges(seqnames = "chr1", ranges=IRanges(start=c(3,1,12,10), end=c(6,4,15,13)), strand = c("+", "-", "+","-"))

mcols(genes1)<- c("geneA", "geneB", "geneC", "geneD")
genes1[follow(breakpoint1, genes1)]
```

``` text
## GRanges object with 1 range and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1  [10, 13]      - |       geneD
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
genes1[follow(breakpoint1, unstrand(genes1))]
```

``` text
## GRanges object with 1 range and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1  [10, 13]      - |       geneD
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

It returns geneD again, but I want to return gene C.

``` r
genes1[follow(breakpoint1, unstrand(resize(genes1, width=1)))]
```

``` text
## GRanges object with 1 range and 1 metadata column:
##       seqnames    ranges strand |       value
##          <Rle> <IRanges>  <Rle> | <character>
##   [1]     chr1  [12, 15]      + |       geneC
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
upstreamGenes<- genes1[follow(breakpoint1, unstrand(resize(genes1, width=1)))]

## distance requires that query and subject on the same strand.
## Note distance calculation changed in BioC 2.12 to accommodate zero-width ranges in a consistent and intuitive manner

distance(breakpoint1, unstrand(upstreamGenes))
```

``` text
## [1] 2
```

### Conclusions

It seems that using `follow(breakpoing, unstrand(resize(genes, width=1)))` serves my purpose very well. However, be careful that some breakpoint may not have a upstream gene and it will return NA using `follow`. GRanges object can not be subscripted by NAs.
