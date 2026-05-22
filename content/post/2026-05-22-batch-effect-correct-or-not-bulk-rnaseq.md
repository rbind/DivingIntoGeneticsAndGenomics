---
title: "Batch Effect: To Correct or Not for Bulk RNA-seq Data"
description: "When should you use limma::removeBatchEffect() on bulk RNA-seq data? A practical guide with R code, PCA examples, and the visualization vs differential expression distinction every bioinformatician should know."
author: Ming Tang
date: '2026-05-22'
slug: batch-effect-correct-or-not-bulk-rnaseq
categories:
  - bioinformatics
  - R
tags:
  - RNA-seq
  - batch-effect
  - limma
  - PCA
  - bulk-rnaseq
  - differential-expression
  - tutorial
  - R
math: false
header:
  caption: ''
  image: ''
---

A colleague sent me a PCA plot last week. Three replicates per condition, control vs KO of a gene. PC1 cleanly separated replicate 1 from replicate 2 from replicate 3. PC2 separated control vs KO. The question that followed: should we run `limma::removeBatchEffect()` before differential expression?

The answer is "it depends," and most of the time the answer is "no, do not pre-correct the matrix before DE testing." But the question hides a few subtleties worth unpacking, because I have seen people both under-correct (ignore a real block effect) and over-correct (treat true biological variation as batch and squash it).

This post walks through:

- what `removeBatchEffect()` is actually doing under the hood
- when to use it for visualization vs differential expression
- whether "replicate 1, 2, 3" in your design is a batch variable at all
- the right way to set up the design matrix
- a few common ways people get burned

## What limma::removeBatchEffect() does

`removeBatchEffect()` is a data-adjustment function. For each gene, it fits a linear model along these lines:

```
expression ~ biological_design + batch
```

It estimates the contribution of the batch variable and subtracts it from the expression values, returning a corrected matrix. The function tries to preserve the biological signal you care about by including the design of the biology in the `design` argument.

So if your samples differ because of:

- condition: control vs KO_gene1
- replicate or run or batch: 1, 2, 3

then `removeBatchEffect()` removes the part of variation explained by replicate/batch, keeping the part explained by condition. If you do not tell it what biological effect to keep, it may strip out part of the biological signal whenever batch and biology are not perfectly orthogonal.

## Visualization vs differential expression: the key distinction

This is the part people miss.

- For **PCA, heatmaps, clustering**: a batch-corrected matrix is often useful. You want to see whether the biological signal is there once the dominant nuisance variation is taken out.
- For **differential expression**: you usually should not pre-correct the matrix and then test on the corrected matrix. Instead, include the batch term directly in the design matrix of the linear model.

Why? When you pre-correct and then run `limma`, the residual degrees of freedom and standard errors that `limma` computes are not quite right. The function does not know that you have already removed variation, so the variance estimates can be too small and the p-values too optimistic. Including batch in the design lets `limma` account for it properly while still using all the data.

Short version:

- PCA: corrected matrix is fine.
- DE: model batch in `lmFit()`, do not pre-correct.

## Are your "replicate 1, 2, 3" actually batches?

This is the question that matters most for your specific case.

It depends entirely on what "replicate number" means in your experiment.

If replicate 1, 2, 3 means:

- samples processed on three different days
- three library prep batches
- three sequencing lanes or runs
- paired samples where replicate number corresponds to a shared technical handling event

then treating that as a batch effect is reasonable.

If replicate 1, 2, 3 means:

- three independent biological replicates with no shared technical handling

then they are not a batch variable, and removing them is usually not appropriate. Biological replicate-to-replicate variation is part of the real data structure. If you remove it, you may distort the data and overstate the clarity of the condition effect.

The PCA pattern alone does not tell you which case you are in. You need to know the experimental design.

## When the block structure is real

The case where `removeBatchEffect()` is most clearly appropriate is when replicate number reflects a paired block. For example:

- Control_1 and KO_1 were processed together on day 1
- Control_2 and KO_2 were processed together on day 2
- Control_3 and KO_3 were processed together on day 3

Replicate number here is shared across conditions and reflects a real source of technical variation (day of processing, plate, mouse litter, sequencing run). In that case, PC1 separating by replicate is a signal that the block effect dominates, and removing it for visualization is sensible.

If the design looks like that, here is the correct usage:

```r
library(limma)

batch <- factor(c(1, 2, 3, 1, 2, 3))
group <- factor(c("control", "control", "control",
                  "KO_gene1", "KO_gene1", "KO_gene1"))

design <- model.matrix(~ group)

expr_corrected <- removeBatchEffect(expr, batch = batch, design = design)
```

The `design` argument tells `limma` to remove variation associated with `batch` while keeping variation associated with `group`. Skip the `design` argument and you risk pulling out part of the biological signal too.

## Sample coding is where people get burned

A common mistake is to set up `batch` and `group` factors that do not actually match the sample order in the expression matrix. The function will run, the PCA will look "better," and you will have no idea you made a mistake.

With 3 controls followed by 3 KOs, the typical setup looks like:

```r
group <- factor(c("control", "control", "control",
                  "KO_gene1", "KO_gene1", "KO_gene1"))
batch <- factor(c(1, 2, 3, 1, 2, 3))
```

This works only if:

- control samples are replicates 1, 2, 3 in that order
- KO samples are replicates 1, 2, 3 in that order
- replicate 1 across both conditions actually shares the underlying block (same day, same plate, same run, whatever)

If your sample sheet has replicate numbers that do not correspond across conditions (replicate 1 of control was processed with replicate 2 of KO, etc), the `batch` factor will be wrong and the correction will not match the real block structure. Always read your sample metadata and confirm. Always.

## What removeBatchEffect() does not do

A few things this function is **not** doing:

- It is not performing differential expression testing.
- It is not guaranteeing that the removed variation is purely technical. It removes whatever variation is associated with the batch variable, including any biology that happens to correlate with batch.
- It cannot fix confounding. If all of your controls were processed in batch 1 and all KOs in batch 2, then batch and condition are perfectly confounded, and no statistical method can untangle them. You would need a redesigned experiment.

The "garbage in garbage out" rule applies. If batch is set up wrong or confounded with biology, the correction will mislead you.

## How to interpret your PCA result

Your PCA shows PC1 = replicate, PC2 = condition. That could mean any of three things:

1. There is a strong paired/blocking effect and the condition signal is real but smaller. After correction the condition separation should become clearer on PC1.
2. There is substantial biological heterogeneity among replicates and the "replicate" label is capturing real biology (different individuals, different time points, etc).
3. Something technical is dominating: a single batch was contaminated, a sample was mislabeled, library quality varies wildly across replicates.

The right interpretation depends on the experimental design and on basic QC. Before reaching for `removeBatchEffect()`, look at:

- per-sample read depth and duplication rate
- library size and complexity
- sample swap checks (e.g., genotype concordance across replicates if you have any genetic markers)
- whether the "replicates" actually share technical handling

If after all that the block structure is real and shared across conditions, correcting for visualization is fine. Otherwise be cautious.

## The right way to run DE with a batch term

For differential expression with `limma` or `limma-voom`, model both effects in the design matrix:

```r
library(limma)
library(edgeR)

group <- factor(c("control", "control", "control",
                  "KO_gene1", "KO_gene1", "KO_gene1"))
replicate <- factor(c(1, 2, 3, 1, 2, 3))

# for RNA-seq counts, run voom first
dge <- DGEList(counts = counts)
dge <- calcNormFactors(dge)

design <- model.matrix(~ replicate + group)
v <- voom(dge, design)

fit <- lmFit(v, design)
fit <- eBayes(fit)

# test the KO vs control coefficient
results <- topTable(fit, coef = "groupKO_gene1", number = Inf)
```

The `~ replicate + group` formula tells `limma` to estimate condition effects while accounting for replicate. This uses all your data properly and gives you correct standard errors. It is almost always better than `removeBatchEffect()` followed by a naive DE run on the corrected matrix.

The same logic applies if you are using `DESeq2`: put the batch/block term in the design formula (`design = ~ replicate + condition`), do not pre-correct the count matrix.

## Rule of thumb

The decision tree I use:

- **PCA or heatmap visualization**: use `removeBatchEffect()` if batch is known, distinct from the contrast of interest, and you have a proper design matrix protecting the biology.
- **Differential expression**: include the batch term in the design matrix instead of pre-correcting. Works for both `limma` and `DESeq2`.
- **Treating biological replicates as batch**: usually no. That is just removing real variation.
- **Treating paired or block structure as batch**: usually yes. If replicate label shares a real technical event across conditions, it is a legitimate block variable.

The honest answer to "should I correct?" is almost always "tell me first what your replicate label means, then we can talk." If you do not know the experimental design well enough to answer that, your first job is not the analysis. It is to call the wet-lab person and find out.

What batch effect gotchas have you hit on bulk RNA-seq? Let me know on [X](https://x.com/tangming2005) or [LinkedIn](https://www.linkedin.com/in/ming-tommy-tang-40650014/), or leave a comment below.
