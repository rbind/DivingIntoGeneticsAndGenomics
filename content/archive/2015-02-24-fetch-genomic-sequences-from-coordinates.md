---
title: fetch genomic sequences from coordinates
author: Ming Tang
date: '2015-02-24T08:19:00-08:00'
lastmod: '2015-02-24T08:19:00-08:00'
slug: fetch-genomic-sequences-from-coordinates
tags:
- coordinates
- fasta
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/02/fetch-genomic-sequences-from-coordinates.html
description: One of the most common tasks in bioinformatics is to retrieve DNA sequences according to the given coordinates (in bed format or others). I had a post…
summary: One of the most common tasks in bioinformatics is to retrieve DNA sequences according to the given coordinates (in bed format or others). I had a post…
header:
  caption: ''
  image: ''
---

One of the most common tasks in bioinformatics is to retrieve DNA sequences according to the given coordinates (in bed format or others). I had a [post](/archive/batch-converting-coordinates-to-sequences/) previously to describe this, and if you google around, you will find many other methods. I am going to compile all the methods that I found in this post.

Please refer to the posts in biostar:

<https://www.biostars.org/p/19267/>\
<https://www.biostars.org/p/3137/>

<https://www.biostars.org/p/56/>

Method 1: Using DAS server\
Have a look at this help page from UCSC genome browser <http://genome.ucsc.edu/FAQ/FAQdownloads.html>

in a web browser :

<http://genome.ucsc.edu/cgi-bin/das/hg19/dna?segment=chr1:100000,100010>\
This XML file does not appear to have any style information associated with it. The document tree is shown below.\
\<DASDNA\>\
\<SEQUENCE id="chr1" start="100000" stop="100010" version="1.00"\>\
\<DNA length="11"\>cactaagcaca\</DNA\>\
\</SEQUENCE\>

\</DASDNA\>

**The coordinates are 1 based !**\
One can also use the ensemble DAS server\
<http://www.ensembl.org/das/Homo_sapiens.GRCh37.reference/sequence?segment=1:100000,100010>

``` text
Segment:
1:100000-100010

100000 cactaagcaca                                                   100010

Ensembl © WTSI / EBI
```

**The coordinates are 1 based too!**\
Other methods in a gist:

{{< gist crazyhottommy 6360523051f63fe56cc8 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/6360523051f63fe56cc8)

Make sure you know how the software works. Mistaking 0 or 1 based coordinates can be dangerous.\
Concerning speed, I have not done a test yet, but if you have a fasta file in your local machine, it will be faster to retrieve sequences. samtools requires you to index the genome fasta first, it takes a while, but you only need to do it once. Similarly, one can use the pyfaidx package <https://github.com/mdshw5/pyfaidx> .  For the cruzdb package, you can mirror the UCSC database to your local computer for faster accession.
