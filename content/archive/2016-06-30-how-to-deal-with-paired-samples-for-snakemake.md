---
title: How to deal with paired samples for snakemake
author: Ming Tang
date: '2016-06-30T20:32:00-07:00'
lastmod: '2016-06-30T20:32:00-07:00'
slug: how-to-deal-with-paired-samples-for-snakemake
tags:
- ChIP-seq
- snakemake
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/06/how-to-deal-with-paired-samples-for.html
description: I am currently experimenting snakemake for pipelines. We will have paired samples when processing data. e.g. ChIP-seq (IP vs Input), DNA-seq (Normal vs…
summary: I am currently experimenting snakemake for pipelines. We will have paired samples when processing data. e.g. ChIP-seq (IP vs Input), DNA-seq (Normal vs…
header:
  caption: ''
  image: ''
---

I am currently [experimenting](/archive/my-first-ever-minimal-working-chip-seq-pipeline-using-snakemake/) snakemake for pipelines. We will have paired samples when processing data. e.g. ChIP-seq (IP vs Input), DNA-seq (Normal vs Tumor). How to specify them in snakemake?\
The trick is to use a python dictionary to pair samples and a function to define Input files.

I will show an toy example with ChIP-seq processing.

{{< gist crazyhottommy 8fac815d7ef4d80429378fa9a1f30aba >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/8fac815d7ef4d80429378fa9a1f30aba)
