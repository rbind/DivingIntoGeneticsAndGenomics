---
title: use linux commands to extract lines from a file by line number
author: Ming Tang
date: '2013-04-04T15:10:00-07:00'
lastmod: '2013-04-04T15:10:00-07:00'
slug: use-linux-commands-to-extract-lines-from-a-file-by-line-number
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/use-linux-commands-to-extract-lines.html
description: many times we want to extract certain lines from a file
summary: many times we want to extract certain lines from a file
header:
  caption: ''
  image: ''
---

many times we want to extract certain lines from a file

tommy@tommy-ThinkPad-T420:~/Datasets$ cat selectlines.txt\
1 one\
2 two\
3 three\
4 four\
5 five\
6 six\
7 seven\
8 eight\
9 nine\
10 ten

\# control + D to indicate the end of the file

extract the first 3 lines

tommy@tommy-ThinkPad-T420:~/Datasets$ sed -n  '1,3 p'  selectlines.txt\
1 one\
2 two\
3 three

<http://stackoverflow.com/questions/83329/how-can-i-extract-a-range-of-lines-from-a-text-file-on-unix>

This is 1-indexed. `-n`suppresses echoing the input as output, which we clearly don't want; the numbers indicate the range of lines to make the following command operate on; the command `p` prints out the relevant lines.

if you have a very large file, add a quit argument, otherwise the sed program will scan to the end of the file\
`sed -n '16224,16482p;16483q' filename`

or use awk

tommy@tommy-ThinkPad-T420:~/Datasets$ awk 'NR==1,NR==3' selectlines.txt\
1 one\
2 two\
3 three

or combine head and tail\
extract  from line 16224 to 16482

``` text
head -16428 < file.in | tail -259 > file.out
```

``` text
or cat file.txt | head -n 16482 | tail -n 258
```

``` text
http://linuxcommando.blogspot.com/2008/04/using-awk-to-extract-lines-in-text-file.html
```

``` text
Using awk, to print every second line counting from line 0 (first printed line is line 2):
$ awk '0 == NR % 2'  somefile.txt
Line 2
Line 4

To print every second line counting from line 1 (first printed line is 1):
$ awk '0 == (NR + 1) % 2'  somefile.txt
Line 1
Line 3
```
