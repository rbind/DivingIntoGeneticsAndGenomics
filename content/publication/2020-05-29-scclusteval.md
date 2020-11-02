+++
title = "Evaluating single-cell cluster stability using the Jaccard similarity index"
date = 2020-05-29T00:00:00

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["**Ming Tang** et.al"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference proceedings
# 2 = Journal
# 3 = Work in progress
# 4 = Technical report
# 5 = Book
# 6 = Book chapter
publication_types = ["2"]

# Publication name and optional abbreviated version.
publication = "In *Bioinformatics In Press*, Bioinformatics."
publication_short = "In *Bioinformatics*"

# Abstract and optional shortened version.
abstract = "Motivation: One major goal of single-cell RNA sequencing (scRNAseq) experiments is to identify novel cell types. With increasingly large scRNAseq datasets, unsupervised clustering methods can now produce detailed catalogues of transcriptionally distinct groups of cells in a sample. However, the interpretation of these clusters is challenging for both technical and biological reasons. Popular clustering algorithms are sensitive to parameter choices, and can produce different clustering solutions with even small changes in the number of principal components used, the k nearest neighbor, and the resolution parameters, among others. Results: Here, we present a set of tools to evaluate cluster stability by subsampling, which can guide parameter choice and aid in biological interpretation. The R package scclusteval and the accompanying Snakemake workflow implement all steps of the pipeline: subsampling the cells, repeating the clustering with Seurat, and estimation of cluster stability using the Jaccard similarity index. The Snakemake workflow takes advantage of high-performance computing clusters and dispatches jobs in parallel to available CPUs to speed up the analysis. The scclusteval package provides functions to facilitate the analysis of the output, including a series of rich visualizations. Availability: R package scclusteval: https://github.com/crazyhottommy/scclusteval Snakemake workflow: https://github.com/crazyhottommy/pyflow_seuratv3_parameter."

# Featured image thumbnail (optional)
image_preview = ""

# Is this a selected publication? (true/false)
selected = true

# Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter the filename of your project in `content/project/`.
#   Otherwise, set `projects = []`.
# projects = ["example-external-project.md"]

# Links (optional).
#url_pdf = "#"
url_preprint = "https://www.biorxiv.org/content/10.1101/2020.05.26.116640v1"
url_code = "https://github.com/crazyhottommy/scclusteval"
url_dataset = "https://osf.io/rfbcg/"
# url_project = "#"
# url_slides = "#"
# url_video = "#"
# url_poster = "#"
# url_source = "#"

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#url_custom = [{name = "Custom Link", url = "http://example.org"}]

# Does the content use math formatting?
math = true

# Does the content use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "headers/scclusteval_flow.png"
caption = "My caption :smile:"

+++

More detail can easily be written here using *Markdown* and $\rm \LaTeX$ math code.
