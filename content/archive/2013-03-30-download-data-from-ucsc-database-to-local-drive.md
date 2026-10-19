---
title: download data from UCSC database to local drive
author: Ming Tang
date: '2013-03-30T16:13:00-07:00'
lastmod: '2013-03-30T16:13:00-07:00'
slug: download-data-from-ucsc-database-to-local-drive
tags:
- mysql
- UCSC
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/03/download-data-from-ucsc-database-to.html
description: I wanted to download UCSC refGene table to my local computer.
summary: I wanted to download UCSC refGene table to my local computer.
header:
  caption: ''
  image: ''
---

I wanted to download UCSC refGene table to my local computer.  <http://genome.ucsc.edu/goldenPath/help/mysql.html>

However, it looks like it is not possible to direct pull data from UCSC to a local host Database.\
on linux command line, `mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A`\
`SHOW Databases;`\
`USE hg19;`\
`SHOW tables like "%ref%";`

`+------------------------+`

`| Tables_in_hg19 (%ref%) |`\
`+------------------------+`

``` text
  | kgXref                 |
  | kgXrefOld5             |
  | refFlat                |
  | refGene                |
  | refLink                |
  | refSeqAli              |
  | refSeqStatus           |
  | refSeqSummary          |
  +------------------------+
  8 rows in set (1.31 sec)

   select * from refGene into outfile "/export.txt";
  ERROR 1045 (28000): Access denied for user 'genome'@'%' (using password: NO)

  I got this error, and I googled it found that
  "The SELECT ... INTO OUTFILE 'file_name' form of SELECT writes the selected
```

``` text
rows to a file. The file is created on the server host, so you must have
the FILE privilege to use this syntax.

If you want to create the resulting file on some client host other than
the server host, you cannot use SELECT ... INTO OUTFILE. In that case, you
should instead use a command such as

mysql -e "SELECT ..." >  file_name
```

``` text
to generate the file on the client host." https://lists.soe.ucsc.edu/pipermail/genome/2009-August/019892.html
```

``` text
So, instead on linux command line, I typed :
```

``` text
mysql --user=genome --host=genome-mysql.cse.ucsc.edu hg19 -A -sre 'SELECT * from refGene' > ~/Desktop/refGene.hg19
```

`and now it works!`
