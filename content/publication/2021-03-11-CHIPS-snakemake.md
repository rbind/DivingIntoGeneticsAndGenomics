+++
title = "CHIPS: A Snakemake pipeline for quality control and reproducible processing of chromatin profiling data"
date = 2021-03-10T00:00:00

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Len Taing * ", "Clara Cousins *", "Gali Bai", "Cejas Paloma", "Xintao Qiu", "Myles Brown", "Clifford A. Meyer", "X Shirley Liu", "Henry W Long#", "**Ming Tang#**"]

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
publication = "In *bioRxiv*, BioRxiv."
publication_short = "In *BioRxiv*"

# Abstract and optional shortened version.
abstract = "Motivation: The chromatin profile measured by ATAC-seq, ChIP-seq, or DNase-seq experiments can identify genomic regions critical in regulating gene expression and provide insights on biological processes such as diseases and development. However, quality control and processing chromatin profiling data involve many steps, and different bioinformatics tools are used at each step. It can be challenging to manage the analysis. Results: We developed a Snakemake pipeline called CHIPS (CHromatin enrichment Processor) to streamline the processing of ChIP-seq, ATAC-seq, and DNase-seq data. The pipeline supports single- and paired-end data and is flexible to start with FASTQ or BAM files. It includes basic steps such as read trimming, mapping, and peak calling. In addition, it calculates quality control metrics such as contamination profiles, PCR bottleneck coefficient, the fraction of reads in peaks, percentage of peaks overlapping with the union of public DNaseI hypersensitivity sites, and conservation profile of the peaks. For downstream analysis, it carries out peak annotations, motif finding, and regulatory potential calculation for all genes. The pipeline ensures that the processing is robust and reproducible. Availability: CHIPS is available at https://bitbucket.org/plumbers/cidc_chips/src/master/ Contact: mtang@ds.dfci.harvard.edu; henry_long@dfci.harvard.edu"

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
# url_pdf = "files/papers/xxx.pdf"
url_preprint = "https://www.biorxiv.org/content/10.1101/2021.03.09.434676v1"
url_code = "https://bitbucket.org/plumbers/cidc_chips/src/master/"
#url_dataset = "https://osf.io/rfbcg/"
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
image = "headers/CHIPS.png"
caption = "My caption :smile:"

+++

More detail can easily be written here using *Markdown* and $\rm \LaTeX$ math code.
