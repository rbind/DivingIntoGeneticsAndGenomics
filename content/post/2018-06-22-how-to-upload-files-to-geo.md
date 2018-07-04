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


### 1. create account

Go to NCBI GEO: http://www.ncbi.nlm.nih.gov/geo/
Create User ID and password. my username is `research_guru`

I used my google account.

### 2. fill in the xls sheet 

Downloaded the meta xls sheet from https://www.ncbi.nlm.nih.gov/geo/info/seq.html  

```bash
## bgzip the fastqs

cd 01seq
find *fastq | parallel bgzip
md5sum *fastq.gz > fastq_md5.txt 
# copy to excle
cat fastq_md5.txt | awk '{print $2}'

#copy to excle
cat fastq_md5.txt | awk '{print $1}'


cd ../07bigwig
#get the md5sum

md5sum *bw > bigwig_md5.txt

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

This is the most time-consuming/tedious step.

### 3. hard link peak and bigwig files to the folder

soft link does not work for me...
```
ln  /rsrch2/genomic_med/krai/hunain_histone_reseq/snakemake_ChIPseq_pipeline_downsample/07bigwig/*bw .


ln /rsrch2/genomic_med/krai/hunain_histone_reseq/snakemake_ChIPseq_pipeline_downsample/08peak_macs1/*macs1_peaks.bed .
```

### 4. upload to GEO

```bash

# inside the folder: research_guru_KMT2D_ChIPseq
ftp ftp-private.ncbi.nlm.nih.gov

## type in the user name and the password
## this is not your GEO account user name.
## everyone uses the same `geo` and the same password below.

# https://www.ncbi.nlm.nih.gov/geo/info/submissionftp.html

#name: geo
#password: 33%9uyj_fCh?M16H

ftp> prompt n
Interactive mode off.

ftp> cd fasp

# make a folder in the ftp site
ftp> mkdir research_guru_ChIPseq

ftp> cd research_guru_ChIPseq

#upload all the files
ftp> mput *
```

### 5. telling NCBI you uploaded stuff
After your transfer is complete, you need to tell the NCBI.

After file transfer is complete, please e-mail GEO with the following information: - GEO account username (tangming2005@gmail.com); - Names of the directory and files deposited; - Public release date (required - up to 3 years from now - see FAQ).

### Side notes
for paired-end sequencing data. the xls sheet requires you to fill in the average insert size and the std.

`picard CollectInsertSizeMetrics` can do this job. 

```bash
time java -jar /scratch/genomic_med/apps/picard/picard-tools-2.13.2/picard.jar CollectInsertSizeMetrics I=4-Mll4-RasG12D-1646-2-cd45_S40_L006.sorted.bam  H=4-Mll4-RasG12D-1646-2-cd45_S40_L006_insert.pdf  O=4-Mll4-RasG12D-1646-2-cd45_S40_L006_insert.txt

# finish in ~5mins
```

read http://thegenomefactory.blogspot.com/2013/08/paired-end-read-confusion-library.html for insert size definition.