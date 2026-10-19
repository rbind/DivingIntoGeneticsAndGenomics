---
title: hosting bigwig by dropbox for UCSC visualization
author: Ming Tang
date: '2014-02-18T12:53:00-08:00'
lastmod: '2014-02-18T12:53:00-08:00'
slug: hosting-bigwig-by-dropbox-for-ucsc-visualization
tags:
- bedgraph
- bedtools
- bigwig
- ChIP-seq
- MACS
- UCSC
- wig
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/02/hosting-bigwig-by-dropbox-for-ucsc.html
description: 'First, you need to get an idea of what is a bigwig file:'
summary: 'First, you need to get an idea of what is a bigwig file:'
header:
  caption: ''
  image: ''
---

First, you need to get an idea of what is a bigwig file: <https://genome.ucsc.edu/goldenPath/help/bigWig.html>

It is the binary form of wig file and allows UCSC genome browser to fetch only the data in the current window. I usually get wig file by MACS1.4 peak calling ChIP-seq data.\
MACS2  now does not have the -w option any more. <https://github.com/taoliu/MACS/>

see a discussion in the google group:\
<https://groups.google.com/forum/#!searchin/macs-announcement/bedgraph$20ucsc$20track/macs-announcement/LBhAtmC-Zho/uZuxU8ZaqdEJ>

MACS2 only creates a bedgraph file <http://genome.ucsc.edu/goldenPath/help/bedgraph.html>\
and one can convert the bedgraph to bigwig:\
<https://github.com/taoliu/MACS/wiki/Build-Signal-Track>\
<https://gist.github.com/taoliu/2469050>\
<http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/>

if you have a bam file, you can produce a bedgraph file by bedtools  genomeCoverageBed:\
<http://bedtools.readthedocs.org/en/latest/content/tools/genomecov.html>\
<http://www.biostars.org/p/64495/#64680>

you need to add a track line (--trackline option) for UCSC genome browser.\
<https://groups.google.com/forum/#!searchin/bedtools-discuss/bedtools$20bedgraph$20ucsc$20track/bedtools-discuss/3CibmlqIdWA/PW_bhgWQfVMJ>

I followed the instructions <https://genome.ucsc.edu/goldenPath/help/bigWig.html>

To create a bigWig track from a wiggle file, follow these steps:

<ol>
<li>Create a wig format file following the directions <a href="https://genome.ucsc.edu/goldenPath/help/wiggle.html">here</a>. Note that when converting a wig file to a bigWig file, you are limited to one track of data in your input file; you must create a separate wig file for each data track. Note that this is the file that is referred to as<code>input.wig</code> in <em>step 5</em> below.</li>
<li>Remove any existing 'track' or 'browser' lines from your wig file so that it contains only data.</li>
<li>Download the <code>wigToBigWig</code> program from the <a href="http://hgdownload.cse.ucsc.edu/admin/exe/">directory</a> of binary utilities.</li>
<li>Use the <code>fetchChromSizes</code> script from the same <a href="http://hgdownload.cse.ucsc.edu/admin/exe/">directory</a> to create the chrom.sizes file for the UCSC database you are working with (e.g. hg19). Note that this is the file that is referred to as <code>chrom.sizes</code> in <em>step 5</em> below.</li>
<li>Create the bigWig file from your wig file using the <code>wigToBigWig</code> utility like so: <code>wigToBigWig input.wig chrom.sizes myBigWig.bw</code><br/>(Note that the wigToBigWig program also accepts a gzipped wig input file.)</li>
<li>Move the newly created bigWig file (<code>myBigWig.bw</code>) to a http, https, or ftp location.</li>
<li>Construct a <a href="https://genome.ucsc.edu/goldenPath/help/hgTracksHelp.html#CustomTracks">custom track</a> using a single <a href="https://genome.ucsc.edu/goldenPath/help/hgTracksHelp.html#TRACK">track line</a>. The most basic version of the track line will look something like this:<br/><code>track type=bigWig name="My Big Wig" description="A Graph of Data from My Lab" bigDataUrl=http://myorg.edu/mylab/myBigWig.bw</code><p>
Optional values can include:</p>
<pre class="text">  autoScale         &lt;on|off&gt;                             # default is on
  alwaysZero        &lt;on|off&gt;                             # default is off
  gridDefault       &lt;on|off&gt;                             # default is off
  maxHeightPixels   &lt;max:default:min&gt;                    # default is 128:128:11
  graphType         &lt;bar|points&gt;                         # default is bar
  viewLimits        &lt;lower:upper&gt;                        # default is range found in data
  viewLimitsMax     &lt;lower:upper&gt;                        # suggested bounds of viewLimits, but not enforced
  yLineMark         &lt;real-value&gt;                         # default is 0.0
  yLineOnOff        &lt;on|off&gt;                             # default is off
  windowingFunction &lt;mean+whiskers|maximum|mean|minimum&gt; # default is maximum, mean+whiskers is recommended
  smoothingWindow   &lt;off|[2-16]&gt;                         # default is off
  transformFunc     &lt;NONE|LOG&gt;                           # default is NONE</pre>
<p>
For further information on custom bigWig track settings, see the <a href="https://genome.ucsc.edu/goldenPath/help/trackDb/trackDbDoc.html#wig_-_Signal_Graphing_Track_Settings">Track Database Definition Document</a>. For further information on how bigWig settings are used in native Browser tracks, see the <a href="https://genome.ucsc.edu/goldenPath/help/hgWiggleTrackHelp.html">Configuring graph-based tracks</a> page.</p>
</li>
<li>Paste this custom track line into the text box in the <a href="https://genome.ucsc.edu/cgi-bin/hgCustom">custom track</a> management page.</li>
</ol>

I've encountered a coordinates problem mentioned here <https://groups.google.com/forum/#!topic/macs-announcement/yPSPlKdTOwo> when I tried to convert wig to bigwig.

the -clip option in the wigToBigWig program seems to resolve the problem, but it still gives warning messages. The best way is to use bedClip program here first <http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/>

Now, I need to use some public ftp or http to host the resulted bigwig file. I decided to use dropbox as I have used it for a while and I have 15G store space. The UCSC genome browser has problem to accept my link. I found a answer here:\
<http://bergmanlab.smith.man.ac.uk/?p=1989>

"The first problem with the Share Link function is that the URL automatically generated by Dropbox cannot be read by the UCSC Genome Browser. For example, the link generated to the file “test.bed” in my Dropbox folder is “https://www.dropbox.com/s/7sjfbknsqhq6xfw/test.bed”, which gives an “Unrecognized format line 1″ error when pasted into the UCSC Browser.  This can easily be fixed if you just want to load a single custom track  to the UCSC Browser using Dropbox by simply replacing “www.dropbox” in the URL generated by Dropbox with “dl.dropboxusercontent”. In this example, the corrected path to the file would be “https://dl.dropboxusercontent.com/s/7sjfbknsqhq6xfw/test.bed”, which can be loaded by the UCSC Genome Browser automatically."

Finally I had the data visualized in UCSC!\
lsd1 binding at the oct4 locus:

![](/img/archive/hosting-bigwig-by-dropbox-for-ucsc-visualization/01-selection_077.png)

Next time, I will get a bedgraph file from the MACS1.4 or MACS2, and use the bdg2bw program <https://gist.github.com/taoliu/2469050>\
" conversion to bedgraph is necessary to reduce the final bw size (up to 70%)"

=========================================
I got an email from dropbox, this was mentioned in the blog <http://bergmanlab.smith.man.ac.uk/?p=1989>\
I guess I need to find another place to host my bigwig files...

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td style="text-align: left;"><br />
Hi Ming,<br />
<br />
This email is an automated notification from Dropbox that your Public links have been temporarily suspended for generating excessive traffic. Your Dropbox will continue to function normally with the exception of Public links.<br />
<br />
For more information on suspended links, please visit the <a href="https://www.dropbox.com/l/XOkYQG3Hof74GkHm0nl3Hc/help/45">Help Center</a>. If this is your first suspension, you may remove the suspension by visiting your <a href="https://www.dropbox.com/l/bLx3GxLJL1RPlOMIgAn7uk/account">account page</a>.</td>
</tr>
</tbody>
</table>
