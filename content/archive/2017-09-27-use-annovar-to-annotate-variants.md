---
title: Use annovar to annotate variants
author: Ming Tang
date: '2017-09-27T19:25:00-07:00'
lastmod: '2017-09-27T19:25:00-07:00'
slug: use-annovar-to-annotate-variants
tags:
- annovar
- SNV
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/09/use-annovar-to-annotate-variants.html
description: Annovar is one of the widely used variants annotation tools. It was cited for over 2000 times which is amazing. It annotates the variants in a tabular…
summary: Annovar is one of the widely used variants annotation tools. It was cited for over 2000 times which is amazing. It annotates the variants in a tabular…
header:
  caption: ''
  image: ''
---

[Annovar](http://annovar.openbioinformatics.org/en/latest/) is one of the widely used variants annotation tools. It was cited for over 2000 times which is amazing. It annotates the variants in a tabular format which is easy to parse. Other tools such as VEP and VCFanno are alternatives.\
First download the annovar package (you will need to register and an email with downloading link will be sent to you)

``` bash
wget  http://www.openbioinformatics.org/annovar/download/0wgxR2rIVP/annovar.latest.tar.gz

tar xvzf annovar.latest.tar.gz
```

#### Download the databases

``` bash
cd annovar

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar refGene humandb/

perl annotate_variation.pl -buildver hg19 -downdb cytoBand humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar snp129 humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar exac03nontcga humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar gnomad_genome humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar cadd13 humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar clinvar_20170130 humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar cosmic70 humandb/

perl annotate_variation.pl -buildver hg19 -downdb -webfrom annovar dbnsfp33a humandb/
```

#### Annotate

First download the annovar package (you will need to register and an email with downloading link will be sent to you) only 5 columns are needed for input: chr, start, end, ref, alt. By default, it assumes the file is in 1-based format.

``` bash
perl /scratch/genomic_med/apps/annovar/annovar/table_annovar.pl patient1_pre-treatment_vs_patient1_leukocyte_recount_alt_fill.txt /scratch/genomic_med/apps/annovar/annovar/humandb/ -buildver hg19 -out patient1_pre-treatment_vs_patient1_leukocyte_recount_annovar -remove -protocol refGene,cytoBand,snp129,cosmic70,dbnsfp33a,cadd13,clinvar_20170130,exac03nontcga,gnomad_genome  -operation g,r,f,f,f,f,f,f,f -nastring NA -polish -otherinfo -thread 2

perl /scratch/genomic_med/apps/annovar/annovar/table_annovar.pl patient1_pre-treatment_vs_patient1_leukocyte_recount_alt_fill.txt /scratch/genomic_med/apps/annovar/annovar/humandb/ -buildver hg19 -out patient1_pre-treatment_vs_patient1_leukocyte_recount_annovar -remove -protocol refGene,cytoBand,snp129  -operation g,r,f -nastring NA  -polish -otherinfo
```
