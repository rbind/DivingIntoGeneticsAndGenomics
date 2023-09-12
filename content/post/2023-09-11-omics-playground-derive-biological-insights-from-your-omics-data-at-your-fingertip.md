---
title: 'Omics Playground: Derive biological insights from your omics data at your
  fingertip'
author: Ming Tang
date: '2023-09-11'
slug: omics-playground-derive-biological-insights-from-your-omics-data-at-your-fingertip
categories:
  - bioinformatics
  - GUI
tags:
  - bioinformatics
  - GUI
header:
  caption: ''
  image: ''
---

**Disclaimer: This post is sponsored by [BigOmics platform](https://bigomics.ch/). I have personally tested the platform. The opinions and views expressed in this post are solely those of the author and do not represent the views of my employer**.

### A brief description of the platform. What challenges could the platform solve?

The BigOmics platform - [Omics Playground](https://playground.bigomics.ch/)- provides a simplified approach for the effective processing of bulk RNA-seq data and proteomics data, resolving many issues experienced by scientists in the field.

This platform has an intuitive user interface that makes analysis more straightforward. It incorporates crucial elements like sample quality control and a gene-centric view to ensure users can easily browse their data. A count/expression data file (counts.csv), a sample information file (samples.csv), and an optional file defining statistical comparisons (comparisons.csv) are among the necessary input files.

When these files are uploaded, the platform quickly creates a Principal Component Analysis (PCA) graphic. Once the analysis is configured, it gives quick insights into the data structure. The analytical workflow is streamlined by creating comparative contrasts by dragging and dropping objects.

Omics Playground's noteworthy benefit is its ability to automatically perform various downstream RNA-seq analyses. It conducts differential expression analysis, produces MA and volcano plots, and builds heatmaps. It also carries out gene-gene correlation analysis, gene set enrichment analysis ([GSEA](https://www.gsea-msigdb.org/gsea/index.jsp)), possible biomarker identification, and weighted gene co-expression network analysis ([WGCNA](https://horvath.genetics.ucla.edu/html/CoexpressionNetwork/Rpackages/WGCNA/Tutorials/)) for gene module analysis.

With sophisticated experimental designs that require several varied comparisons, this platform successfully tackles the issue of labor-intensive and time-consuming bespoke analyses. Researchers may now rely upon Omics Playground to speed up the processing of their RNA-seq data, giving them more time for crucial interpretation and scientific breakthroughs.

### What could be the value proposition for i) biologists, ii) bioinformaticians and iii) biotech companies?

For biologists: Omics Playground serves as a user-friendly data analysis dashboard, offering a range of visualizations that facilitate quick data understanding and insight extraction. Notably, the platform automatically converts ENSEMBL IDs for genes in the count table into official gene symbols, making it easier to analyze the data. The platform also provides a gene overview that shows the levels of gene expression in distinct [GTEX](https://www.gtexportal.org/home/) tissues, enhancing our understanding of biology. Using feature importances produced by six machine learning algorithms, including XGBoost and Random Forest, biologists may also quickly find new biomarkers. This feature enables scientists to extract important biological insights easily.

For bioinformaticians: Omics Playground is a valuable tool since it eliminates the need for considerable custom code creation, saving them time. It speeds up the data analysis, enabling bioinformaticians to conduct studies quickly and work efficiently with wet lab scientists. The platform's capacity to promptly discover prospective biomarkers is quite helpful since it speeds the identification of essential targets and genes of interest.

For Biotech: Because Omics Playground makes it possible to quickly and effectively analyze large amounts of RNA-seq data, biotech businesses stand to benefit significantly from it. Due to its efficiency, Research and Development (R&D) activities move more quickly, and decision-making processes accelerate. Biotech businesses may maximize resource allocation and strengthen their competitive edge in the dynamic and quick-moving biotechnology industry by utilizing Omics Playground.

### What are the capabilities and strengths of the platform? What do I like the most?

The feature that appeals to me the most about this platform is the interactive features of many plots generated. For example, it is easy to upload customized gene sets and highlight them in the volcano plot; the heatmaps are interactive (**see the figure below**), and one can zoom in and out or hover over the mouse to get the gene information. 

![](/img/bigomics.png)

Another noteworthy aspect of the platform is its ability to compare many comparisons simultaneously. Additionally, the Weighted Gene Co-expression Network Analysis (WGCNA) module's integration with drug connectivity analysis is tremendously beneficial since it quickly identifies new drug candidates and interesting gene modules. Those features make it an essential tool for thorough data exploration and analysis.



### What can be improved?

A more interactive visualization option for the pathway analysis module would be great. For example, it will be nice for the users to click a gene in the pathway that is identified and get more information. It would also be beneficial to give users a wider variety of choices for editing and configuring figures so they are easily suited for publishing.

The efficiency and synergy of research teams might be significantly improved by enhancing the platform's collaboration capabilities by enabling bioinformaticians to easily share their projects with wet lab scientists, allowing them to view and study the same dataset together. These suggested changes will improve user experience and promote seamless scientific cooperation.

In conclusion, biotech businesses without dedicated bioinformatics will benefit from using [Omics Playground](https://bigomics.ch/). Omics Playground may also successfully free up the valuable time of bioinformaticians, allowing them to focus on more critical exploratory work for businesses with limited bioinformatics resources. In turn, wet biologists are thus free to explore data in more depth, eventually increasing the speed and effectiveness of data processing procedures.


