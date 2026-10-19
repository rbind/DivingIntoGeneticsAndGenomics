---
title: convert bam file to bigwig file and visualize in UCSC genome browser in a Box (GBiB)
author: Ming Tang
date: '2014-10-23T13:13:00-07:00'
lastmod: '2014-10-23T13:13:00-07:00'
slug: convert-bam-file-to-bigwig-file-and-visualize-in-ucsc-genome-browser-in-a-box
tags:
- bam
- bigwig
- ChIP-seq
- UCSC
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/10/convert-bam-file-to-bigwig-file-and.html
description: I just came across UCSC genome browser in a box Gbib, and I want to try it out.
summary: I just came across UCSC genome browser in a box Gbib, and I want to try it out.
header:
  caption: ''
  image: ''
---

I just came across UCSC genome browser in a box [Gbib](http://genome.ucsc.edu/goldenPath/help/gbib), and I want to try it out.

what is Gbib?\
from the link above:

"Genome Browser in a Box (GBiB) is a "virtual machine" of the entire UCSC Genome Browser website that is designed to run on most PCs (Windows, Mac OSX or Linux). GBiB allows you to access much of the UCSC Genome Browser's functionality from the comfort of your own computer. It is particularly directed at individuals who want to use the Genome Browser's functionality to view protected data. With the anticipation that the majority of protected data use will focus on the human genomes (primarily GRCh37/hg19, and transitioning to GRCh38/hg38), the current version of GBiB has been optimized for use with the hg19 assembly. Many other recent genome assemblies can also be viewed, but without [mirroring](http://genome.ucsc.edu/goldenPath/help/gbib#GbibMirroring) of additional data, they may be slow."

Download the compressed file from here <https://genome-store.ucsc.edu/> and follow the installation instructions. Open virtual box and start the virtual machine and then direct your web browser to [127.0.0.1:1234](https://www.blogger.com/127.0.0.1:1234), you should see the UCSC genome browser page.

It looks the same as the UCSC genome browser page <https://genome.ucsc.edu/>. You can then go to the browser and do what you usually do in the genome browser. when I searched a gene, and errors came out:

- nextRow failed
- mySQL error 1194: Table 'rmsk' is marked as crashed and should be repaired (profile=db, host=localhost, db=hg19)

I asked the UCSC mail list, and I typed the command below in the console:\
 `sudo mysqlcheck --all-databases --auto-repair --fast`\
it works like a charm now!

I have some bam files and I want to visualize them in the UCSC genome browser. I would first convert them to bigwig <http://genome.ucsc.edu/goldenpath/help/bigWig.html> and then upload to some open http https or ftp locations.  see my old post:\
[hosting bigwig by Dropbox for UCSC](/tags/bigwig/)

However, with GBiB one only needs to share a local folder containing the bigwig files with virtual box.

"**Loading Local Big Data Tracks**

Your computer can share directories with GBiB so that you can load big files without the need to upload them to a web server. The big file formats are compressed, indexed, binary files, and include [bigBed](http://genome.ucsc.edu/goldenPath/help/bigBed.html),[bigWig](http://genome.ucsc.edu/goldenPath/help/bigWig.html), [BAM](http://genome.ucsc.edu/goldenPath/help/bam.html), and [VCF](http://genome.ucsc.edu/goldenPath/help/vcf.html) files. Normally, one would have to place these types files onto a publicly accessible web server to upload them as a custom track. However, GBiB acts as its own web server, meaning that you can share local files with GBiB for easy uploading as a custom track."

I will start from converting bam to bigwig.\
several posts you may want to have a look:\
<https://www.biostars.org/p/64495/#64680>\
<https://www.biostars.org/p/2699/>\
<http://onetipperday.blogspot.com/2012/07/three-ways-to-convert-bambed-file-to.html>

I used genomeCoverageBed from bedtools to convert bam to bedgraph first, and then used UCSC utilities bedClip and bedGraphToBigWig <http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/> to do the conversion.

bedtools requires the bam files be sorted. so samtools sort was used to sort the bam file first.\
I used a script from Tao Liu (the author of famous MACS ChIP-seq calling software) to do the conversion after I got the bedgraph files. I then put the bigwig files in a folder and shared it with virtual box. Follow the instructions <http://genome.ucsc.edu/goldenPath/help/gbib#LocalTracks>. I successfully visualized a bunch of bigwig files!

{{< gist crazyhottommy b38bed2fa9d7060806e4 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/b38bed2fa9d7060806e4)
