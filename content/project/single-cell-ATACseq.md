+++
# Date this page was created.
date = 2019-07-17T00:00:00

# Project title.
title = "Snakemake pipeline post-processing scATAC-seq"

# Project summary to display on homepage.
summary = "A snakemake pipeline to split scATACseq bam by cluster, make bigwig tracks, call peaks and recount"

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "headers/ATAC.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["python", "scATACseq", "snakemake"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = "headers/bubbles-wide.jpg"
caption = "My caption :smile:"

+++

### What does it do?

For single cell ATACseq experiment, one gets a merged bam file for all cells. After going through clustering, one groups similar cells into cell types (cell states). This workflow will split the bam by clusters to create a pseudo bulk bam for each cluster, create bigwig tracks for visulization, call peaks for each cluster and merge the peaks across the clusters. Finally it will count reads per peak per cell from the original bam file on the merged peaks.

In the future, the peak calling software should be barcode aware, so one does not need to split the bam file by cluster. But for now, I have this work for me.

You can find the workflow at my [github](https://github.com/crazyhottommy/pyflow-scATACseq).

![](/img/rulegraph_scATAC.png)

