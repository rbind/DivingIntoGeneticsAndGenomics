---
title: get all the promoter sequences of human hg19 genome
author: Ming Tang
date: '2015-04-30T14:57:00-07:00'
lastmod: '2015-04-30T14:57:00-07:00'
slug: get-all-the-promoter-sequences-of-human-hg19-genome
tags:
- Bioconductor
- coordinates
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/04/get-all-promoter-sequences-of-human.html
description: One of my former friends (a biologist who does not know much about computer) wants me to help her to get all the promoter sequences from the human genome.…
summary: One of my former friends (a biologist who does not know much about computer) wants me to help her to get all the promoter sequences from the human genome.…
header:
  caption: ''
  image: ''
---

One of my former friends (a biologist who does not know much about computer) wants me to help her to get all the promoter sequences from the human genome. It is a very simple task and I think all the biologists should know how to do it. There are many ways to do it, but I will show you how to do it using bioconductor.

{{< gist crazyhottommy f4ba546e0083abbc5f05 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/f4ba546e0083abbc5f05)

Using less I see all the sequences are there, to make sure the sequences are right, one can manually inspect the UCSC genome browser for several sequences.

![](/img/archive/get-all-the-promoter-sequences-of-human-hg19-genome/01-screenshot-2015-04-30-16.53.26.png)

I really do not want to dig in (google is your friend) to find a way to write the name using the SYMBOL rather than the ENTREZID....\
You can convert the gene ids by many ways too.\
I have two posts for that [converting gene ids using bioconductor with biomaRt and annotation packages](/archive/converting-gene-ids-using-bioconductor-with-biomart-and-annotation-packages/)\
and [mapping gene ids with mygene](/archive/mapping-gene-ids-with-mygene/)\
In addition, I prefer to prepare a bed file for all the promoters using bedtools slop (RefSeq table from UCSC, or from a GENCODE GTF file). Then, use bedtools to extract DNA sequences using bedtools getfasta. To me, it is more flexible on the command lines.\
see my previous post here [fetch genomic sequences from coordinates](/archive/fetch-genomic-sequences-from-coordinates/)
