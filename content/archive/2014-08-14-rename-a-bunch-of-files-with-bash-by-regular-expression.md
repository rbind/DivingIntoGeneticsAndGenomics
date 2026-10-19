---
title: rename a bunch of files with bash by regular expression
author: Ming Tang
date: '2014-08-14T07:44:00-07:00'
lastmod: '2014-08-14T07:44:00-07:00'
slug: rename-a-bunch-of-files-with-bash-by-regular-expression
tags:
- bash
- FASTQ
- unix
- sed
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/08/rename-bunch-of-files-with-bash-by.html
description: I am at the MSU NGS 2014 course. My colleague wanted to follow the khmer protocol with his own data, but one of the steps has to use a certain file name…
summary: I am at the MSU NGS 2014 course. My colleague wanted to follow the khmer protocol with his own data, but one of the steps has to use a certain file name…
header:
  caption: ''
  image: ''
---

I am at the MSU NGS 2014 course. My colleague wanted to follow the [khmer](http://khmer-protocols.readthedocs.org/en/ngs2014/index.html) protocol with his own data, but one of the steps has to use a certain file name convention.

In the protocol it requires fastq files listed as:   \*001_R1.fastq.gz\
001 is the replicate number, it can be 002 or 003 or any number of replicates you have. ( for RNA-seq, sequence as many as biological samples as possible !)\
R1 is the pair-end reads 1, it can be R2

What he has is something like:\
1_egg_r1_01_sub.fastq.gz

1 is the stage of the egg. He sequenced  4 eggs, so he has 1_egg, 2_egg., 3_egg and 4_egg\
r1 is the pair-end reads 1\
01 is the first replicates. He has two replicates for each egg.

Basically, he wants to rename these files to the khmer convention.

This problem gets down to writing a regular expression.\
To recapture the problem, I made some dummy files:

`mkdir foo && cd foo`

I have a txt file contains the names of the file:

`foo$ cat files.txt`\
`1egg_r1_01_sub.fastq.gz`\
`1egg_r2_01_sub.fastq.gz`\
`1egg_r1_02_sub.fastq.gz`\
`1egg_r2_02_sub.fastq.gz`\
`2egg_r1_01_sub.fastq.gz`\
`2egg_r2_01_sub.fastq.gz`\
`2egg_r1_02_sub.fastq.gz`\
`2egg_r2_02_sub.fastq.gz`\
`3egg_r1_01_sub.fastq.gz`\
`3egg_r2_01_sub.fastq.gz`\
`3egg_r1_02_sub.fastq.gz`\
`3egg_r2_02_sub.fastq.gz`\
`4egg_r1_01_sub.fastq.gz`\
`4egg_r2_01_sub.fastq.gz`\
`4egg_r1_02_sub.fastq.gz`\
`4egg_r2_02_sub.fastq.gz`

Now I want to make dummy files with the names in this file.\
one can make the dummy files in a fly also.

=====update on 08/26/14======\
one can use the {} expansion to create the dummy files

`tommy@tommy-ThinkPad-T420[foo] touch {1,2,3,4}_r{1,2}_0{1,2}_sub.fastq.gz`\
`tommy@tommy-ThinkPad-T420[foo] ls [ 3:45PM]`\
`1_r1_01_sub.fastq.gz 2_r2_01_sub.fastq.gz 4_r1_01_sub.fastq.gz`\
`1_r1_02_sub.fastq.gz 2_r2_02_sub.fastq.gz 4_r1_02_sub.fastq.gz`\
`1_r2_01_sub.fastq.gz 3_r1_01_sub.fastq.gz 4_r2_01_sub.fastq.gz`\
`1_r2_02_sub.fastq.gz 3_r1_02_sub.fastq.gz 4_r2_02_sub.fastq.gz`\
`2_r1_01_sub.fastq.gz 3_r2_01_sub.fastq.gz`\
`2_r1_02_sub.fastq.gz 3_r2_02_sub.fastq.gz`

========================

{{< gist crazyhottommy e8984722bf6a13191030 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/e8984722bf6a13191030)

The difference of make_dummy_file.sh and make_dummy_file_1.sh is that I specified shebang line in the make_dummy_file.sh script to tell the bash that it is a bash script, to invoke it: ./make_dummy_file.sh files.txt

In contrast, to invoke the other two which I did not specify the shebang: bash make_dummy_file_1.sh bash make_dummy_file_2.sh

Rename the files with regular expression by either using sed or rename command

{{< gist crazyhottommy 50a3c5f723a42a56885d >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/50a3c5f723a42a56885d)

the rename command use the perl regular expression. use \\ to escape $.\
the sed command need to escape the () which are used to capture the back reference\
before:\
`tommy@tommy-ThinkPad-T420:~/foo$ ls 1_egg_r1_01_sub.fastq.gz 2_egg_r1_01_sub.fastq.gz 3_egg_r1_01_sub.fastq.gz 4_egg_r1_01_sub.fastq.gz copy make_dummy_file_1.sh 1_egg_r1_02_sub.fastq.gz 2_egg_r1_02_sub.fastq.gz 3_egg_r1_02_sub.fastq.gz 4_egg_r1_02_sub.fastq.gz dummy make_dummy_file_2.sh 1_egg_r2_01_sub.fastq.gz 2_egg_r2_01_sub.fastq.gz 3_egg_r2_01_sub.fastq.gz 4_egg_r2_01_sub.fastq.gz files.txt rename.sh 1_egg_r2_02_sub.fastq.gz 2_egg_r2_02_sub.fastq.gz 3_egg_r2_02_sub.fastq.gz 4_egg_r2_02_sub.fastq.gz make_dummy_file.sh rename_one_liner.sh`

after:\
`tommy@tommy-ThinkPad-T420:~/foo$ ls 1egg_R1_001.fastq.gz 2egg_R1_001.fastq.gz 3egg_R1_001.fastq.gz 4egg_R1_001.fastq.gz copy make_dummy_file_1.sh 1egg_R1_002.fastq.gz 2egg_R1_002.fastq.gz 3egg_R1_002.fastq.gz 4egg_R1_002.fastq.gz dummy make_dummy_file_2.sh 1egg_R2_001.fastq.gz 2egg_R2_001.fastq.gz 3egg_R2_001.fastq.gz 4egg_R2_001.fastq.gz files.txt rename.sh 1egg_R2_002.fastq.gz 2egg_R2_002.fastq.gz 3egg_R2_002.fastq.gz 4egg_R2_002.fastq.gz make_dummy_file.sh rename_one_liner.sh`

`References:`<http://stackoverflow.com/questions/399078/what-special-characters-must-be-escaped-in-regular-expressions>

<http://stackoverflow.com/questions/10929453/bash-scripting-read-file-line-by-line>\
<https://www.cs.tut.fi/~jkorpela/perl/regexp.html>
