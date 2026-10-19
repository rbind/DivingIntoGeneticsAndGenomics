---
title: Tophat run time
author: Ming Tang
date: '2013-09-17T11:49:00-07:00'
lastmod: '2013-09-17T11:49:00-07:00'
slug: tophat-run-time
tags:
- RNAseq
- Tophat
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/tophat-run-time.html
description: It took me 45 hrs to map a 24G single-end RNA-seq data to human hg19 with the gtf file from GENECODE!
summary: It took me 45 hrs to map a 24G single-end RNA-seq data to human hg19 with the gtf file from GENECODE!
header:
  caption: ''
  image: ''
---

It took me 45 hrs to map a 24G single-end RNA-seq data to human hg19 with the gtf file from GENECODE!

I run tophat on a cluster with 1 node, 8 processors with 3gb ram for each processor (the max free resource as a regular user). I have six such files...it will take me 12 days to finish the mapping....wish I had better access to the computing resource...

a quick google confirmed that it does take that long...\
<http://seqanswers.com/forums/showthread.php?t=5148>\
"It routinely takes ~40 hours for me (~45 million reads, with - p 4 to use four threads and 16G RAM). It will delete the huge temp files it generates, so the disk usage may not be that much of any issue."
