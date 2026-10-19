---
title: useful unix one liners for bioinformatics
author: Ming Tang
date: '2013-10-24T11:23:00-07:00'
lastmod: '2013-10-24T11:23:00-07:00'
slug: useful-unix-one-liners-for-bioinformatics
tags:
- awk
- computation skills for biologist
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/useful-unix-one-liner-for-bioinformatics.html
description: I have been using ubuntu 12.04LTS for one year, and I really love it and do not look back at windows at all. Currently, I have dual booting system for linux…
summary: I have been using ubuntu 12.04LTS for one year, and I really love it and do not look back at windows at all. Currently, I have dual booting system for linux…
header:
  caption: ''
  image: ''
---

I have been using ubuntu 12.04LTS for one year, and I really love it and do not look back at windows at all.  Currently, I have dual booting system for linux and windows, but now I barely fire the windows.

The power of Linux comes from, at least in one aspect, its command line. As I have stated before, in  the field of bioinformatics, we spend a lot of time in reformatting, cleaning, extracting the data. Linux commands become very handy in these routine jobs.\
several good resources:\
1. From the author of BWA samtools Heng Li <http://lh3lh3.users.sourceforge.net/biounix.shtml>\
2. Stephen Turner from UVA <http://gettinggeneticsdone.blogspot.com/2013/10/useful-linux-oneliners-for-bioinformatics.html>\
3. <http://genomics-array.blogspot.com/2010/11/some-unixperl-oneliners-for.html>\
4. UT at Austin <https://wikis.utexas.edu/display/bioiteam/Scott's+list+of+linux+one-liners> very good resource for NGS analysis.

Most commonly used ones for me are:\
head, tail, wc, tr, sort, uniq, cat, cut, paste, join, grep(or the more powerful ack), find, xargs, comm, diff, awk, sed etc

Everyday, I learn new stuffs.\
yesterday, from twitter, I learned this one:

[**Vince Buffalo** ‏@vsbuffalo](https://twitter.com/vsbuffalo)[22h](https://twitter.com/vsbuffalo/status/393090046816817152 "12:02 PM - 23 Oct 13")\
Compare 2 arbitrary columns of different files: paste \<(cut -f2 file1.txt) \<(cut -f7 file2.txt) \| awk '{if ($1 != $2) { print "do stuff"} }'

some others:\
let's say you have a fasta file contain multiple sequences, and you want to split it to many files with one record per file.

`tommy@tommy-ThinkPad-T420:~$ cat contig`\
`>contig1`\
`ATCGGGTC`\
`>contig2`\
`GCTCGTTCAA`\
`>contig3`\
`TACGGGGT`

`tommy@tommy-ThinkPad-T420:~$ cat contig | awk '/^>/{close("out"n);n++}{print > "out"n}'`\
`tommy@tommy-ThinkPad-T420:~$ ls out*`\
`out1 out2 out3`\
`tommy@tommy-ThinkPad-T420:~$ cat out1`\
`>contig1`\
`ATCGGGTC`\
`tommy@tommy-ThinkPad-T420:~$ cat out2`\
`>contig2`\
`GCTCGTTCAA`\
`tommy@tommy-ThinkPad-T420:~$ cat out3`\
`>contig3`\
`TACGGGGT`\
print hidden characters ( cat -v is not what I want)\
`tommy@tommy-ThinkPad-T420:~$ sed -n l contig`\
`>contig1$`\
`ATCGGGTC$`\
`>contig2$`\
`GCTCGTTCAA$`\
`>contig3$`\
`TACGGGGT$`

it can also print out the tabs as \t

I have a bed file that contains the Transcription start sites information, some genes have multiple records because they have isoforms, but I only want to use the unique TSSs for my downstream analysis. of course, you can do cut -f1-3 \| sort \| uniq, but I just show you another way to do it by awk

`tommy@tommy-ThinkPad-T420:~$ head tss_-3kb_+3kb_hg19.txt`\
`chr1 66996824 67002824 NM_032291 +`\
`chr1 50486626 50492626 NM_032785 -`\
`chr1 33543713 33549713 NM_052998 +`\
`chr1 8381389 8387389 NM_001080397 +`\
`chr1 25068759 25074759 NM_013943 +`\
`chr1 16764166 16770166 NM_018090 +`\
`chr1 16764166 16770166 NM_001145278 +`\
`chr1 16764166 16770166 NM_001145277 +`\
`chr1 92368559 92374559 NM_001195684 -`\
`chr1 92348836 92354836 NM_001195683 -`\
you can see a record with identical first three columns, but with three different transcript ids. To get only the first occurrence of the TSS

`tommy@tommy-ThinkPad-T420:~$ head tss_-3kb_+3kb_hg19.txt | awk '!a[$1,$2,$3]++'`\
`chr1 66996824 67002824 NM_032291 +`\
`chr1 50486626 50492626 NM_032785 -`\
`chr1 33543713 33549713 NM_052998 +`\
`chr1 8381389 8387389 NM_001080397 +`\
`chr1 25068759 25074759 NM_013943 +`\
`chr1 16764166 16770166 NM_018090 +`\
`chr1 92368559 92374559 NM_001195684 -`\
`chr1 92348836 92354836 NM_001195683 -`

That just gives you a flavour of how powerful command lines are:)
