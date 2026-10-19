---
title: convert Eland sorted.txt to bam file
author: Ming Tang
date: '2013-06-01T16:40:00-07:00'
lastmod: '2013-06-01T16:40:00-07:00'
slug: convert-eland-sorted-txt-to-bam-file
tags:
- bam
- Eland
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/convert-eland-sortedtxt-to-bam-file.html
description: I downloaded a human ChIP-seq data set mapped to hg18 from NCBI GEO.
summary: I downloaded a human ChIP-seq data set mapped to hg18 from NCBI GEO.
header:
  caption: ''
  image: ''
---

I downloaded a human ChIP-seq data set mapped to hg18  from NCBI GEO.

it is in Eland sorted.txt format, I want to convert it to bam file for some downstream analysis.

a google search: <http://www.biostars.org/p/3121/>\
I followed the link to <http://genomewiki.ucsc.edu/index.php/ABRF2010_Tutorial>

the export2sam.pl script in the samtool is what I need.

I downloaded samtools-0.1.8 and found the script in the misc folder.

but I got  errors when I tried to convert it:

`tommy@tommy-ThinkPad-T420:~/Downloads/samtools-0.1.8/misc$ ./export2sam.pl --read1=test.txt`\
`@PG ID:export2sam.pl VN:2.0.0 CL:./export2sam.pl --read1=test.txt`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 1.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 2.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 3.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 4.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 5.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 6.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 7.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 8.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 9.`\
`Use of uninitialized value $t[21] in string eq at ./export2sam.pl line 279, <$fh1> line 10.`

closely looking into the eland export file, I found that it missed a column at the \[21\] position.

A sample sorted.txt file looks like this:

SOLEXA2 90529 1 1 0 1514 0 1 AGCAGATGTTAAAGCAAGACNAACAGTCTCCC LWYWSWYYWZZZXVZZZXZMDNZZYQYZZZZZ chr21.fa 46467450 R 20C11 84 N\
SOLEXA2 90529 1 1 0 32 0 1 CCTAAGCAAAGAAGTCACTCNGTCACTGAACA NHMFPXXUUVYLPSUUUUYMDKVVTWYYUXXX chr21.fa 45472787 F 1T18A11 69 N\
SOLEXA2 90529 1 1 5 121 0 1 TCGAGTGCACACACCCAGCAGCTCTGCAGATA \SH\_\ZYU_U\_\\\_\]\]\_\]\_^a\\X^\\XSS\_\\ chr21.fa 43555500 F 1A30 69 Y

the last column contains a letter N or Y denoting whether the sequence passed the quality filtering.

So, I have to add a  "Y" to all the lines in my file:

`tommy@tommy-ThinkPad-T420:~/Downloads/samtools-0.1.8/misc$ cat test.txt | awk '{print $0, "\tY"}' > eland.txt`

then, use the export2sam script:

`tommy@tommy-ThinkPad-T420:~/Downloads/samtools-0.1.8/misc$ ./export2sam.pl --read1=eland.txt | perl -wpe 's/(chr.*)\.fa/$1/' > test.sam`

\#  perl changes all the chr1.fa to chr1  etc....

And this sam file lacks header <http://blog.daum.net/seq/20>   the headers are tab delimited.

@HD VN:1.0 SO:unsorted\
@SQ SN:chr1 LN:247249719\
@SQ SN:chr2 LN:242951149\
@SQ SN:chr3 LN:199501827\
@SQ SN:chr4 LN:191273063\
@SQ SN:chr5 LN:180857866\
@SQ SN:chr6 LN:170899992\
@SQ SN:chr7 LN:158821424\
@SQ SN:chr8 LN:146274826\
@SQ SN:chr9 LN:140273252\
@SQ SN:chr10 LN:135374737\
@SQ SN:chr11 LN:134452384\
@SQ SN:chr12 LN:132349534\
@SQ SN:chr13 LN:114142980\
@SQ SN:chr14 LN:106368585\
@SQ SN:chr15 LN:100338915\
@SQ SN:chr16 LN:88827254\
@SQ SN:chr17 LN:78774742\
@SQ SN:chr18 LN:76117153\
@SQ SN:chr19 LN:63811651\
@SQ SN:chr20 LN:62435964\
@SQ SN:chr21 LN:46944323\
@SQ SN:chr22 LN:49691432\
@SQ SN:chrX LN:154913754\
@SQ SN:chrY LN:57772954\
@SQ SN:chrM LN:16571

So, I "cat" the header to my resulted sam file and convert it to bam with samtools view.
