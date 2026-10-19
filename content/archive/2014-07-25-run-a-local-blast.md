---
title: run a local blast
author: Ming Tang
date: '2014-07-25T10:53:00-07:00'
lastmod: '2014-07-25T10:53:00-07:00'
slug: run-a-local-blast
tags:
- blast
- fastqc
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/07/run-local-blast.html
description: I was checking the quality of a ChIP-seq fastq with fastqc, and the overrepresented sequence showed that the data contain Illumina Single End Adapter 1…
summary: I was checking the quality of a ChIP-seq fastq with fastqc, and the overrepresented sequence showed that the data contain Illumina Single End Adapter 1…
header:
  caption: ''
  image: ''
---

I was checking the quality of a ChIP-seq fastq with fastqc, and the overrepresented sequence showed that the data contain Illumina Single End Adapter 1 contamination.

![](/img/archive/run-a-local-blast/01-capture.png)

I have a file which contains all the possible illumina library sequences in fasta format (got from [here](http://onetipperday.blogspot.com/2012/08/three-ways-to-trim-adaptorprimer.html)), and I manually looked it and found that the single End Adapter 1 sequence did not match the report. I then was wondering  which sequence is the contamination. To do that, I have to do a local blast of  the overrepresented sequence "`GATCGGAAGAGCTCGTATGCCGTCTTCTGCTTAGATCGGAAGAGCTCGTATGCC"`\
on the illumina library sequences.

Luckily, the ncbi_blast was installed on the HPC. One can follow the link here [run local blast](http://ged.msu.edu/angus/tutorials-2013/running-command-line-blast.html)\
first, use the formatdb command to convert the file to a database

`$ module load ncbi_blast`

`# check what arguments are for the command`\
`$ formatdb -`

`formatdb 2.2.26 arguments:`

` -t Title for database file [String] Optional`\
` -i Input file(s) for formatting [File In] Optional`\
` -l Logfile name: [File Out] Optional`\
` default = formatdb.log`\
` -p Type of file`\
` T - protein`\
` F - nucleotide [T/F] Optional`\
` default = T`\
` -o Parse options`\
` T - True: Parse SeqId and create indexes.`\
` F - False: Do not parse SeqId. Do not create indexes.`\
` [T/F] Optional`\
` default = F`\
` -a Input file is database in ASN.1 format (otherwise FASTA is expected)`\
` T - True,`\
` F - False.`\
` [T/F] Optional`\
` default = F`\
` -b ASN.1 database in binary mode`\
` T - binary,`\
` F - text mode.`\
` [T/F] Optional`\
` default = F`\
` -e Input is a Seq-entry [T/F] Optional`\
` default = F`\
` -n Base name for BLAST files [String] Optional`\
` -v Database volume size in millions of letters [Integer] Optional`\
` default = 4000`\
` -s Create indexes limited only to accessions - sparse [T/F] Optional`\
` default = F`\
` -V Verbose: check for non-unique string ids in the database [T/F] Optional`\
` default = F`\
` -L Create an alias file with this name`\
` use the gifile arg (below) if set to calculate db size`\
` use the BLAST db specified with -i (above) [File Out] Optional`\
` -F Gifile (file containing list of gi's) [File In] Optional`\
` -B Binary Gifile produced from the Gifile specified above [File Out] Optional`\
` -T Taxid file to set the taxonomy ids in ASN.1 deflines [File In] Optional`

`$ formatdb -i adaptors_list.fa -o F -p F`

several other files with names ended with nhr, nin and nsq are generated in the same directory.

then run blast:

`# check argument for balstall`\
`$ blastall -`\
`blastall 2.2.26 arguments:`

` -p Program Name [String]`\
` -d Database [String]`\
` default = nr`\
` -i Query File [File In]`\
` default = stdin`\
` -e Expectation value (E) [Real]`\
` default = 10.0`\
` -m alignment view options:`\
`0 = pairwise,`\
`1 = query-anchored showing identities,`\
`2 = query-anchored no identities,`\
`3 = flat query-anchored, show identities,`\
`4 = flat query-anchored, no identities,`\
`5 = query-anchored no identities and blunt ends,`\
`6 = flat query-anchored, no identities and blunt ends,`\
`7 = XML Blast output,`\
`8 = tabular,`\
`9 tabular with comment lines`\
`10 ASN, text`\
`11 ASN, binary [Integer]`\
` default = 0`\
` range from 0 to 11`\
` -o BLAST report Output File [File Out] Optional`\
` default = stdout`\
` -F Filter query sequence (DUST with blastn, SEG with others) [String]`\
` default = T`\
` -G Cost to open a gap (-1 invokes default behavior) [Integer]`\
` default = -1`\
` -E Cost to extend a gap (-1 invokes default behavior) [Integer]`\
` default = -1`\
` -X X dropoff value for gapped alignment (in bits) (zero invokes default behavior)`\
` blastn 30, megablast 20, tblastx 0, all others 15 [Integer]`\
` default = 0`\
` -I Show GI's in deflines [T/F]`\
` default = F`\
` -q Penalty for a nucleotide mismatch (blastn only) [Integer]`\
` default = -3`\
` -r Reward for a nucleotide match (blastn only) [Integer]`\
` default = 1`\
` -v Number of database sequences to show one-line descriptions for (V) [Integer]`\
` default = 500`\
` -b Number of database sequence to show alignments for (B) [Integer]`\
` default = 250`\
` -f Threshold for extending hits, default if zero`\
` blastp 11, blastn 0, blastx 12, tblastn 13`\
` tblastx 13, megablast 0 [Real]`\
` default = 0`\
` -g Perform gapped alignment (not available with tblastx) [T/F]`\
` default = T`\
` -Q Query Genetic code to use [Integer]`\
` default = 1`\
` -D DB Genetic code (for tblast[nx] only) [Integer]`\
` default = 1`\
` -a Number of processors to use [Integer]`\
` default = 1`\
` -O SeqAlign file [File Out] Optional`\
` -J Believe the query defline [T/F]`\
` default = F`\
` -M Matrix [String]`\
` default = BLOSUM62`

`..........`\
`..........`

`$ blastall -i overrepresented_sequence.fa -d adaptors_list.fa -p blastn -e 1e-6`

look at the result, I found that Adapter 2 is the contamination 

`>Illumina_Single_End_Apapter_2`\
` Length = 34`

` Score = 63.9 bits (32), Expect = 1e-14`\
` Identities = 32/32 (100%)`\
` Strand = Plus / Minus`

`Query: 1 gatcggaagagctcgtatgccgtcttctgctt 32`\
` ||||||||||||||||||||||||||||||||`\
`Sbjct: 33 gatcggaagagctcgtatgccgtcttctgctt 2`

I then feed Trimmomatic with this fasta file and removed the adaptor.
