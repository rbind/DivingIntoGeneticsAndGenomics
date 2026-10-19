---
title: awk for simple text manipulation
author: Ming Tang
date: '2013-04-22T18:45:00-07:00'
lastmod: '2013-04-22T18:45:00-07:00'
slug: awk-for-simple-text-manipulation
tags:
- awk
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/awk-for-simple-text-manipulation.html
description: 'let''s say you have a bed file (tab delimited):'
summary: 'let''s say you have a bed file (tab delimited):'
header:
  caption: ''
  image: ''
---

let's say you have a bed file (tab delimited):

tommy@tommy-ThinkPad-T420:~$ cat file.bed\
chr1     100 302\
chr2     600 901\
chr3     250 383

you want to calculate the average peak length of this file:

tommy@tommy-ThinkPad-T420:~$ cat file.bed \| awk '{print $3-$2}'\| awk '{sum+=$0} END {print "Average= " sum/NR}'\
Average= 212

if you calculate the middle point of the peak,  $2+ ($3- $2)/2   and you get a float, you want to round the column:

cat file.bed \| awk '{print $2+($3-$2)/2}'\
201\
750.5\
316.5

tommy@tommy-ThinkPad-T420:~$ cat file.bed \| awk '{print $2+($3-$2)/2}'\| awk 'function round (A) { return int(A+0.5)} { printf("%d\n", round($0))}'\
201\
751\
317

if you want to add an artificial column with peak1, peak2, peak3.....

cat file.bed \| awk '{print $1"\t"$2"\t"$3"\t""peak"NR}'\
chr1  100   302 peak1\
chr2         600   901 peak2\
chr3   250   383 peak3

if you want to change a space delimited file to a tab delimited file

cat foo.txt \| awk ' -F'' { print $1"\t"$2"\t"$3 } \> newfile.txt

you have two files, you want to subtract the third column of file1 (total 3 columns) from the second column of file2(total 3 columns)

paste foo1.txt  foo2.txt \| awk ' { print $3 -$5}'

you want to cut column 3 from file2, cut column 1 from file 1 and put them together:

paste \<(cut -f3 foo2.txt)  \<(cut -f1 foo1.txt)
