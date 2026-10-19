---
title: 0-based and 1-based coordinates
author: Ming Tang
date: '2013-05-08T19:14:00-07:00'
lastmod: '2013-05-08T19:14:00-07:00'
slug: 0-based-and-1-based-coordinates
tags:
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/0-based-and-1-based-coordinates.html
description: '0-base system: the first base is 0. You represent a region as [a, b). This is also called "half-close-half-open", "0-base end exclusive", or "1-base end…'
summary: '0-base system: the first base is 0. You represent a region as [a, b). This is also called "half-close-half-open", "0-base end exclusive", or "1-base end…'
header:
  caption: ''
  image: ''
---

### see the blog here:

<http://allaboutbioinfo.blogspot.com/search?updated-max=2011-08-18T16:56:00-07:00&max-results=7>

###

### There are two different coordinate base systems in different files: 0-base and 1-base. Different files use different base systems, and sometimes it causes confusions (especially when one tries to calculate the length of the region). Here I show the differences in two systems, and summarize several file formats that use both sytems.[Bases in sequence positions](http://allaboutbioinfo.blogspot.com/2011/08/bases-in-sequence-positions.html)

**0-base system**: the first base is 0. You represent a region as \[a, b). This is also called "**half-close-half-open**", "**0-base end exclusive**", or "**1-base end inclusive**". When calculating the length of the region, subtract a from b directly:

*L=b-a*

**1-base system**: the first base is 1, and you represent a region as \[a,b\]. When calculating the length of the region, don't forget to add 1:

*L=b-a+1*

Here is an example. Suppose you want to represent a region of X in the following sequence:

000**XXX**0000

In the 0-base system, this is represented as \[3,6), and the length is 6-3=3. In the 1-base system, use \[4, 6\], and the length is 6-4+1=3.

0-base system files: [BED](http://genome.ucsc.edu/FAQ/FAQformat.html#format1), [BAM](http://samtools.sourceforge.net/SAM1.pdf)\
1-base system files: [SAM](http://samtools.sourceforge.net/SAM1.pdf), [GFF](http://genome.ucsc.edu/FAQ/FAQformat.html#format3), [GTF](http://genome.ucsc.edu/FAQ/FAQformat.html#format4), [Wig](http://genome.ucsc.edu/goldenPath/help/wiggle.html), [PSL](http://genome.ucsc.edu/FAQ/FAQformat.html#format2)
