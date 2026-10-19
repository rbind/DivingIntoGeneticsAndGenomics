---
title: batch converting coordinates to sequences
author: Ming Tang
date: '2013-04-30T19:20:00-07:00'
lastmod: '2013-04-30T19:20:00-07:00'
slug: batch-converting-coordinates-to-sequences
tags:
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/batch-converting-coordinates-to.html
description: If you have a file which contains the coordinates of the chromosomes ( let's say you have a bed file from a\ ChIP-seq peak call) , how can you get the…
summary: If you have a file which contains the coordinates of the chromosomes ( let's say you have a bed file from a\ ChIP-seq peak call) , how can you get the…
header:
  caption: ''
  image: ''
---

If you have a file which contains the coordinates of the chromosomes ( let's say you have a bed <https://genome.ucsc.edu/FAQ/FAQformat.html#format1> file from a\
ChIP-seq peak call) , how can you get the sequences of corresponding coordinates? ( you may need the sequences for motif analysis by MEME <http://meme.nbcr.net/meme/intro.html> )

1.  a simple way is using the table browser in UCSC genome browser

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><table>
<tbody>
<tr>
<td style="text-align: left;"><strong> Extracting sequence in batch from an assembly</strong></td>
<td style="text-align: right;"><a href="http://genome.ucsc.edu/FAQ/FAQdownloads.html#TOP"><img src="http://genome.ucsc.edu/images/index.gif" /></a></td>
</tr>
</tbody>
</table></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th></th>
</tr>
&#10;<tr>
<td></td>
<td><p><strong>Question: </strong><br />
"I have a lot of coordinates for an assembly and want to extract the corresponding sequences. What is the best way to proceed?<br />
<strong>Response:</strong><br />
There are two ways to extract genomic sequence in batch from an assembly:<br />
A. Download the appropriate fasta files from our <a href="ftp://hgdownload.cse.ucsc.edu/goldenPath/">ftp server</a> and extract sequence data using your own tools or the tools from our source tree. This is the recommended method when you have very large sequence datasets or will be extracting data frequently. Sequence data for most assemblies is located in the assembly's "chromosomes" subdirectory on the downloads server. For example, the sequence for human assembly hg17 can be found in<a href="ftp://hgdownload.cse.ucsc.edu/goldenPath/hg17/chromosomes/">ftp://hgdownload.cse.ucsc.edu/goldenPath/hg17/chromosomes/</a>. You'll find instructions for obtaining our source programs and utilities <a href="http://genome.ucsc.edu/FAQ/FAQlicense#license3">here</a>. Some programs that you may find useful are nibFrag and twoBitToFa, as well as other fa* programs. To obtain usage information about most programs, execute it without arguments.<br />
B. Use the Table browser to extract sequence. This is a convenient way to obtain small amounts of sequence.</p>
<ol>
<li>Create a <a href="http://genome.ucsc.edu/goldenPath/help/hgTracksHelp.html#CustomTracks">custom track</a> of the genomic coordinates in <a href="http://genome.ucsc.edu/FAQ/FAQformat#format1">BED format</a> and upload into the Genome Browser.</li>
<li>Select the custom track in the Table browser, then select the "sequence" output format to retrieve data. We recommend that you save the file locally as gzip.</li>
</ol></td>
</tr>
</tbody>
</table>

2.\
Use bedtools\
<http://bedtools.readthedocs.org/en/latest/content/tools/getfasta.html>

<http://www.biostars.org/p/19267/>

``` text
$ bedtools getfasta [OPTIONS] -fi <input FASTA> -bed <BED/GFF/VCF> -fo <output FASTA>
```

but you will need to download the whole genome sequences in a fasta format.

3\.  another option will be using python package pygr <https://code.google.com/p/pygr/>

``` text
>>> from pygr import worldbase
>>> worldbase.dir('Bio.Seq.Genome.HUMAN')
['Bio.Seq.Genome.HUMAN.hg17', 'Bio.Seq.Genome.HUMAN.hg18', 'Bio.Seq.Genome.HUMAN.hg19']
```

``` text
>>> hg17 = worldbase.Bio.Seq.Genome.HUMAN.hg17()
```

``` text
>>> len(hg17)
46
>>> hg17.keys()
['chr6_random', 'chr19_random', 'chr8_random', 'chrY', 'chrX', 'chr13', 'chr12', 'chr11', 'chr15_random', 'chr17', 'chr16', 'chr15', 'chr14', 'chr19', 'chr18', 'chrM', 'chr1_random', 'chr13_random', 'chr3_random', 'chr6_hla_hap2', 'chr9_random', 'chr22_random', 'chr10', 'chr4_random', 'chr18_random', 'chr2_random', 'chr22', 'chr20', 'chr21', 'chr10_random', 'chr6_hla_hap1', 'chr7', 'chr6', 'chr5', 'chr4', 'chr3', 'chr2', 'chr1', 'chr7_random', 'chrX_random', 'chr9', 'chr8', 'chr16_random', 'chr5_random', 'chr17_random', 'chr12_random']
>>> chr1 = hg17['chr1']
>>> len(chr1)
245522847
>>> s = chr1[100000000:100001000]
>>> len(s)
1000
>>> repr(s)
'chr1[100000000:100001000]'
>>> print s
ATTCAGGCAATGTTGACTTCATTAGTGTTGACAGAAAATAAGCATAAAAATGCAAAACATTGTTGGCTTAACCTGAATATACCTGCATTACCTATTATTAACCAGTTTGTGTTGCCAAAAGACTTATTCCTTGGCATTAAAATGGAGCACTTAAAAATATTTCTAAAAAGCAAATGCCCACACGCTGGTCTTTGCAGCAAATAAGGGTATTTATACTTTTAAAATATTTTAAGTCCATAATTGGATTAATATACACACCTTCTTATGTATAAGGAGTTCAGATCATATAAACACCGTACAATCCAAAAAACCCTACTGAGAATAAAACTAAATAGGCTTATGATAAGAAATACAGATATTCCCATGTATTTACAAATATCATAGACACACAAATTTGGTCAAATACTGTAAAGAAAGAAGAAGtacctgtacctctacctctacctctacctcctctacctcctctacctcctctacctcctctacctcctctacctcctcttcctctacctctacctctacctctacctctacccacggtctccctttccctctctttccacggtctccctctgatgccgagccgaagctggactgtactgctgccatctcggctcactgcaacctccctgcctgattctcctgcctcaacctgccgagtgcctgcgattgcaggcgcgcgccgccacgcctgactggttttcgtatttttttggtggagacggggtttcgctgtgttggccgggctggtctccagctcctaacctcgagtgatccgccagcctcggcctcccgaggtgccgggtttgcagaggagtctcattcactcagtgctcaatggtgcccaggctggagtgcagtggcgtgatctcggctcgctacaacctccacctcccagcagcctgccttggcctcccaaagtgccgagattgcagtctccgcccggctgccaccccatctgggaagtgaggagcatctctgcctggccgcccatcgtctg
```

you can easily pull out the sequences based on chromosome name, start, end.

" Again, note that all of these operations were more or less instantaneous (even over the network). These Pygr objects (`chr1`, `s`) are just abstract representations of the specified pieces of the human genome, so creating these objects*does not* require Pygr to download all of human chromosome 1. Only when you force it to show a sequence string does Pygr obtain that data over the network – and then only the piece that you need."
