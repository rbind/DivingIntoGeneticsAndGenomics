---
title: Liftover between different Genome assemblies (hg18 to hg19)
author: Ming Tang
date: '2013-04-20T17:22:00-07:00'
lastmod: '2013-04-20T17:22:00-07:00'
slug: liftover-between-different-genome-assemblies-hg18-to-hg19
tags:
- liftover
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/liftover-between-different-genome.html
description: Given the huge response I have at work about remapping features into another assembly, I present here an adapted version for how to remap a feature from…
summary: Given the huge response I have at work about remapping features into another assembly, I present here an adapted version for how to remap a feature from…
header:
  caption: ''
  image: ''
---

see a post here:

<http://manuelcorpas.com/2011/02/02/838/>

Given the huge response I have at work about remapping features into another assembly, I present here an adapted version for how to remap a feature from NCBI36/hg18 to GRCh37/hg19 using UCSC’s liftOver tool.

### Important:

Please make sure you know in advance the assembly to which your aberration data is currently mapped to. If by mistake you remap an aberration already in GRCh37 to GRCh37 you will get new coordinates for the region mapped to the wrong coordinates.\
UCSC’s Genome Browser provides a web facility to convert coordinates from one assembly into another. To convert coordinates using their liftOver tool do the following:

1.  Make sure that your data is in BED format, e.g.  “chr3     100000  999990  myPatientId0000123” –\> aberration in NCBI36/hg18
2.  Note that each field is separated by a tab and each line by a character return. Please follow this strictly or the remapping tool may throw an error.
3.  Add as many lines as aberrations you would like to remap.
4.  Go to the [liftOver page](http://genome.ucsc.edu/cgi-bin/hgLiftOver)
5.  Select “Original Assembly” Mar. 2006 (NCBI36/hg18) and “New Assembly” Feb. 2009 (GRCh37/hg19)
6.  Leave all other parameters (Minimum ratio of bases that must remap, etc) with default values
7.  Paste your aberration in the input box where it says “Paste in data” and hit submit
8.  To get results, scroll down the page and click on the “View Conversions” link.
9.  Here is the result I get:

``` text
chr3  125000      1024990     myPatientId0000123
```

Please note that your feature may not remap because the region is partially or entirely deleted in the new assembly or split in GRCh37. In this case I recommend that you use another start or end point position, maybe use the start/end of alternative probes until you find a region where it maps. Another possibility would be to look at the genes for the region in the old assembly and select a region in GRCh37 that includes the same genes as in NCBI36. Each of these solutions require careful deliberation and may not be applicable to your particular case (e.g. genes in different chromosomes would not allow remapping based on genes).\
I hope this is helpful.\
[Follow Manuel Corpas on Twitter](https://twitter.com/manuelcorpas)

## [Tips for Remapping from NCBI36 to GRCh37 Genome Assembly](http://manuelcorpas.com/2009/07/28/tips-for-remapping-from-ncbi36-to-ncbi37-genome-assembly/ "Permalink to Tips for Remapping from NCBI36 to GRCh37 Genome Assembly")

July 28, 2009 [§ 5 Comments](http://manuelcorpas.com/2009/07/28/tips-for-remapping-from-ncbi36-to-ncbi37-genome-assembly/#comments "Comment on Tips for Remapping from NCBI36 to GRCh37 Genome Assembly")

It might seem for some people straight forward but I had to spend quite some time trying to understand how to remap my array probes from ncbi36 to CGRCh37. If you use the Ensembl genome browser, you might have noticed that from July 2009 the ncbi37 assembly is now in use. For [DECIPHER](https://decipher.sanger.ac.uk/) (the database I help develop), this is a little bit of a headache, because it means that all of the probes from array CGH that we used have to be remapped to the new assembly. If this does not interest you I recommend that you stop reading here.\
First I learned that there is a program called liftOver by UCSC that is able to do this remapping. Since the amount of probes I have to map (around 6 million) is a number that I would not wish to through to anyone’s server, I decided to do this in-house. You can download this program from [here](http://hgdownload.cse.ucsc.edu/admin/exe/). I did not know which was the right binary for me to download, as they had linux32 and linux64 versions. I decided to go for the former, since I am using debian and it sounds like a conservative option.\
Once I downloaded the program, I needed to make it executable:

``` text
chmod u+x liftOver
```

OK, so I was in a position to run it:

``` text
./liftOver
```

In the usage information it appears that I need several arguments and files to be able to run this program correctly:

``` text
liftOver oldFile map.chain newFile unMapped
```

Now I learned that I need also to get a file called the map.chain. I was not sure what it meant. I learned that this map.chain file has parameters that are used by liftOver and that there are map.chain files depending on the remapping one wants to do. In my case, I want to remap from ncbi36 to GRCh37 in human. However, when I look at the different remappings, I do not see ncbi formats anywhere. I learned [here](ftp://hgdownload.cse.ucsc.edu/goldenPath/hg19/liftOver/README.txt) that what I am looking for is map chain file that is called this:

``` text
hg18toHg19.over.chain
```

Apparently hg18 refers to ncbi36 and hg19 to ncbi37. Doing a google search I could find that file [here](http://hg.mirror.ac.za/gbdb/hg18/liftOver/hg18ToHg19.over.chain.gz).\
Now I get quite a few options and learn that I need to have my probes in bed format to run liftOver. Apparently there are quite a few formats I can use according to [UCSC FAQs formats](http://genome.ucsc.edu/FAQ/FAQformat#format1). Here an example of what my bed file looks like (chromosome-tab-start_position-tab-end_position):

``` text
chrY       12308579        12468100
chrY       12468101        12581699
chrY       12581700        12759636
chrY       12759637        12838587
```

Now I am in a position to run liftOver. I notice now that in the usage one has the following description:

``` text
liftOver oldFile map.chain newFile unMapped
```

‘newFile’ and ‘unMapped’ are the names of the files where the output goes into and therefore are empty. This can be confusing as the user might think that these are some other kind of files one has to get hold of.\
OK, so now I am ready to transform our old array probe mapping ncbi36 to the new ncbi37 one:

``` text
./liftOver probes.ncbi36 hg18toHg19.over.chain probes.grch37 unmapped-to-grch37
```

I got the following output to console:

``` text
Reading liftover chains
Mapping coordinates
ERROR: start coordinate is after end coordinate (chromStart > chromEnd) on line 5171240 of bed file probes.decipher.ncbi36
ERROR: 4 2515512 2515453
```

…which is a bit worrying.\
I’ve gone through my probes and found that some of them (just 44757!) had start point coordinates greater than their ends. I guess that if you encounter those you’ll have to decide what to do. For the time being I just took them out and re run liftOver again.\
This time it worked.

--------------------------------------------------------------------------------------------------------------------------------------------------------
You can find the liftover files here:\
<http://hgdownload.cse.ucsc.edu/goldenPath/hg18/liftOver/>\
web based:\
<http://genome.ucsc.edu/cgi-bin/hgLiftOver>\
Galaxy <https://main.g2.bx.psu.edu/> also has a function tool for that purpose.\
NCBI:\
<http://www.ncbi.nlm.nih.gov/genome/tools/remap/>
