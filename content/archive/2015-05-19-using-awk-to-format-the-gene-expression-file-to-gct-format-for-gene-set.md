---
title: Using Awk to format the gene expression file to gct format for Gene Set Enrichment Analysis (GSEA)
author: Ming Tang
date: '2015-05-19T13:42:00-07:00'
lastmod: '2015-05-19T13:42:00-07:00'
slug: using-awk-to-format-the-gene-expression-file-to-gct-format-for-gene-set
tags:
- awk
- excel
- GSEA
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/05/using-awk-to-format-gene-expression.html
description: After one runs microarray or RNA-seq analysis, usually he would do a Gene Set Enrichment Analysis (GSEA) analysis. There are many tools to use. One of the…
summary: After one runs microarray or RNA-seq analysis, usually he would do a Gene Set Enrichment Analysis (GSEA) analysis. There are many tools to use. One of the…
header:
  caption: ''
  image: ''
---

After one runs microarray or RNA-seq analysis, usually he would do a Gene Set Enrichment Analysis (GSEA) analysis.  There are many tools to use. One of the most commonly used one is GSEA developed in [Broad Institute](http://www.broadinstitute.org/gsea/index.jsp).

It requires [four data files](http://www.broadinstitute.org/gsea/doc/GSEAUserGuideFrame.html?gmx) to be loaded:\
1. Expression dataset in res, gct, pcl or txt format\
2. Phenotype labels in cls format\
3. Gene sets in gmx or gmt formt\
4. Chip annotations

The first impression of mine is that: Oh my, why there are so many different formats? Yes, after merging into the computational biology field for a while, I find that most of the time I spend is on data formatting. That's in consistence with [many others' experiences](https://www.biostars.org/p/71204/).

Well, for this post, I will specifically show you how to format gene expression data file output from affy (for microarray) to gct format using awk. For RNA-seq data, you can do it similarly for DESeq2 and EdgR outputs (using normalized counts).

Let's look at the expression file output by affy:

\# R code\
library(affy)\
\## read in the data\
Data\<- ReadAffy()\
\## RMA normalization and get the eset (expressionSet) object

eset\<- rma(Data)

e\<- exprs(eset)\
write.table( e, "raw_expression.txt", row.names=F, quote=F, sep="\t")

The file we have:

![](/img/archive/using-awk-to-format-the-gene-expression-file-to-gct-format-for-gene-set/01-screenshot-2015-05-19-14.56.52.png)

The required file format:

![](/img/archive/using-awk-to-format-the-gene-expression-file-to-gct-format-for-gene-set/02-screenshot-2015-05-19-15.20.56.png)

we see that the first column is the probe name and the other columns are expression values for different samples. The first problem is that the first line is one grid off; the first column should have a name "Name". In addition, we need to add two lines, and we need to add a dummy column in the second column. We will fix it step by step:

{{< gist crazyhottommy 9d16a293ba786442c6da >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/9d16a293ba786442c6da)

Now, we have the desired format:

![](/img/archive/using-awk-to-format-the-gene-expression-file-to-gct-format-for-gene-set/03-screenshot-2015-05-19-15.32.07.png)

You can certainly open the file in excel and edit it very easily. The file is only several MB big. However, when you have a file that is several hundred MB or several GB big, you can not open it with excel. Avoiding using excel is my ultimate goal, although it comes very handy for small data sets. Using excel for bioinformatics can cause problems:\
It can change gene names to dates\
[https://www.techdirt.com/articles/20140727/03133828025/using-spreadsheets-bioinformatics-can-corrupt-data-changing-gene-names-into-dates.shtml](http://see%20link%20here/)

<https://nsaunders.wordpress.com/2012/10/22/gene-name-errors-and-excel-lessons-not-learned/>

<https://www.techdirt.com/articles/20140727/03133828025/using-spreadsheets-bioinformatics-can-corrupt-data-changing-gene-names-into-dates.shtml>

Again, learning how to use awk is invaluable!
