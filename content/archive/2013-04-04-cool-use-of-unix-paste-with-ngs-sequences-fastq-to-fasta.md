---
title: Cool use of Unix paste with NGS sequences:FASTQ to FASTA
author: Ming Tang
date: '2013-04-04T19:29:00-07:00'
lastmod: '2013-04-04T19:29:00-07:00'
slug: cool-use-of-unix-paste-with-ngs-sequences-fastq-to-fasta
tags:
- FASTQ
- unix
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/cool-use-of-unix-paste-with-ngs.html
description: While browsing SeqAnswers.com today I came across a post where Uwe Appelt provided a couple of lines of Unix shell wizadry to solve some problem. What…
summary: While browsing SeqAnswers.com today I came across a post where Uwe Appelt provided a couple of lines of Unix shell wizadry to solve some problem. What…
header:
  caption: ''
  image: ''
---

### 

### <http://thegenomefactory.blogspot.com.au/search?updated-max=2012-09-08T12:11:00%2B10:00&max-results=7>

### [Cool use of Unix paste with NGS sequences](http://thegenomefactory.blogspot.com.au/2012/05/cool-use-of-unix-paste-with-ngs.html)

While browsing SeqAnswers.com  today I came across a [post](http://seqanswers.com/forums/showthread.php?t=6847&referrerid=2547) where Uwe Appelt provided a couple of lines of Unix shell wizadry to solve some problem. What attracted my attention was the following:

> `paste - - - - < in.fq | filter | tr "\t" "\n" > out.fq`

Now, I've done a reasonable amount of shell one-liners in my life, but I'd never seen this before.  I've used the paste command a couple of times, but clearly its potential power did not sink in! Here is the man page description for paste:

> `Write lines consisting of the sequentially corresponding lines from each FILE, separated by TABs, to standard output. With no FILE, or when FILE is -, read standard input.`

So what's happening here? Well, in Unix, the "-" character means to use STDIN instead of a filename. Here Uwe is providing paste with *four filenames, each of which is the same stdin filehandle. *So lines 1..4 of input.fq are put onto one line (with tab separator), and lines 5..8 on the next line and so on. Now, our stream has the four lines of FASTQ entry on a single line, which makes it much more amenable to Unix line-based manipulation, represented by *filter* in my example. Once that's all done, we need to put it back into the standard 4-line FASTQ format, which is as simple as converting the tabs "\t" back to newlines "\n" with the tr command.

***Example 1: FASTQ to FASTA***

`A common thing to do is convert FASTQ to FASTA, and we don't always have our favourite tool or script to to this when we aren't on our own servers:`

` paste - - - - < in.fq | cut -f 1,2 | sed 's/^@/>/' | tr "\t" "\n" > out.fa`

1.  *paste* converts the input FASTQ into a 4-column file
2.  *cut *command extracts out just column 1 (the ID) and column 2 (the sequence)
3.  *sed* replaces the FASTQ ID prefix "@" with the FASTA ID prefix "\>"
4.  *tr* conversts the 2 columns back into 2 lines

#### 

And because the shell command above uses a pipe connecting four commands (paste, cut, sed, tr) the operating system will run them all in parallel, which will make it run faster assuming your disk I/O can keep up. 

***Example 2: Removing redundant FASTQ ID in line 3***

The third line in the [FASTQ format](http://en.wikipedia.org/wiki/FASTQ_format) is somewhat redundant - it is usually a duplicate of the first line, except with "+" instead of "@" to denote that a quality string is coming next rather than an ID. Most parsers ignore it, and happily accept a blank ID after the "+", which saves a fair chunk of disk space. If you have legacy files with the redundant IDs and want to conver them, here's how we can do it with our new paste trick:

`paste -d ' ' - - - - | sed 's/ +[^ ]*/ +/' | tr " " "\n"`

1.  *paste* converts the input FASTQ into a 4-column file, but using SPACE instead of TAB as the separator character
2.  *sed *finds and replaces the "+DUPE_ID" line with just a "+"
3.  *tr* conversts the 4 columns back into 4 lines

That's it for today, hope you learnt something, because I certainly did.
