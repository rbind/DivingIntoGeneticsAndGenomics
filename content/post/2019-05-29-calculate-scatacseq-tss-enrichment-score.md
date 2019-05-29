---
title: Calculate scATACseq TSS enrichment score
author: Ming Tang
date: '2019-05-29'
slug: calculate-scatacseq-tss-enrichment-score
categories:
  - bioinformatics
  - genomics
  - scATACseq
  - single-cell
tags:
  - ATACseq
  - genomics
  - GRanges
header:
  caption: ''
  image: ''
---

[TSS enrichment score](https://www.encodeproject.org/data-standards/terms/#enrichment) serves as an important quality control metric for ATACseq data. I want to write a script for single cell ATACseq data.

From the Encode page:

>Transcription Start Site (TSS) Enrichment Score - The TSS enrichment calculation is a signal to noise calculation. The reads around a reference set of TSSs are collected to form an aggregate distribution of reads centered on the TSSs and extending to 1000 bp in either direction (for a total of 2000bp). This distribution is then normalized by taking the average read depth in the 100 bps at each of the end flanks of the distribution (for a total of 200bp of averaged data) and calculating a fold change at each position over that average read depth. This means that the flanks should start at 1, and if there is high read signal at transcription start sites (highly open regions of the genome) there should be an increase in signal up to a peak in the middle. We take the signal value at the center of the distribution after this normalization as our TSS enrichment metric. Used to evaluate ATAC-seq. 

It was not so clear to me from the definition on how **EXACTLY** this score is calculated.

I inspected the [source code](https://github.com/jianhong/ATACseqQC/blob/master/R/TSSEscore.R#L80) of  `ATACseqQC` which calculates the TSS enrichment score for bulk ATACseq data, but I think it is not calculating it the right way as described by the ENCODE page.

I reached out to [Ansu Satpathy](https://twitter.com/Satpathology) (thanks!), and got a script written by Jeffrey Granja, who are the authors of this recent scATACseq paper:
[Massively parallel single-cell chromatin landscapes of human immune cell development and intratumoral T cell exhaustion (2019)](https://www.biorxiv.org/content/10.1101/610550v1)

I studied the script and also got the confirmation from ENCODE how they calculate the TSS enrichment score
https://github.com/ENCODE-DCC/atac-seq-pipeline/issues/50 by a python script.

To work with this coverage type of data in R, I want to take advantage of the data structure `View` in bioconductor, so I borrowed some codes from [`genomation::ScoreMatrix`](https://bioconductor.org/packages/release/bioc/vignettes/genomation/inst/doc/GenomationManual.html) instead of using the script sent by Ansu. It is a very nice package by [Altuna Akalin](https://twitter.com/AltunaAkalin). A side note, he has a very nice book you might be interested in: [Computational Genomics with R](http://compgenomr.github.io/book/how-to-contribute.html)

Below, I ended up with a hybrid script from multiple sources. 
Now it works with the 10x cellranger-atac output `fragment.tsv.gz`. One can tweak it to work with the bam file. However, the bam file is 25G, R takes a long time to parse it.

I explain what the script does:

1. for each TSS, get per base coverage for the 1000 bp flanking region(flank = 1000).
2. do this for all TSSs, We get a matrix of #TSS x 2000 bp dimension.
3. do a column sum of the matrix.
4. sum of the coverage of the endFlank (100bp) at both ends and divide by 200 bp to get a 
normalization factor.
5. divide the the normalization factor for -1900 to + 1900 bp to get per base normalized coverage.
6. do a smoothing with a defined window (50bp by default) using `zoo::rollmean`.
7. select the highest value within a window (highest_tss_flank, 50 bp by default) around the TSS because the highest peak is not necessary at exactly the TSS site (position 0)
8. repeat 1-7 for all cells.

### Extra technical notes:

* One thing to note is that one needs to filter out the TSSs which are not within the coverage. e.g. A TSS with 1000 bp flanking regions fall out of the coverage. 

* use only the common chromosomes between coverage and the txs.

* convert GRanges to IntergerRangesList does not maintain the order of the GRanges.
so a unique id was given for each Ranges, and the matrix can be reordered according to this unique id. That's what `constrainRanges()` does. read this thread for more https://stat.ethz.ch/pipermail/bioc-devel/2016-June/009433.html

### How long it takes.

It took me around ~15 seconds to calculate the TSS enrichment score for a single cell. 
1.213291 hours for 5000 PBMC cells using 15 workers (not too bad :).

### R code

```r
library(GenomicRanges)
library(dplyr)

#' checkClass function
#' 
#' check whether the x object corresponds to the given class
#'
#' @param x object
#' @param class.name class name
#' @param var.name uses x object
#' @keywords internal
checkClass = function(x, class.name, var.name = deparse(substitute(x))){
  
  fun.name = match.call(call=sys.call(sys.parent(n=1)))[[1]]
  if(!class(x) %in% class.name)
    stop(paste(fun.name,': ', 
               var.name, 
               ' is not of class: ', 
               paste(class.name, collapse=' '), 
               '\n', sep=''))
}

### remove the tss that do not have coverage
### I took some code from the ScoreMatrix.R function in the genomation package.
### give the credit due :)
### see https://github.com/BIMSBbioinfo/genomation/blob/master/R/scoreMatrix.R#L113
constrainRanges = function(target, windows){
  
  checkClass(target, c('SimpleRleList','RleList','CompressedRleList'))
  checkClass(windows, 'GRanges')
  
  mcols(windows)$X_rank = 1:length(windows)
  r.chr.len = elementNROWS(target)
  constraint = GRanges(seqnames=names(r.chr.len),
                       IRanges(start=rep(1,length(r.chr.len)),
                               end=as.numeric(r.chr.len)))
  # suppressWarnings is done becuause GenomicRanges function give warnings 
  #if you don't have the same seqnames in both objects
  win.list.chr = suppressWarnings(subsetByOverlaps(windows, 
                                                   constraint,
                                                   type = "within",
                                                   ignore.strand = TRUE))
  
  if(length(win.list.chr) == 0)
    stop('All windows fell have coordinates outside windows boundaries')
  return(win.list.chr)
}



#' Calculate tss enrichment score from 10xscATAC fragment file
#'
#' @param frag_gz_file  fragment.tsv.gz file from 10x cellranger-atac output or 
#' anyother tool but in the same format.
#' @param txs  a txdb object
#' @param flank flanking bp of tss (upstream and downstream)
#' @param endFlank  bp end flanks of flank for local noise control
#'     flank               flank
#'  ---------------|-----------------
#'                tss
#'  ---                           ---
#'  endFlank                     endFlank
#'  
#' @param highest_tss_flank bp flanking tss windown for choosing the highest tss score.
#' The highest tss enrichment score is not always exactly at tss.
#' @param barcodeList valid barcode list, a file with one column 
#' @param smooth window size to smooth 
#' @param strand.aware consider tss strandness when calculating 
#'
#' @return
#' @export
#'
#' @examples
#' library(TxDb.Hsapiens.UCSC.hg19.knownGene)
#' library(dplyr); library(readr); library(BiocParallel)
#' txs <- transcripts(TxDb.Hsapiens.UCSC.hg19.knownGene)
#' scores<- TssEnrichmentFromFrags("fragment.tsv.gz", txs = txs)

TssEnrichmentFromFrags <- function(frag_gz_file,
                               txs,
                               flank = 1000,
                               endFlank = 100,
                               highest_tss_flank= 50,
                               smooth = 50,
                               strand.aware = TRUE,
                               workers = 1,
                               barcodeList = NULL){
        
        # Make GRanges of fragments that are solid for the cells that we care about
        frags_valid <- data.table::fread(paste0("zcat < ", frag_gz_file)) %>% 
                data.frame() %>% 
                mutate(V2 = V2 + 1) %>% # make it 1 based for R
                GenomicRanges::makeGRangesFromDataFrame(seqnames.field = "V1", start.field = "V2", end.field = "V3", keep.extra.columns = TRUE)
        if (!is.null(barcodeList)){
                validBarcodes<- read_tsv(barcodeList, col_names = F)
                frags_valid<- frags_valid[frags_valid$V4 %in% validBarcodes$X1]
        }
        
        # calculate coverage per cell
        frags_valid_per_cell<- split(frags_valid, frags_valid$V4)
        
       
        # this step can take minutes 
        multicoreParam <- BiocParallel::MulticoreParam(workers = workers)
        # can add the chromosome length as additional argument for `coverage`
        # to get 0 coverages if there are no reads there. 
        cvgs<- bplapply(frags_valid_per_cell, function(x) coverage(x), BPPARAM = multicoreParam)
        
        ## return only transcripts with a coverage 
        txs<- keepSeqlevels(txs, seqlev, pruning.mode="coarse")
        txs <- unique(txs)
        
        txs.flanks<- promoters(txs, upstream = flank, 
                            downstream = flank)
        txs.length<- length(txs.flanks)
        
        TssEnrichmentScores<- BiocParallel::bplapply(cvgs, TssEnrichmentSingleCell, txs.flanks, BPPARAM = multicoreParam)

        enrichment<- do.call("rbind", TssEnrichmentScores)
        return(enrichment)
}    

TssEnrichmentSingleCell<- function(cvg, txs.flanks){
        ## remove tss not in the coverage and assign a unique id for each tss: X_rank
        txs.flanks<- constrainRanges(cvg, txs.flanks)
        
        if(length(txs.flanks)!=txs.length){
              warning(paste0(txs.length-length(txs.flanks),
                             " Tss removed because they fall out of the coverage"))
            }
        # common chromosomes
        chrs<- sort(intersect(names(cvg), as.character(unique(seqnames(txs.flanks)))))
        
        # convert GRanges to IntergerRangesList does not maintain the order
        # a unique id was given for each Ranges
        myViews<- Views(cvg[chrs],as(txs.flanks,"IntegerRangesList")[chrs]) # get subsets of RleList
        mat = lapply(myViews,function(x) t(viewApply(x,as.vector)) )
        mat = do.call("rbind",mat)
        
        r.list=split(mcols(txs.flanks)[,"X_rank"], as.vector(seqnames(txs.flanks))  )
        r.list=r.list[order(names(r.list))]
        ranks=do.call("c",r.list)
        rownames(mat) = ranks
        
        if(strand.aware == TRUE){
              orig.rows=txs.flanks[strand(txs.flanks) == '-',]$X_rank
              mat[rownames(mat) %in% orig.rows,] = mat[rownames(mat) %in% 
                                                         orig.rows, ncol(mat):1]
        }
        
        # reorder according to the original Granges (txs)
        mat = mat[order(ranks),]
        
  
        ### normlization by the endFlank local noise
        profile <- colSums(mat)
        profile_norm <- profile/mean(profile[c(1:endFlank,(flank*2-endFlank+1):(flank*2))])

        #smooth
        profile_norm_smooth <- zoo::rollmean(profile_norm, smooth, fill = 1)
        

        #enrichment
        max_finite <- function(x){
        suppressWarnings(max(x[is.finite(x)], na.rm=TRUE))
        }
        
        e <- max_finite(profile_norm_smooth[(flank-highest_tss_flank):(flank+highest_tss_flank)])
        return(e)
}


```