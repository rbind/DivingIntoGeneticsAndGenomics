---
title: Breakpoints clustering for structural variants
author: Ming Tang
date: '2016-03-03T20:53:00-08:00'
lastmod: '2016-03-03T20:53:00-08:00'
slug: breakpoints-clustering-for-structural-variants
tags:
- Genomic Interactions
- GRanges
- R
- structural variants
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/03/breakpoints-clustering-for-structural.html
description: When structural variants are represented in bedpe format, each structural variant is represented as two linked breakpoints. Each breakpoint is represented…
summary: When structural variants are represented in bedpe format, each structural variant is represented as two linked breakpoints. Each breakpoint is represented…
header:
  caption: ''
  image: ''
---

#### *Ming Tang*

#### *March 2, 2016*

When structural variants are represented in `bedpe` format, each structural variant is represented as two linked breakpoints. Each breakpoint is represented as a genomic interval (GRanges). It is very similar to representations of Genomic interactions (data from Hi-C, ChIA-PET):\
e.g.\
`chr1 100 200 chr2 300 400`\
one of the common task is to merge the breakpoints when they are overlapping.\
———\|———\|———————–\|———-\|—–\
—–\|———\|————————-\|—————\|—\
merged to\
—–\|————-\|———————\|—————\|—\
It is not a trivial problem as stated by Aaron Qunlan in this [post](https://groups.google.com/forum/#!topic/bedtools-discuss/JXZbJSwVxUo)\
It is the so called “breakpoint clustering” problem. I am going to use `InteractionSet` bioconductor package to solve this problem.\
Please also check `clusterPairs` function in the [diffHic](https://www.bioconductor.org/packages/release/bioc/html/diffHic.html) package.\
see [answer](https://support.bioconductor.org/p/78082/#79139) from the author of `InteractionSet`. Note that `InteractionSet` is still under development.  Please refer to the tutorial for more [usages](https://github.com/LTLA/InteractionSet/blob/master/vignettes/interactions.Rmd). Install it using devtools:`install_github("LTLA/InteractionSet").`\
Make some dummy GRanges.

``` r
library(InteractionSet)
all.regions <- GRanges(rep("chrA",8),
    IRanges(c(1,6,2,9,5,2,15,20), c(3,10,4,12,7,4,18,23)))
index.1 <- c(1,3,5,7)
index.2 <- c(2,4,6,8)
```

Using `mode`=`strict` when constructing the GInteraction object or using the `swapAnchors` method is to ensure that the first anchor index is always less than the second anchor index for each interaction. This eliminates redundant permutations of anchor regions and ensures that an interaction between regions \#1 and \#2 is treated the same as an interaction between regions \#2 and \#1. Obviously, this assumes that redundant permutations are uninteresting.

``` r
gi <- GInteractions(index.1, index.2, all.regions, mode ="strict")

gi
```

``` text
## StrictGInteractions object with 4 interactions and 0 metadata columns:
##       seqnames1   ranges1     seqnames2   ranges2
##           <Rle> <IRanges>         <Rle> <IRanges>
##   [1]      chrA  [ 1,  3] ---      chrA  [ 6, 10]
##   [2]      chrA  [ 2,  4] ---      chrA  [ 9, 12]
##   [3]      chrA  [ 2,  4] ---      chrA  [ 5,  7]
##   [4]      chrA  [15, 18] ---      chrA  [20, 23]
##   -------
##   regions: 8 ranges and 0 metadata columns
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

The first three pairs of interactions can be merged. Note that two anchors of each pair are overlapping. First, use `findOverlaps` for GInteraction object to find out which pairs with two anchors are overlapping. (2D overlapping)

``` r
out<- findOverlaps(gi)

out
```

``` text
## Hits object with 8 hits and 0 metadata columns:
##       queryHits subjectHits
##       <integer>   <integer>
##   [1]         1           1
##   [2]         1           2
##   [3]         1           3
##   [4]         2           1
##   [5]         2           2
##   [6]         3           1
##   [7]         3           3
##   [8]         4           4
##   -------
##   queryLength: 4 / subjectLength: 4
```

This will identify all pairs of interactions in gi that have two-dimensional overlaps with each other, i.e., both anchor regions in one interaction overlap with corresponding anchor regions in the other interaction. Using graph algorithm to cluster the anchors (I myself is by no means an algorithm person :)). We then do:

``` r
library(RBGL)

## need to trick the function by specifying edgemode to be directional
g <- ftM2graphNEL(as.matrix(out), W=NULL, V=NULL, edgemode="directed")

## if you want to see what's going on
library(Rgraphviz)
plot(g)
```

![](/img/archive/breakpoints-clustering-for-structural-variants/01-inline.png)

``` r
## change it back to undirected
edgemode(g) <- "undirected"

plot(g)
```

![](/img/archive/breakpoints-clustering-for-structural-variants/02-inline.png)

``` r
connections <- connectedComp(g)
```

To identify groups of interactions that overlap at least one other interaction in the same group (i.e., “single-linkage clusters” of interactions that have overlapping areas in the two-dimensional interaction space). The clusters can be explicitly identified using:

``` r
cluster <- integer(length(gi))
for (i in seq_along(connections)) {
    cluster[as.integer(connections[[i]])] <- i
}
```

We can then identify the bounding box (merge the overlapping interactions) for each cluster of interactions with:

``` r
boundingBox(gi, cluster)
```

``` text
## GInteractions object with 2 interactions and 0 metadata columns:
##     seqnames1   ranges1     seqnames2   ranges2
##         <Rle> <IRanges>         <Rle> <IRanges>
##   1      chrA  [ 1,  4] ---      chrA  [ 5, 12]
##   2      chrA  [15, 18] ---      chrA  [20, 23]
##   -------
##   regions: 4 ranges and 0 metadata columns
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

### overlap with one anchor and get the other anchor

I have `of.interest` overlaps with the `second` anchor, and I want to return the `first` anchor. how can I do it?\
e.g.

``` text
    first (promoters)                      second
```

——\|————-\|———————-\|————\|——– gi

``` text
                             |------------|        of.interest (enhancers)
```

``` r
of.interest <- GRanges(c("chrA","chrA"), IRanges(start=c(2, 5),end=c(8,8)))

# get overlaps between of.interest and first anchor
hits1 <- findOverlaps(gi, of.interest, use.region="first")
anchors(gi[queryHits(hits1),], type="first") # overlapping
```

``` text
## GRanges object with 3 ranges and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chrA    [1, 3]      *
##   [2]     chrA    [2, 4]      *
##   [3]     chrA    [2, 4]      *
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
anchors(gi[queryHits(hits1),], type="second") # "other"
```

``` text
## GRanges object with 3 ranges and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chrA   [6, 10]      *
##   [2]     chrA   [9, 12]      *
##   [3]     chrA   [5,  7]      *
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
of.interest[subjectHits(hits1)]
```

``` text
## GRanges object with 3 ranges and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chrA    [2, 8]      *
##   [2]     chrA    [2, 8]      *
##   [3]     chrA    [2, 8]      *
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
# get overlaps # between of.interest and second anchor
hits2 <- findOverlaps(gi, of.interest, use.region="second")
anchors(gi[queryHits(hits2),], type="second") # overlapping
```

``` text
## GRanges object with 4 ranges and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chrA   [6, 10]      *
##   [2]     chrA   [6, 10]      *
##   [3]     chrA   [5,  7]      *
##   [4]     chrA   [5,  7]      *
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
anchors(gi[queryHits(hits2),], type="first") # "other"
```

``` text
## GRanges object with 4 ranges and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chrA    [1, 3]      *
##   [2]     chrA    [1, 3]      *
##   [3]     chrA    [2, 4]      *
##   [4]     chrA    [2, 4]      *
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
of.interest[subjectHits(hits2)]
```

``` text
## GRanges object with 4 ranges and 0 metadata columns:
##       seqnames    ranges strand
##          <Rle> <IRanges>  <Rle>
##   [1]     chrA    [2, 8]      *
##   [2]     chrA    [5, 8]      *
##   [3]     chrA    [2, 8]      *
##   [4]     chrA    [5, 8]      *
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

If you’re willing to sacrifice some information, you might consider using the linearize method:

``` r
gi$index <- seq_along(gi)
linearize(gi, of.interest[1])
```

``` text
## GRanges object with 3 ranges and 1 metadata column:
##       seqnames    ranges strand |     index
##          <Rle> <IRanges>  <Rle> | <integer>
##   [1]     chrA   [1, 10]      * |         1
##   [2]     chrA   [9, 12]      * |         2
##   [3]     chrA   [2,  7]      * |         3
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

``` r
linearize(gi, of.interest[2])
```

``` text
## GRanges object with 2 ranges and 1 metadata column:
##       seqnames    ranges strand |     index
##          <Rle> <IRanges>  <Rle> | <integer>
##   [1]     chrA    [1, 3]      * |         1
##   [2]     chrA    [2, 4]      * |         3
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

This will return a GRanges containing the “other” anchor region for all interactions that overlap each entry in of.interest (you can figure out what those interactions were by looking at index in the returned object). linearize also provides options for handling “internal” interactions where both anchor regions overlap the specified entry of of.interest; by default, the union of the two anchor regions is returned for such interactions, but they can be removed entirely by setting internal=FALSE.

### Alternatively, if you already have two sets of regions(promoters, enhancers) you want to know which enhancer links to which promoter

``` r
all.enhancers<- GRanges(c("chrA","chrA"), IRanges(c(2,17), c(5,19)))
all.genes<- GRanges(c("chrA","chrA"), IRanges(c(4,6), c(7,16)))

linkOverlaps(gi, all.genes, all.enhancers)
```

``` text
##   query subject1 subject2
## 1     1        1        1
## 2     1        2        1
## 3     2        2        1
## 4     3        1        1
## 5     3        2        1
```

which two genes are linked?

``` r
linkOverlaps(gi, all.genes)
```

``` text
##   query subject1 subject2
## 1     2        2        1
## 2     3        1        1
## 3     3        2        1
```

which two enhancers are linked?

``` r
linkOverlaps(gi, all.enhancers)
```

``` text
##   query subject1 subject2
## 1     3        1        1
```
