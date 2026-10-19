---
title: mysql rocks
author: Ming Tang
date: '2013-11-19T17:18:00-08:00'
lastmod: '2013-11-19T17:18:00-08:00'
slug: mysql-rocks
tags:
- database
- mysql
- UCSC
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/11/mysql-rocks.html
description: I just finished the first chapter of MySQL by Paul DuBois \ I was really excited to explore the power of mysql. Just by following the command examples of…
summary: I just finished the first chapter of MySQL by Paul DuBois \ I was really excited to explore the power of mysql. Just by following the command examples of…
header:
  caption: ''
  image: ''
---

I just finished the first chapter of MySQL by Paul DuBois <http://www.amazon.com/MySQL-5th-Edition-Developers-Library-ebook/dp/B00C2SFK2Q>\
I was really excited to explore the power of mysql. Just by following the command examples of  the two sample databases in the book, I learned a lot.

I am studying biology, so, I want to make practical use of mysql. One of the databases on top of my mind is the UCSC genome browser database <http://genome.ucsc.edu/goldenPath/help/mysql.html>

connect to the database:\
`tommy@tommy-ThinkPad-T420:~$ mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A`

`mysql> show databases;`

`+--------------------+`\
`| Database |`\
`+--------------------+`\
`| information_schema |`\
`| ailMel1 |`\
`| allMis1 |`\
`| anoCar1 |`\
`| anoCar2 |`\
`| anoGam1 |`\
`| apiMel1 |`

............................\
............................

`mysql> use hg19`\
`Database changed`

find the hg19 refGene annotation table

`mysql> show tables like "%ref%";`\
`+------------------------+`\
`| Tables_in_hg19 (%ref%) |`\
`+------------------------+`\
`| kgXref |`\
`| kgXrefOld5 |`\
`| kgXrefOld6 |`\
`| refFlat |`\
`| refGene |`\
`| refLink |`\
`| refSeqAli |`\
`| refSeqStatus |`\
`| refSeqSummary |`\
`+------------------------+`\
`9 rows in set (0.12 sec)`

`mysql> describe refGene;`\
`+--------------+------------------------------------+------+-----+---------+-------+`\
`| Field | Type | Null | Key | Default | Extra |`\
`+--------------+------------------------------------+------+-----+---------+-------+`\
`| bin | smallint(5) unsigned | NO | | NULL | |`\
`| name | varchar(255) | NO | MUL | NULL | |`\
`| chrom | varchar(255) | NO | MUL | NULL | |`\
`| strand | char(1) | NO | | NULL | |`\
`| txStart | int(10) unsigned | NO | | NULL | |`\
`| txEnd | int(10) unsigned | NO | | NULL | |`\
`| cdsStart | int(10) unsigned | NO | | NULL | |`\
`| cdsEnd | int(10) unsigned | NO | | NULL | |`\
`| exonCount | int(10) unsigned | NO | | NULL | |`\
`| exonStarts | longblob | NO | | NULL | |`\
`| exonEnds | longblob | NO | | NULL | |`\
`| score | int(11) | YES | | NULL | |`\
`| name2 | varchar(255) | NO | MUL | NULL | |`\
`| cdsStartStat | enum('none','unk','incmpl','cmpl') | NO | | NULL | |`\
`| cdsEndStat | enum('none','unk','incmpl','cmpl') | NO | | NULL | |`\
`| exonFrames | longblob | NO | | NULL | |`\
`+--------------+------------------------------------+------+-----+---------+-------+`\
`16 rows in set (0.54 sec)`

which gene has the most exons?

`mysql> select chrom, name, name2, exonCount from refGene order by exonCount DESC limit 10;`\
`+-------+--------------+-------+-----------+`\
`| chrom | name | name2 | exonCount |`\
`+-------+--------------+-------+-----------+`\
`| chr2 | NM_001267550 | TTN | 363 |`\
`| chr2 | NM_001256850 | TTN | 313 |`\
`| chr2 | NM_133378 | TTN | 312 |`\
`| chr2 | NM_133437 | TTN | 192 |`\
`| chr2 | NM_133432 | TTN | 192 |`\
`| chr2 | NM_003319 | TTN | 191 |`\
`| chr2 | NM_001271208 | NEB | 183 |`\
`| chr2 | NM_001164507 | NEB | 182 |`\
`| chr2 | NM_001164508 | NEB | 182 |`\
`| chr12 | NM_173600 | MUC19 | 173 |`\
`+-------+--------------+-------+-----------+`

I want the genes rather than the transcripts, so I group them by gene name (name2)

`mysql> select chrom, name, name2, max(exonCount) AS maximum from refGene group by name2 order by maximum DESC limit 10;`\
`+-------+--------------+--------------+---------+`\
`| chrom | name | name2 | maximum |`\
`+-------+--------------+--------------+---------+`\
`| chr2 | NM_001256850 | TTN | 363 |`\
`| chr2 | NM_001271208 | NEB | 183 |`\
`| chr12 | NM_173600 | MUC19 | 173 |`\
`| chr6 | NM_033071 | SYNE1 | 146 |`\
`| chr1 | NM_001278267 | LOC100288142 | 131 |`\
`| chr3 | NM_000094 | COL7A1 | 118 |`\
`| chr14 | NM_182914 | SYNE2 | 116 |`\
`| chr1 | NM_001098623 | OBSCN | 116 |`\
`| chr7 | NM_198455 | SSPO | 110 |`\
`| chr1 | NM_031935 | HMCN1 | 107 |`\
`+-------+--------------+--------------+---------+`\
`10 rows in set (2.53 sec)`\
sanity check on UCSC genome browser:

![](/img/archive/mysql-rocks/01-selection_038.png)

Just curious, which gene has the longest transcript?

`mysql> select chrom, name, name2, txEnd-txStart AS span from refGene order by span DESC limit 10;`\
`+-------+--------------+--------------+---------+`\
`| chrom | name | name2 | span |`\
`+-------+--------------+--------------+---------+`\
`| chr1 | NM_001278267 | LOC100288142 | 2320934 |`\
`| chr7 | NM_014141 | CNTNAP2 | 2304636 |`\
`| chr9 | NM_002839 | PTPRD | 2298478 |`\
`| chrX | NM_000109 | DMD | 2220382 |`\
`| chr11 | NM_001142699 | DLG2 | 2172259 |`\
`| chrX | NM_004006 | DMD | 2092329 |`\
`| chr8 | NM_033225 | CSMD1 | 2059454 |`\
`| chr20 | NM_080676 | MACROD2 | 2057696 |`\
`| chrX | NM_004009 | DMD | 2009201 |`\
`| chrX | NM_004010 | DMD | 2009200 |`\
`+-------+--------------+--------------+---------+`\
`10 rows in set (0.40 sec)`

![](/img/archive/mysql-rocks/02-selection_039.png)

![](/img/archive/mysql-rocks/03-selection_037.png)

These two genes span more than 2Mb!

of course, one can download the refGene table to local computer and use awk  etc to do this kind of work, but I just see how powerful and convenient mysql is especially when you have multiple tables and want to do some summarization across them.
