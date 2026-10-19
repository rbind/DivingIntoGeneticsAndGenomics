---
title: How to make a heatmap based on ChIP-seq data by R
author: Ming Tang
date: '2013-08-27T12:42:00-07:00'
lastmod: '2013-08-27T12:42:00-07:00'
slug: how-to-make-a-heatmap-based-on-chip-seq-data-by-r
tags:
- ChIP-seq
- heatmap
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/08/how-to-make-heatmap-based-on-chip-seq.html
description: update on 04/20/2016.\ I noticed that this post is the most frequently visited one. it has been almost 3 years\ since I wrote this post. Now, there are…
summary: update on 04/20/2016.\ I noticed that this post is the most frequently visited one. it has been almost 3 years\ since I wrote this post. Now, there are…
header:
  caption: ''
  image: ''
---

**update on 04/20/2016.**\
I noticed that this post is the most frequently visited one. it has been almost 3 years\
since I wrote this post. Now, there are various tools for this purpose. See a\
post on [biostars](https://www.biostars.org/p/180314/).

well, I recently just went through the whole process for making a heatmap based on a ChIP-seq data set. If you do not know the technique, google it :)  <http://en.wikipedia.org/wiki/ChIP-sequencing>

Often, you have a ChIP-seq data that are mapped to the reference genome ( a bam file). You want to plot the sequence tag intensity around certain features ( transcription start sites, gene body, enhancers, or any other genomic region you defined).

you can make an average plot [How to make TSS plot using RNA-seq and ChIP-seq data](/archive/how-to-make-tss-plot-using-rna-seq-and-chip-seq-data/). I will need to re-write this one though, the code format is just too bad (I have to learn how to embed R and python code into the blog) ....and the Y axis is not normalized to counts per million.

you may also want to generate a heatmap with the same data.  see ngsplot for examples <https://code.google.com/p/ngsplot/>.  If you do not want to code R by yourself, try it. It has been improved a lot since last time I checked it. I once asked a question in the google group : <https://groups.google.com/forum/#!topic/ngsplot-discuss/efHQ-P-14XM>.

Seqmonk <http://www.bioinformatics.babraham.ac.uk/projects/seqmonk/>  from Simon Andrews can also plot this kind of figure very easily, I am just not satisfied with the picture quality, and I want more customized control of the picture.

I will just paste my code below, and it is heavily commented, you should be able to follow it fairly easily.\
update on 05/05/2015, I put the code in a gist instead:

{{< gist crazyhottommy 63d743958d250e294d9c >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/63d743958d250e294d9c)

![](/img/archive/how-to-make-a-heatmap-based-on-chip-seq-data-by-r/01-dnase_looping__hre_distal_hif_heatmap.png)

The second gist:

{{< gist crazyhottommy 5a929801a3a8cb697d15 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/5a929801a3a8cb697d15)

![](/img/archive/how-to-make-a-heatmap-based-on-chip-seq-data-by-r/02-test.png)

That's all!

I hope you have learned something after reading it:)

===============================
update on 09/17/13\
arrange the rows in the heatmap by the coverage from strong to weak

{{< gist crazyhottommy 56576e633bfc888937c3 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/56576e633bfc888937c3)

![](/img/archive/how-to-make-a-heatmap-based-on-chip-seq-data-by-r/03-dnase_looping_distal_hre.png)
