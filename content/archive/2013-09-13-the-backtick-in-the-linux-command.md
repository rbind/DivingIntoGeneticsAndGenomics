---
title: The Backtick in the linux command
author: Ming Tang
date: '2013-09-13T13:18:00-07:00'
lastmod: '2013-09-13T13:18:00-07:00'
slug: the-backtick-in-the-linux-command
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/the-backtick-in-linux-command.html
description: I was doing my first RNA-seq mapping by Tophat, I downloaded the SRA file from GEO and dump them to fastq files (you need to install the sra-toolkit from…
summary: I was doing my first RNA-seq mapping by Tophat, I downloaded the SRA file from GEO and dump them to fastq files (you need to install the sra-toolkit from…
header:
  caption: ''
  image: ''
---

I was doing my first RNA-seq mapping by Tophat, I downloaded the SRA file from GEO and dump them to fastq files (you need to install the sra-toolkit from the NCBI SRA website). I had total six sra files, and I do not want to write something:

`fastq-dump SRR0001 --split-3`\
`fastq-dump SRR0002 --split-3`\
`........`

for six times.

that's where the backtick come to rescue. (it is usually on the same key as the tilde symbol ~)

The backtick allows you to assign the output of a shell command to a variable. You must surround the entire command line command with backtick characters.

for the task above, you can do:

`` for f in `ls *.sra` ``\
`do`\
`fastq-dump --split-3 $f`\
`done&`

it is a little bit awkward to write something like that, in fact\
you can do:

`for f in *.sra`\
`do`\
`fastq-dump --split-3 $f`\
`done&`

however, if it becomes very handy if you want to do something like this:

`# Run tophat`

`for fq in *fastq`\
`do`\
`` FQ=`basename $fq .fastq` ``\
`tophat -p 8 -G gencode.v18.annotation.gtf -o $FQ`\
`/project/bio/bowtie2/hg19 $fq`\
`done`

in this example, I created a variable FQ to capture the name of the fastq file ( man basename if you do not know it), and I used this variable as the output directory name.

pretty neat!
