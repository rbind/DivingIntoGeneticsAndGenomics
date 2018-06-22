---
title: How to upload files to GEO
author: Ming Tang
date: '2018-06-22'
slug: how-to-upload-files-to-geo
categories:
  - genomics
tags:
  - GEO
header:
  caption: ''
  image: ''
---

### readings 

links: 
http://yeolab.github.io/onboarding/geo.html  
http://www.hildeschjerven.net/Protocols/Submission_of_HighSeq_data_to_GEO.pdf  
https://www.ncbi.nlm.nih.gov/geo/info/submissionftp.html  


### 1. create an account in GEO

Goto NCBI GEO: http://www.ncbi.nlm.nih.gov/geo/
Create User ID and password : Will assume "YOUR_NAME" as user id below 

I used my google account.

### 2. fill in the xls sheet 

Downloaded the meata xls sheet from https://www.ncbi.nlm.nih.gov/geo/info/seq.html  

```bash
## bgzip the fastqs

cd 01seq
find *fastq | parallel bgzip
md5sum *fastq.gz > fastq_md5.txt 
cat fastq_md5.txt | awk '{print $2}'
cat fastq_md5.txt | awk '{print $1}'


cd ../07bigwig
#get the md5sum

md5sum *bw | grep -v "mean" > bigwig_md5.txt

# sample name, copy to excel
cat bigwig_md5.txt | awk '{print $2}'

# md5, copy to excel
cat bigwig_md5.txt | awk '{print $1}'

cd ../08peak_macs1

md5sum *macs1_peaks.bed > peaks_md5.txt
# copy to excel
cat peaks_md5.txt | awk '{print $2}'

cd ..
mkdir research_guru_KMT2D_ChIPseq

cd research_guru_KMT2D_ChIPseq

## fill in the xls sheet and save in this folder
```



### 3. soft link peak and bigwig files to the folder
```
ln -s /rsrch2/genomic_med/krai/hunain_histone_reseq/snakemake_ChIPseq_pipeline_downsample/07bigwig/*bw .


ln -s /rsrch2/genomic_med/krai/hunain_histone_reseq/snakemake_ChIPseq_pipeline_downsample/08peak_macs1/*macs1_peaks.bed .
```

### 4. upload to GEO

```bash

# inside the folder: research_guru_KMT2D_ChIPseq
ftp ftp-private.ncbi.nlm.nih.gov

ftp> prompt n
Interactive mode off.

ftp> cd fasp

ftp> mkdir research_guru_ChIPseq
```
