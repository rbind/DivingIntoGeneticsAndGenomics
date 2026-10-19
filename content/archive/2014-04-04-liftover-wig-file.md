---
title: liftover wig file
author: Ming Tang
date: '2014-04-04T14:44:00-07:00'
lastmod: '2014-04-04T14:44:00-07:00'
slug: liftover-wig-file
tags:
- liftover
- UCSC
- wig
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/04/liftover-wig-file.html
description: I was looking at a public ChIP-seq data set, but the author did not put the raw sequence data in the GEO database. Instead, they uploaded a wig file of the…
summary: I was looking at a public ChIP-seq data set, but the author did not put the raw sequence data in the GEO database. Instead, they uploaded a wig file of the…
header:
  caption: ''
  image: ''
---

I was looking at a public ChIP-seq data set, but the author did not put the raw sequence data in the GEO database. Instead, they uploaded a wig file of the ChIP-seq signal file and it was mapped to human hg18 reference genome.

All the other data sets I am analyzing are mapped to hg19, so I have to liftover this wig file to hg19 also.

Solution: CrossMap! <http://crossmap.sourceforge.net/>

## Convert Wiggle/BigWig format files

[Wiggle](http://genome.ucsc.edu/goldenPath/help/wiggle.html) (WIG) format is useful for displaying continuous data such as GC content and reads intensity of high-throughput sequencing data. BigWig is a self-indexed binary-format Wiggle file, and has the advantage of supporting random access. This means only regions that need to be displayed are retrieved by genome browser, and it dramatically reduces the time needed for data transferring ([Kent et al., 2010](http://bioinformatics.oxfordjournals.org/content/26/17/2204.long)). Input wiggle data can be in variableStep (for data with irregular intervals) or fixedStep (for data with regular intervals). Regardless of the input, the output will always in bedGraph format. bedGraph format is similar to wiggle format and can be converted into BigWig format using UCSC [wigToBigWig](http://hgdownload.cse.ucsc.edu/admin/exe/) tool. We export files in bedGraph because it is usually much smaller than file in wiggle format, and more importantly, CrossMap internally transforms wiggle into bedGraph to increase running speed.\
If an input file is in BigWig format, the output is BigWig format if UCSC’s ‘[wigToBigWig](http://hgdownload.cse.ucsc.edu/admin/exe/)‘ executable can be found; otherwise, the output file will be in bedGraph format.\
Typing command without any arguments will print help message:

``` text
$ python2.7 CrossMap.py  wig
```

Screen output:

``` text
Usage:
  CrossMap.py wig input_chain_file input_wig_file output_prefix

Description:
  "input_chain_file" can be regular or compressed (*.gz, *.Z, *.z, *.bz, *.bz2,
  *.bzip2) file, local file or URL (http://, https://, ftp://) pointing to remote
  file.  Both "variableStep" and "fixedStep" wiggle lines are supported. Wiggle
  format: http://genome.ucsc.edu/goldenPath/help/wiggle.html

Example:
  CrossMapy.py wig hg18ToHg19.over.chain.gz test.hg18.wig test.hg19
```

It is very easy to use, after install it following the instruction I did:\
`CrossMap.py wig hg18ToHg19.over.chain.gz my.wig my_hg19`

it generates a bigwig file, a sorted bedgraph and a unsorted bedgraph file.

it took me 36mins to convert a 1.4Gb wig file on my desktop with 4Gb ram.
