---
title: use sed to print out every n lines, split pair-end FASTQ file
author: Ming Tang
date: '2013-06-25T13:00:00-07:00'
lastmod: '2013-06-25T13:00:00-07:00'
slug: use-sed-to-print-out-every-n-lines-split-pair-end-fastq-file
tags:
- FASTQ
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/use-sed-to-print-out-every-n-lines.html
description: '#print out the first two lines of every 4 lines. -n flag suppress all of the other lines and only print the line\ you specified. -e option tells sed to…'
summary: '#print out the first two lines of every 4 lines. -n flag suppress all of the other lines and only print the line\ you specified. -e option tells sed to…'
header:
  caption: ''
  image: ''
---

`tommy@tommy-ThinkPad-T420:~$ cat test.txt`\
`1`\
`2`\
`3`\
`4`\
`5`\
`6`\
`7`\
`8`\
`9`\
`10`\
`11`\
`12`

\#print out the first two lines of every 4 lines. -n flag suppress all of the other lines and only print the line\
you specified. -e option tells sed to accept multiple p (print) command.

`tommy@tommy-ThinkPad-T420:~$ sed -ne '1~4p;2~4p' test.txt`\
`1`\
`2`\
`5`\
`6`\
`9`\
`10`

This trick would be useful if you have a pair-end FASTq file and want to split it into two files.

see here:\
<http://seqanswers.com/forums/showthread.php?t=13776>

and here <http://www.biostars.org/p/19446/> two reads in one fastq

from SRA file:\
<http://vinaykmittal.blogspot.com/2012/02/how-to-extract-paired-end-reads-from.html>

### How to extract paired-end reads from SRA files

SRA(NCBI) stores all the sequencing run as single "sra" or "lite.sra" file. You may want separate files if you want to use the data from paired-end sequencing. When I run SRA toolkit's "fastq-dump" utility on paired-end sequencing SRA files, sometimes I get only one files where all the mate-pairs are stored in one file rather than two or three files.\
The solution for the problem is to always run fastq-dump with "--split-3" option. If the experiment is single-end sequencing, only one fastq file will be generated. If it is paired-end sequencing, there may be two or three fastq files.\
Two files (with suffix "\_1" and "\_2") are matched mate-pair read file where as the third one (without any suffix) contains all the reads that do not have any mate-paires (or SRA couldn't resolve mate-paires for them).

Hope my experiences with NCBI SRA data handling help the readership.

Posted by [vinay mittal ](https://plus.google.com/105474277476918192937 "author profile")at [11:02 AM](http://vinaykmittal.blogspot.com/2012/02/how-to-extract-paired-end-reads-from.html "permanent link")
