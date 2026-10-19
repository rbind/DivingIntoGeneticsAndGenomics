---
title: bwa-mem multi-mapping reads
author: Ming Tang
date: '2017-08-10T11:27:00-07:00'
lastmod: '2017-08-10T11:27:00-07:00'
slug: bwa-mem-multi-mapping-reads
tags:
- bwa
- samtools
- WES
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/08/bwa-mem-multi-mapping-reads.html
description: I was using TEQC to do quality control of my WES bam files aligned by bwa-mem. My data are paired end, so a function reads2pairs is called to make the…
summary: I was using TEQC to do quality control of my WES bam files aligned by bwa-mem. My data are paired end, so a function reads2pairs is called to make the…
header:
  caption: ''
  image: ''
---

### An error

I was using [TEQC](https://bioconductor.org/packages/release/bioc/html/TEQC.html) to do quality control of my WES bam files aligned by bwa-mem. My data are paired end, so a function `reads2pairs` is called to make the paired-end reads to be a single fragment. I then get this error:

``` text
> readpairs <- reads2pairs(reads)
Error in reads2pairs(reads) : read pair IDs do not seem to be unique
```

I [asked](https://support.bioconductor.org/p/98786/#99082) in the bioconductor support site and went to the source code of that function. It turns out that `TEQC` was complaining about the not uniquely mapped reads.

### How does BWA-MEM determine and mark the not uniquely mapped reads?

google directed me to biostars again with no surprising. Please read this three posts:\
<https://www.biostars.org/p/76893/>\
<https://www.biostars.org/p/167892/>

> one extract multi-mapped reads by looking for mapq \< 23 and/or the XA flag on the reads

``` text
samtools view -q 40 file.bam
samtools view -q 1 file.bam
samtools view -q 23 file.bam
```

BWA added tags (Tags starting with ‘X’ are specific to BWA):

| Tag | Meaning                                         |
|-----|-------------------------------------------------|
| NM  | Edit distance                                   |
| MD  | Mismatching positions/bases                     |
| AS  | Alignment score                                 |
| BC  | Barcode sequence                                |
| X0  | Number of best hits                             |
| X1  | Number of suboptimal hits found by BWA          |
| XN  | Number of ambiguous bases in the referenece     |
| XM  | Number of mismatches in the alignment           |
| XO  | Number of gap opens                             |
| XG  | Number of gap extentions                        |
| XT  | Type: Unique/Repeat/N/Mate-sw                   |
| XA  | Alternative hits; format: (chr,pos,CIGAR,NM;)\* |
| XS  | Suboptimal alignment score                      |
| XF  | Support from forward/reverse alignment          |
| XE  | Number of supporting seeds                      |

### Checking my own bam files

``` text
samtools view my.bam | grep "XA:" | head -2

E00514:124:H2FC7CCXY:1:1221:16741:50076 97 1 69019 0 151M = 69298 430 CTCCTTCTCTTCTTCAAGGTAACTGCAGAGGCTATTTCCTGGAATGAATCAACGAGTGAAACGAATAACTCTATGGTGACTGAATTCATTTTTCTGGGTCTCTCTGATTCTCAGGAACTCCAGACCTTCCTATTTATGTTGTTTTTTGTAT @@CC?=D@E@=F@>F>?FG==?FAGG?F?FFGA<=?>GFAGF?@=G>?=F?>E@>F=G>>>E?=>>;>D@E@;<EE<E=CAE<>;<D@<<<==D@ED@<D@D@E>E<<=D@E@E>=>C@DD@E=CD@<DD@<>=><>G>>G>>>=;;?;;> XA:Z:15,-102463184,151M,0;19,+110607,151M,1; MC:Z:151M BD:Z:NNNNNNNONONNONNONNOONNNOOONONOONONNNGNNNOONNNOONNOONNNOOOMONFNNONNNNNONONNOOOMOOOOONNNNOONGGGNOOOLOOONONOOOONNONOOONNNONNOONOONNNNNNNNGNNOMNNMNGGGGNMNN MD:Z:151 BI:Z:QQPQQQQPOPQQQQQQQQQQQQQQRQQRQQQQRQQQMQQQRRQQQRQQQRQQQQQQRQQQKQQQQQQQQQPQQQRRQQQQQRQQQQQQQQMMMQQRRPQRQPQPQRQRQQQPQRQQQQQPQQRQQQQQQQQQQQMQQRQQQQQMMMMQQQQ NM:i:0 MQ:i:0 AS:i:151 XS:i:151 RG:Z:F17042956987-KY290-2-WES
E00514:124:H2FC7CCXY:1:2219:12855:17641 81 1 69039 0 150M 13 83828251 0 AACTGCAGAGGCTATTTCCTGGAATGAATCAACGAGTGAAACGAATAACTCTATGGTGACTGAATTCATTTTTCTGGGTCTCTCTGATTCTCAGGAACTCCAGACCTTCCTATTTATGTTGTTTTTTGTATTCTATGGAGGAATCGTGTT =;C?FEAFAFGF<>>>=FF@DD=<@D=<;E<<?D@C@E=<<?D;<;<<E<E;=@DC@D<E@D=<==E<>>=><E@EDD<E=E=E@D<>>F>G@CF>=F>FGAF=GF@?FG===><=@E??E??????F==??F==?FF@EE=<=>D@B@@ XA:Z:15,+102463165,150M,0;19,-110627,150M,1; BD:Z:NOONOOONNNOONNFNNOOONNNOONNOONNNNNOMONGNNNNNNNNOONONOOOMOOOOONNNNOONFFFNNOOLOONONONOOONNNOOONNNNOONOOOOOONNNOONNFNNOMNNMNFFFFNMNNNNNONOONNNNNNOONMMNNN MD:Z:150 BI:Z:QRRQQRQPQQRQQQKQQQRQQQQQQQQRQQQQQPQPQQMQQQQQQQQRQQQQQQQPQRRRQQQQQQRQKKKQQRQPQQQQQQQRQRQQQQQRQQQQRQQRRQRQQQQQQQQQKQQQOQQOQKKKKQOQQQQQQQQQQPQQPPQQQPOQQQ NM:i:0 AS:i:150 XS:i:150 RG:Z:F17042956987-KY290-2-WES
```

Indeed, most of the reads with `XA:` tag has a quality score of 0 (fifth column).

``` text
samtools view -q 1 my.bam | wc -l
7787794
samtools view my.bam | grep -v "XA:" | wc -l
7972777

## not exactly the same number, what's wrong?
samtools view my.bam | grep  "XA:" | awk '{print $5}' | sort | uniq -c
201878 0
    463 1
    688 10
    666 11
    677 12
    693 13
    271 14
    777 15
    281 16
    414 17
    564 18
   1429 19
    192 2
    327 20
   3772 21
   1674 22
    742 23
   1543 24
   3106 25
    368 26
   6223 27
    498 28
    514 29
    760 3
    830 30
   1526 31
    954 32
   3726 33
    367 34
    343 35
    379 36
    641 37
    150 38
   1082 39
    442 4
   3058 40
    673 41
    866 42
   2570 43
   1285 44
   6374 45
   6885 46
   7669 47
  22571 48
  17666 49
    611 5
  16128 50
  13824 51
   9864 52
   6277 53
   4328 54
   2568 55
   1440 56
   4547 57
   1462 58
   1888 59
   1171 6
 169162 60
      2 61
      3 62
      1 64
      5 65
      1 67
      2 69
   1611 7
     86 70
   2234 8
   4013 9
```

so, many reads with `XA` tag with mapping quality scores \> 0 !!

### retain only uniquely mapped reads

``` text
samtools view -h my.bam | awk '$17 !~ /XA:/|| $1 ~ /^@/' | samtools view -bS - > my_unique.bam
```
