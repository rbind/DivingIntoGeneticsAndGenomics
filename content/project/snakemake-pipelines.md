+++
# Date this page was created.
date = 2016-04-27T00:00:00

# Project title.
title = "Snakemake Pipelines"

# Project summary to display on homepage.
summary = "Snakemake pipelines for processing high-throughput sequencing data"

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "snakemake.png"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["genomics", "pipeline", "snakemake"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = "headers/snakemake.png"
caption = "My caption :smile:"

+++

Snakemake is a python extension for writing workflows. Genomics data processing usually requires bundling many different tools to reach a stage that is ready for downstream analysis.

I have been using snakemake writing workflows for various genomic and epigenomic datasets.

* [ChIP-seq](https://github.com/crazyhottommy/pyflow-ChIPseq)

* [DNA-seq](https://gitlab.com/tangming2005/snakemake_DNAseq_pipeline/tree/multiRG)

* [RNA-seq](https://github.com/crazyhottommy/pyflow-RNAseq)

* [ATAC-seq](https://github.com/crazyhottommy/pyflow-ATACseq)

* [RRBSeq](https://gitlab.com/tangming2005/pyflow-RRBSeq)

The DNAseq Snakemake pipeline is the most complicated workflow I have written so far. I will need to think about refining it, modulizing it and adding conda env support. Eventually, I will need to put all the snakemake workflows togther using something like [snakeparse](https://github.com/nh13/snakeparse)

![](/img/rule_graph_lancet.png)

