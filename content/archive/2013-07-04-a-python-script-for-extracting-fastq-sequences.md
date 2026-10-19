---
title: a python script for extracting fastq sequences
author: Ming Tang
date: '2013-07-04T09:05:00-07:00'
lastmod: '2013-07-04T09:05:00-07:00'
slug: a-python-script-for-extracting-fastq-sequences
tags:
- NGS
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/07/a-python-script-for-extracting-fastq.html
description: a simple python script to handle fastq file.
summary: a simple python script to handle fastq file.
header:
  caption: ''
  image: ''
---

a simple python script to handle fastq file.

`fast fastq file parser, extract ~10,000 reads with their sequences and quality`\
`from a fastq file (~70milion reads) based on names of the reads. HengLi in Princeton https://github.com/lh3/seqtk`\
`has a wrapper for this kind of task.`\
`the following code is from http://www.biostars.org/p/10353/`\
`this script demonstrates the usage of set(hash-able), a data structure that is much faster than list`\
`when you have two files, put the information from one file into a container, loop over the other file.`\
`usage: cat file.fastq | python parse_fastq.py id_file.txt > selected.fastq`

`#! /usr/bin/env python`\
`import sys`\
`# get filename from parameter`\
`idfile = sys.argv[1]`

`# load ids in a set with a set comprehension`

`ids = set( x.strip() for x in open(idfile) )`

`# read the fastq file`\
`handle = sys.stdin`

`while ids:`\
` #parse fastq`\
` idline = handle.readline()`\
` seq = handle.readline()`\
` spacer = handle.readline()`\
` quals = handle.readline()`

` id_name = idline[:-1] # except the newline \n`\
` if id_name in ids:`\
` #print fastq`\
` sys.stdout.write( '%s%s%s%s%' % ( idline, seq, spacer,\ quals) )`\
` ids.remove(id_name)`

see link here <https://github.com/lh3/seqtk>
