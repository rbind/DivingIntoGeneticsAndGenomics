---
title: SRA file and my first Bowtie run on UF HPC (high performance computing center)
author: Ming Tang
date: '2013-05-07T09:30:00-07:00'
lastmod: '2013-05-07T09:30:00-07:00'
slug: sra-file-and-my-first-bowtie-run-on-uf-hpc-high-performance-computing-center
tags:
- bowtie
- SRA
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/sra-file-and-my-first-bowtie-run-on-uf.html
description: I wanted to have a look at a certain ChIP-seq data in MCF7 breast cancer cells.
summary: I wanted to have a look at a certain ChIP-seq data in MCF7 breast cancer cells.
header:
  caption: ''
  image: ''
---

I wanted to have a look at a certain ChIP-seq data in MCF7 breast cancer cells.

I could not find any data sets in UCSC which are in bam file format.\
then, I found a sra file in  NCBI sequence read archive:\
<http://www.ncbi.nlm.nih.gov/sra>

I downloaded the sra file from there, next question would be : how to convert it to sam or bam file?

It looks like SAR toolkit <http://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std#header-global>can directly convert sra to bam file combined with samtools.<http://samtools.sourceforge.net/>

download the sra toolkit and install it following the instruction, in a terminal redirect to the bin directory

`./sam-dump SRR390728 | samtools view -Sb -o my_bam.bam -`

it can convert the sra to bam file, but will have a warning saying " \[samopen\] no @SQ lines in the header."

the header in a "regular"  sam file looks like this:

@HD VN:1.0 SO:unsorted\
@SQ SN:chr1 LN:249250621\
@SQ SN:chr2 LN:243199373\
@SQ SN:chr3 LN:198022430\
@SQ SN:chr4 LN:191154276\
@SQ SN:chr5 LN:180915260\
@SQ SN:chr6 LN:171115067\
@SQ SN:chr7 LN:159138663\
@SQ SN:chr8 LN:146364022\
@SQ SN:chr9 LN:141213431\
@SQ SN:chr10 LN:135534747\
@SQ SN:chr11 LN:135006516\
@SQ SN:chr12 LN:133851895\
@SQ SN:chr13 LN:115169878\
@SQ SN:chr14 LN:107349540\
@SQ SN:chr15 LN:102531392\
@SQ SN:chr16 LN:90354753\
@SQ SN:chr17 LN:81195210\
@SQ SN:chr18 LN:78077248\
@SQ SN:chr19 LN:59128983\
@SQ SN:chr20 LN:63025520\
@SQ SN:chr21 LN:48129895\
@SQ SN:chr22 LN:51304566\
@SQ SN:chrX LN:155270560\
@SQ SN:chrM LN:16571\
@RG ID:D103GACXX_L7_GSLv2-7_04 SM: LB:SL13484 PL:ILLUMINA

what I did next is : I first converted the sra to sam file, then cut the header from a different sam file then "cat" them together. 

I loaded the sam file into SeqMonk <http://www.bioinformatics.babraham.ac.uk/projects/seqmonk/>

after reading the lines, and saying caching... the data did not show up in the "data set" track on the left.

it turned out that the sra files contain unmapped raw sequences, and SeqMonk basically just ignored the lines from sam file which is derived from the sra file.

Well, I think I need to convert the sra to fastq file first (use fastq-dump in sra toolkit), and map it to hg19 with bowtie.\
<http://bowtie-bio.sourceforge.net/index.shtml>

the fastq file is around 6GB, and my computer is not powerful enough to handle such big file.

So, I decided to use the hpc in UFL\
<http://wiki.hpc.ufl.edu/doc/UF_HPC_Center>

it has bowtie installed, and ready to go!

first need to copy the fastq file to hpc, use scp command in a local terminal:

`scp my_file.fastq username@submit.hpc.ufl.edu:/scratch/hpc/mtang/`

log in the remote host server:\
`ssh username@submit.hpc.ufl.edu`

redirect to /scratch/hpc/mtang

make a bowtie_test.pbs file there.

`cat bowtie_test.pbs`

-------------------------------------

\#!/bin/bash\
\#\
\#PBS -N bowtie\
\#PBS -M username@ufl.edu\
\#PBS -m abe\
\#PBS -o bowtie.test.out\
\#PBS -e bowtie.test.err\
\#PBS -l nodes=1:ppn=4\
\#PBS -l pmem=2000mb\
\#PBS -l walltime=00:30:00\
\#

cd $PBS_O_WORKDIR

\# Load the module for bowtie\
module load bowtie

\# Run bowtie\
bowtie -p 4 --best  /project/bio/bowtie/hg19  -q my_file.fastq -S my_file.sam

-------------------------------------------------------------
at remote host terminal:\
`qsub bowtie_test.pbs`\
\#submit the job

`qstat -u username`\
\#check status

it took me around 10 mins to finish the mapping.

then at a local terminal download the resulting my_file.sam to local computer:

`scp username.submit.hpc.ufl.edu:/scratch/hpc/mtang/my_file.sam ~/Datasets`
