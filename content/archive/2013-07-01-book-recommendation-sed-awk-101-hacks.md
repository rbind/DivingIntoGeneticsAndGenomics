---
title: 'book recommendation: Sed & Awk 101 Hacks'
author: Ming Tang
date: '2013-07-01T18:38:00-07:00'
lastmod: '2013-07-01T18:38:00-07:00'
slug: book-recommendation-sed-awk-101-hacks
tags:
- unix
- Ubuntu
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/07/book-recommendation-sed-awk-101-hacks.html
description: I highly recommend this book for text manipulation\
summary: I highly recommend this book for text manipulation\
header:
  caption: ''
  image: ''
---

I highly recommend this book for text manipulation\
<http://www.thegeekstuff.com/sed-awk-101-hacks-ebook/>

Through my limited experience of "bioinformatics", I realized that we spend a lot of time to reformat the data ( mostly in txt format) to some other formats which can be fed into other programs.\
python has a very good re (regular expression) module for that purpose, but I do like the one liner of Sed and Awk. Sometimes, I am just too lazy to write something like:

`ifile = open(<filename>)`

`for line in ifile:`\
` do some parse`

`ifile.close()`

because Sed and Awk read file line by line, execute the command for one line, and then process the next line, it is very concise without writing the explicit for loops.

This book is very comprehensive and has a lot of practical examples making you follow easily. I just finished Chapter 5 and I found it is really helpful, although I had some experience with sed and awk.
