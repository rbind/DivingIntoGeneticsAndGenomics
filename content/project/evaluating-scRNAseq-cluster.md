+++
# Date this page was created.
date = 2019-02-05T00:00:00

# Project title.
title = "Evaluating single cell RNAseq cluster stability"

# Project summary to display on homepage.
summary = "An R package for evaluating and visualizing scRNAseq cluster stability"

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "scclusteval.png"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["R", "scRNAseq", "cluster"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = "headers/scclusteval.png"
caption = "My caption :smile:"

+++

The goal of scclusteval(Single Cell Cluster Evaluation) is to evaluate the single cell clustering stability by boostrapping/subsampling the cells and provide many visualization methods for comparing clusters.

for Theory behind the method, see Christian Henning, “Cluster-wise assessment of cluster stability,” Research Report 271, Dept. of Statistical Science, University College London, December 2006)

You can find the package at my [github](https://github.com/crazyhottommy/scclusteval).

![](/img/raincloud_cluster.png)

