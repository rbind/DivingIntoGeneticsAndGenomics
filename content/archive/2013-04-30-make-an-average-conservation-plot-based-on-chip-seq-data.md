---
title: make an average conservation plot based on ChIP-seq data
author: Ming Tang
date: '2013-04-30T19:27:00-07:00'
lastmod: '2013-04-30T19:27:00-07:00'
slug: make-an-average-conservation-plot-based-on-chip-seq-data
tags:
- ChIP-seq
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/make-average-conservation-plot-based-on.html
description: I asked this question at Seqanswer\
summary: I asked this question at Seqanswer\
header:
  caption: ''
  image: ''
---

I asked this question at Seqanswer\
<http://seqanswers.com/forums/showthread.php?t=29535>

save it here for later reference. Thanks

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p><strong>How to make an average conservation plot from ChIP-seq data</strong></p>
<hr />
<p>Hi all,<br />
<br />
I want to know how to make an average conservation plot<br />
<a href="http://ceas.cbi.pku.edu.cn/">http://ceas.cbi.pku.edu.cn/</a> this website can make something I want, but it can only plot<br />
a set of regions at one time, I want to plot two sets of regions, and compare them.<br />
<br />
" 2. GC content and evolutionary conservation of each ChIP-region and their<br />
average. CEAS uses PhastCons conservation scores from UCSC Genome<br />
Bioinformatics, which is based on multiz alignment of human, chimp, mouse,<br />
rat, dog, chicken, fugu, and zebrafish genomic DNA. CEAS generates thumbnail<br />
conservation plot for each ChIP-region and the average conservation plot for<br />
all the ChIP-regions, which can be directly used in ChIP-chip biologists'<br />
manuscript.<br />
"<br />
<br />
<br />
Any python scripts or bioconductor package can do it?<br />
<br />
Thanks</p></td>
<td></td>
</tr>
<tr>
<td>   </td>
<td style="text-align: right;">   </td>
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
<td> 04-22-2013, 02:05 AM</td>
<td style="text-align: right;">  #<a href="http://seqanswers.com/forums/showpost.php?p=102509&amp;postcount=2"><strong>2</strong></a></td>
</tr>
<tr>
<td><p><a href="http://seqanswers.com/forums/member.php?u=20116">paolo.kunder</a><br />
Member</p>
<br />
&#10;<p>Location: Milano, Italy</p>
<br />
&#10;<p>Join Date: Aug 2011<br />
Posts: 56</p></td>
<td style="text-align: right;"><hr />
Hi,<br />
<br />
I encountered the same problem few weeks ago,<br />
<br />
It seems that there are not many "expert" regarding PhastCons in this forum.<br />
<br />
I explain you what I did,<br />
<br />
I took my Chip-seq regions, in bed file format, intersect them with PhastCons element in (mouse example)<br />
<br />
&#10;<p>Code:</p>
<pre class="text"><code>http://hgdownload-test.cse.ucsc.edu/goldenPath/mm9/phastCons30way/</code></pre>
<br />
and calculated for each genomic position the score of conservation.<br />
Then averaged all the score.<br />
<br />
Consider that you will need to transform a bit the PhastCons file format,<br />
<br />
for example:<br />
<br />
original format ( just replaced some words with empty)<br />
&#10;<p>Code:</p>
<pre class="text"><code>chrom=chr1 start=3000306
0.006
0.010
0.014
chrom=chrX start=40000306
0.014
chrom=chr9 start=80000306
0.1
0.2</code></pre>
processed format<br />
&#10;<p>Code:</p>
<pre class="text"><code>chr1 3000306 3000307 0.006
chr1 3000307 3000308 0.010
chr1 3000308 3000309 0.019
chrX 40000306 40000307 0.014
chr9 80000306 80000307 0.1
chr9 80000307 80000308 0.2</code></pre>
with the following script:<br />
&#10;<p>Code:</p>
<pre class="text"><code>awk &#39;/^chrom/{split($1,a,&quot;=&quot;);split($2,b,&quot;=&quot;);next} { printf &quot;%s\t%10d\t%10d\t%f\n&quot;,a[2],b[2],b[2]+1,$1;b[2]++}&#39; filename</code></pre>
<br />
<br />
Another possibility that I am investigating is using circos<br />
&#10;<p>HTML Code:</p>
<pre class="text"><code>http://circos.ca/</code></pre>
but You will need to study first how the software works,<br />
<br />
Cheers,<br />
Paolo</td>
</tr>
<tr>
<td>   </td>
<td style="text-align: right;">  </td>
</tr>
</tbody>
</table>
