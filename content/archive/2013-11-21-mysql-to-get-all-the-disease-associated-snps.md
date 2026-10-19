---
title: mysql to get all the disease associated SNPs
author: Ming Tang
date: '2013-11-21T13:05:00-08:00'
lastmod: '2013-11-21T13:05:00-08:00'
slug: mysql-to-get-all-the-disease-associated-snps
tags:
- bedtools
- ChIP-seq
- mysql
- UCSC
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/11/mysql-to-get-all-disease-associated-snps.html
description: I want to find SNPs occur at the transcription binding sites which may potentially affect the binding of the transcription factor.
summary: I want to find SNPs occur at the transcription binding sites which may potentially affect the binding of the transcription factor.
header:
  caption: ''
  image: ''
---

I want to find SNPs occur at the transcription binding sites which may potentially affect the binding of the transcription factor.

I need  to have a list of SNPs that are associated with diseases first.\
people usually go to dbSNP <http://www.ncbi.nlm.nih.gov/SNP/>\
but the UCSC snp138 table hg19 is much better accessible in terms of parsing.

See posts here: <http://www.biostars.org/p/1288/>\
<http://www.biostars.org/p/7073/>\
<http://www.biostars.org/p/11701/>

find SNPs associated with OMIM gene  (by [Pierre ](http://www.biostars.org/u/30/))

" Inspired by Khader's comment. The following mysql query for the mysql anonymous server at UCSC answers the SNPs in the OMIM genes:

``` text
mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A  -D hg18 -e '
select
   concat(left(title1,30),"..."),
   omimId,
   S.name,
   S.func,
   G.chrom,
   S.chromStart,
   S.chromEnd
from
   omimGene as G,
   omimGeneMap as M,
   snp130 as S
where
  G.name=M.omimId and
  G.chrom=S.chrom and
  S.chromStart>=G.chromStart and
  S.chromEnd <= G.chromEnd
limit 10;'
```

Result:

``` text
+-----------------------------------+--------+------------+--------------------+-------+------------+----------+
| concat(left(title1,30),"...")     | omimId | name       | func               | chrom | chromStart | chromEnd |
+-----------------------------------+--------+------------+--------------------+-------+------------+----------+
| Nucleolar complex-associated p... | 610770 | rs72904505 | untranslated-3     | chr1  |     869480 |   869481 |
| Nucleolar complex-associated p... | 610770 | rs6605067  | untranslated-3     | chr1  |     869538 |   869539 |
| Nucleolar complex-associated p... | 610770 | rs2839     | untranslated-3     | chr1  |     869549 |   869550 |
| Nucleolar complex-associated p... | 610770 | rs3196153  | untranslated-3     | chr1  |     869586 |   869587 |
| Nucleolar complex-associated p... | 610770 | rs1133980  | untranslated-3     | chr1  |     869614 |   869615 |
| Nucleolar complex-associated p... | 610770 | rs28453979 | untranslated-3     | chr1  |     869781 |   869782 |
| Nucleolar complex-associated p... | 610770 | rs61551591 | intron,near-gene-3 | chr1  |     870079 |   870080 |
| Nucleolar complex-associated p... | 610770 | rs3748592  | intron,near-gene-3 | chr1  |     870100 |   870101 |
| Nucleolar complex-associated p... | 610770 | rs3748593  | intron,near-gene-3 | chr1  |     870252 |   870253 |
| Nucleolar complex-associated p... | 610770 | rs74047418 | missense           | chr1  |     870364 |   870365 |
+-----------------------------------+--------+------------+--------------------+-------+------------+----------+
```

however the table is not available any more in the UCSC databases.\
instead you should do:\
also  by [Pierre ](http://www.biostars.org/u/30/)

1\) Register an access to the FTP site of omim: <http://omim.org/downloads> and download mim2gene:

``` text
$ curl -s  "ftp://anonymous:xxxxxxx@xxxxx.edu/OMIM/mim2gene.txt" | head
# Mim Number    Type    Gene IDs    Approved Gene Symbols
100050  phenotype   -   -
100070  phenotype   100329167   -
100100  phenotype   -   -
100200  phenotype   -   -
100300  phenotype   100188340   -
100500  moved/removed   -   -
100600  phenotype   -   -
100640  gene    216 ALDH1A1
100650  gene/phenotype  217 ALDH2
```

get a list of the gene symbols:

``` text
~$ curl -s  "ftp://anonymous:xxxxx@xxxxxx.edu/OMIM/mim2gene.txt" |\
   egrep -v "#" | cut -d '  ' -f 4 | egrep -v '^\-$' |\
   sort | uniq > list1.txt
```

2\) get your list of SNP associiated to the gene symbol. Something like:

``` text
mysql -N --user=genome --host=genome-mysql.cse.ucsc.edu -A  -D hg19 -e 'select  distinct
  G.geneSymbol,
  S.name
from snp132 as S,
kgXref as G,
knownGene as K where
    S.chrom=K.chrom and
    S.chromStart>=K.txStart and
    S.chromEnd<=K.txEnd and
    K.name=G.kgId
    /* AND something to restrict the result to YOUR list of SNPs or gene */
' | sort -t '    ' -k1,1 > list2.txt
```

3\) use unix join to join the two lists:

``` text
join -1 1 -2 1 list1.txt list2.txt
```

you should get a list with two columns: the OMIM gene and your SNP.

I want to get a list of SNPs that are clinical associated. description of the snp138 table\
<http://genome.ucsc.edu/cgi-bin/hgTables>\
at terminal:

`mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A -D hg19 -e ' SELECT * FROM snp138 AS s WHERE s.bitfields LIKE "clin%" ' > clinic_associated_SNPs_hg19.txt`

remember you are on the remote server of UCSC, you can not connect the database first, and do something like:

``` text
SELECT *
FROM snp138 AS s
```

``` text
WHERE s.bitfields LIKE "clin%"
INTO OUTFILE '/home/tommy/clinic_associated_SNPs_hg.txt'
FIELDS TERMINATED BY '\t'
LINES TERMINATED BY '\n';
```

Because we do not have the write privilege\
after I got the txt file, I  changed it to a bed file:

`tommy@tommy-ThinkPad-T420:~$ cat clinic_associated_SNPs_hg19.txt | cut -f2-7 | head`\
`chrom chromStart chromEnd name score strand`\
`chr1 985954 985955 rs199476396 0 +`\
`chr1 1199488 1199489 rs207460006 0 +`\
`chr1 1245103 1245104 rs144003672 0 +`\
`chr1 1265153 1265154 rs307355 0 +`\
`chr1 1265459 1265460 rs35744813 0 -`\
`chr1 1469330 1469331 rs145324009 0 +`\
`chr1 1635334 1635335 rs201004006 0 +`\
`chr1 1689555 1689556 rs207460007 0 +`\
`chr1 1959074 1959075 rs121434580 0 +`

`tommy@tommy-ThinkPad-T420:~$ wc -l clinic_associated_SNPs_hg19.txt`\
`103174 clinic_associated_SNPs_hg19.txt`

There are total 103174 SNPs in the file.\
Now, I can just use bedtools to intersect the SNPs bed file with the ChIP-seq bed file generated by MACS.

`tommy@tommy-ThinkPad-T420:~$ bedtools intersect -a TF.bed -b SNPs_hg19.bed -wo`

Just keep in mind the difference between the 0 based and 1 based coordinates system. See a post here:\
<http://www.biostars.org/p/84686/>

Alternatively, you can create a database to do this kind of intersection by using Join command.\
<http://gettinggeneticsdone.blogspot.com/2013/11/using-database-joins-to-compare-results.html>\
<http://www.sitepoint.com/understanding-sql-joins-mysql-database/>

other tools\
regulomeDB\
<http://regulome.stanford.edu/>

a quick google\
 <http://www.genomatix.de/online_help/help_gems/SNPInspector.html>\
<http://viis.abdn.ac.uk/regsnp/Home.aspx>

Finally a python package for Database <https://dataset.readthedocs.org/en/latest/quickstart.html>
