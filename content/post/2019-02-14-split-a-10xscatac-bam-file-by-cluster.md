---
title: Split a 10xscATAC bam file by cluster
author: Ming Tang
date: '2019-02-14'
slug: split-a-10xscatac-bam-file-by-cluster
categories:
  - bioinformatics
  - python
tags:
  - ATACseq
  - single-cell
header:
  caption: ''
  image: ''
---

I want to split the PBMC scATAC bam from 10x by cluster id. So, I can then make a bigwig for each cluster to visualize in `IGV`.

The first thing I did was googling to see if anyone has written such a tool (Do not reinvent the wheels!). People have done that because I saw figures from the scATAC papers. I just could not find it. Maybe I need to refine my googling skills.

I decided to write one myself. The following is my journey for this small task.

download the 5k pbmc scATAC data from https://support.10xgenomics.com/single-cell-atac/datasets/1.0.1/atac_v1_pbmc_5k


### split the cell barcodes by cluster id

```bash
cd analysis/clustering/graphclust
head clusters.csv
Barcode,Cluster
AAACGAAAGCGCAATG-1,1
AAACGAAAGGGTATCG-1,4
AAACGAAAGTAACATG-1,8
AAACGAAAGTTACACC-1,1
AAACGAACAGAGATGC-1,4
AAACGAACATGCTATG-1,5
AAACGAAGTGCATCAT-1,3
AAACGAAGTGGACGAT-1,3
AAACGAAGTGGCCTCA-1,7

# there are ^M characters at the end of the line if you do cat -A you will see it.
# change it to unix
dos2unix clusters.csv
```

```bash
awk -F"," 'NR>1{print $1 >> "cluster_"$2".csv"}' clusters.csv
wc -l *csv
   330 cluster_10.csv
   322 cluster_11.csv
   258 cluster_12.csv
   191 cluster_13.csv
   608 cluster_1.csv
   563 cluster_2.csv
   559 cluster_3.csv
   532 cluster_4.csv
   483 cluster_5.csv
   425 cluster_6.csv
   366 cluster_7.csv
   360 cluster_8.csv
   338 cluster_9.csv
  5336 clusters.csv
```

### use bamtools 
```bash
time bamtools filter -tag CB:Z:AAACTGCAGAGCAGCT-1 -in atac_v1_pbmc_5k_possorted_bam.bam -out AAACTGCAGAGCAGCT-1.bam
```

This takes a little over 1 hour for one barcode! And there is no easy way
to specify a group of barcodes.

### use the linux tricks

inspired partly by this post https://www.biostars.org/p/263346/

```bash
wc -l clusters.csv
5336 clusters.csv
```

and let's see how fast each regular expression takes for `awk`

```bash
time samtools view atac_v1_pbmc_5k_possorted_bam.bam | awk -v tag="CB:Z:AAACTGCAGAGCAGCT-1" 'index($0,tag)>0' >> AAACTGCAGAGCAGCT-1.sam

real    27m14.332s
user    48m36.883s
sys     4m37.908s
```
It is not too bad, but if we loops over the `clusters.csv` files for 5335 times,

```bash
samtools view -H atac_v1_pbmc_5k_possorted_bam.bam > header.txt

cat clusters.csv \
| sed '1d' \
| while IFS=',' read -r barcode cluster
    do samtools view atac_v1_pbmc_5k_possorted_bam.bam |  awk -v tag="CB:Z:$barcode" 'index($0,tag)>0' >> "$cluster.sam"
    done

## then cat the header with the sam.

```
it will take ~30min * 5335 = ~100 days to finish.

we can do better to parallize by GNU parallel

```bash 
## not tested...
cat clusters.csv \
| sed '1d' \
| parallel --colsep ',' -j 40 'samtools view atac_v1_pbmc_5k_possorted_bam.bam |awk -v tag="CB:Z:{1}" 'index(\$0,tag)>0' >> {2}.sam'
```
Again, using 40 cores may reduce our time to 100/40 = 5 days.

### use pysam

Let's only loop over the sam file once

```python
import pysam
import csv

cluster_dict = {}
with open('clusters.csv') as csv_file:
    csv_reader = csv.reader(csv_file, delimiter=',')
    #skip header
    header = next(csv_reader)
    for row in csv_reader:
        cluster_dict[row[0]] = row[1]

clusters = set(x for x in cluster_dict.values())


fin = pysam.AlignmentFile("atac_v1_pbmc_5k_possorted_bam.bam", "rb")

# open the number of bam files as the same number of clusters, and map the out file handler to the cluster id, write to a bam with wb
fouts_dict = {}
for cluster in clusters:
    fout = pysam.AlignmentFile("cluster" + cluster + ".bam", "wb", template = fin)
    fouts_dict[cluster] = fout

for read in fin:
    tags = read.tags
    CB_list = [ x for x in tags if x[0] == "CB"]
    if CB_list:
        cell_barcode = CB_list[0][1]
    # the bam files may contain reads not in the final clustered barcodes
    # will be None if the barcode is not in the clusters.csv file
    else: 
        continue
    cluster_id = cluster_dict.get(cell_barcode)
    if cluster_id:
        fouts_dict[cluster_id].write(read)

## do not forget to close the files
fin.close()
for fout in fouts_dict.values():
    fout.close()
```

real    172m58.758s
user    172m10.678s
sys     0m46.071s

Note, some read record in the bam file do not have `CB` but only `CR`.

from https://support.10xgenomics.com/single-cell-atac/software/pipelines/latest/output/bam

| Tag      | Description |
| ----------- | ----------- |
| CB    | Chromium cellular barcode sequence that is error-corrected and confirmed against a list of known-good barcode sequences.       |
| CR   | Chromium cellular barcode sequence as reported by the sequencer.        |




How many of those reads with `CR`? 

```bash
# every read has a CR tag
samtools view atac_v1_pbmc_5k_possorted_bam.bam| grep -v "CR" | wc -l
0 

# not every read has a CB tag.
samtools view atac_v1_pbmc_5k_possorted_bam.bam| grep -v "CB" | wc -l
10647804

```
### use pybam

https://www.biostars.org/p/186732/

https://github.com/JohnLonginotto/pybam

Note that pybam is python2.x

```bash
source activate py27
cd ~/apps
git clone https://github.com/JohnLonginotto/pybam
```
inside python:

```python
import pybam
import csv

cluster_dict = {}
with open('clusters.csv') as csv_file:
    csv_reader = csv.reader(csv_file, delimiter=',')
    #skip header
    header = next(csv_reader)
    for row in csv_reader:
        cluster_dict[row[0]] = row[1]

clusters = set(x for x in cluster_dict.values())


# open the number of bam files as the same number of clusters, and map the out file handler to the cluster id

header = pybam.read('atac_v1_pbmc_5k_possorted_bam.bam').file_header
fouts_dict = {}
for cluster in clusters:
    fout = open("cluster" + cluster + ".sam", "w")
    fout.write(header)
    fouts_dict[cluster] = fout

for read in pybam.read('possorted_bam.bam'):
        ## not always the same position in the list for the CB tag
        ## there could be no CB tag for a certian read as well
        ## it will return empty list
        CB_list = [ x for x in read.sam_tags_list if x[0] == "CB"]
        if CB_list:
            cell_barcode = CB_list[0][2]
            cluster_id = cluster_dict.get(cell_barcode)
            if cluster_id:
                fouts_dict[cluster_id].write(read.sam + '\n')
        
## do not forget to close the files
for fout in fouts_dict.values():
    fout.close()
```
real    1262m30.849s
user    1240m11.906s
sys     22m9.325s

Did not find how to write to a bam file, so I have to write to a sam file. I asked on github issues but no responses. The author is not actively maintaining the library anymore.

### use hts-nim 

https://github.com/brentp/hts-nim-tools/issues/5

Thanks Brent for providing the code.

#### htslib need to be in `$LD_LIBRARY_PATH`:

```bash
wget https://github.com/samtools/htslib/releases/download/1.6/htslib-1.6.tar.bz2
tar xjf htslib-1.6.tar.bz2
cd htslib-1.6
./configure ~/bin/

make

# add this to .bashrc and source ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/n/home02/mtang/apps/htslib-1.6
```

#### install `nim` and `hts-nim`
```bash
wget https://raw.githubusercontent.com/brentp/hts-nim/master/scripts/simple-install.sh

chmod u+x simple-install.sh
./simple-install.sh

# add nim to PATH

git clone https://github.com/brentp/hts-nim
cd hts-nim
nimble install -y
```


```python
import hts
import os
import strutils
import tables
var ibam:Bam

# lookup from cb -> cluster
var clusterTbl = initTable[string,string]()
# lookup from cluster -> bam
var tbl = initTable[string, Bam]()

for x in paramStr(1).lines:
  var toks = x.strip().split(",")
  clusterTbl[toks[0]] = toks[1]

if not open(ibam, paramStr(2)):
   quit "couldn't open bam"

for aln in ibam:
  var cb = tag[string](aln, "CB").get
  if cb.isNullOrEmpty: continue
  if cb notin clusterTbl: continue
  var cluster = clusterTbl[cb]
  if cluster notin tbl:
    var obam: Bam
    if not open(obam, "out-cluster-" & cluster & ".bam", mode="w"):
      quit "couldn't open bam for writing"
    obam.write_header(ibam.hdr)
    tbl[cluster] = obam
  tbl[cluster].write(aln)

for k, bam in tbl:
  bam.close()
ibam.close()
```
#### compile

save it to `split_scATAC_bam.nim` and compile:

```bash
nim compile -d:release scATAC_split_scATAC_bam.nim
split_scATAC_bam clusters.csv atac_v1_pbmc_5k_possorted_bam.bam
```

real    105m17.140s
user    102m17.214s
sys     2m58.312s

it is 172/105 **~1.6 times faster** in `hts-nim` than in `pysam`.
### speed up

* parallize by chromosome 
* pysam parallization
* `hts-nim` from Brent:

> you can add `threads=2` (or 3) to the `open` calls to get a bit more speed on de/compressing the bam which will be the most CPU time

* C htslib, I expect the speed will be similar to `hts-nim` since `hts-nim` is a wrapper around it.

### Lessons learned

I had [a bug](https://github.com/brentp/hts-nim-tools/issues/5#issuecomment-464114496) in my `pysam` code and it pulls out some reads without the `CB` tag. Thanks Brent for catching it. I spent some time to debug and could not find it. 

Lessons that I have learned:

* How to make sure the output of the software is correct is very difficult. unit testing is important.
* It is good to have someone else with more programming experience to look at the code for you. You are so used to the code that you write and can not find the "obvious" problem. 
* Do not use libraries that are not well maintained. The `pybam` author is not maintaining the library now and it is written in python2.x. I am writing all my python code in python3.x