---
title: Genomic Interval manipulation
author: Ming Tang
date: '2013-04-02T20:02:00-07:00'
lastmod: '2013-04-02T20:02:00-07:00'
slug: genomic-interval-manipulation
tags:
- Genomic Interval
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/several-good-blogs-for-bioinformatics.html
description: '# for manipulation of Genomic Intervals\ # you can use Galaxy ,\ #Bedtools \ #or Bioconductor package GenomicRanges #\ #python package HTSeq'
summary: '# for manipulation of Genomic Intervals\ # you can use Galaxy ,\ #Bedtools \ #or Bioconductor package GenomicRanges #\ #python package HTSeq'
header:
  caption: ''
  image: ''
---

\#  for manipulation of Genomic Intervals\
\# you can use Galaxy <https://main.g2.bx.psu.edu/>,\
\#Bedtools <http://bedtools.readthedocs.org/en/latest/content/overview.html>\
\#or Bioconductor  package GenomicRanges \#<http://stuff.mit.edu/afs/athena/software/r_v2.14.1/lib/R/library/GenomicRanges/html/00Index.html>\
\#python package HTSeq <http://www-huber.embl.de/users/anders/HTSeq/doc/tour.html#tour>

## <http://psaffrey.wordpress.com/2011/04/>

## [Peter Saffrey's Weblog](http://psaffrey.wordpress.com/)

Scientific computing and bioinformatics.

### [Overlapping Genomic Intervals Revisited](http://psaffrey.wordpress.com/2011/04/05/overlapping-genomic-intervals-revisited/ "Permanent Link to Overlapping Genomic Intervals Revisited")

April 5, 2011

I’ve [posted](http://psaffrey.wordpress.com/2009/03/19/binning-on-a-grand-scale/) [before](http://psaffrey.wordpress.com/2009/04/01/interval-trees-even-faster-in-cython/) on finding the right approach to overlapping genomic intervals. Broadly speaking, the problem is this: you have two sets of intervals, where each interval is (chromosome, start, end). You want to know where the intervals in the first set overlap the intervals in the second set.\
For a while now, my go-to tool for this problem has been [interval trees](http://en.wikipedia.org/wiki/Interval_tree), which are a flexible and efficient data structure for inserting intervals from one set and making queries with the other. I have a cython implementation which is nice and fast.\
Using an interval tree to find interval overlap has two stages:

1.  Loading intervals into the tree to be queried.
2.  Querying those intervals.

In general, a tree data structure like this is optimised so that adding items is (relatively) slower than making queries, so we should try to minimise this stage. Once the tree is built, we can make nice fast queries over and over again. This is good in a database-like environment, where we build a tree at the start and then make queries here and there throughout the rest of our application.\
However, many of my interval overlap applications are not like that. I have two sets of intervals and I want to find the overlaps between them once and then I’m done. I have to go through the expensive tree construction stage every time.\
My alternative to this approach is to use a [plane sweep](http://en.wikipedia.org/wiki/Sweep_line_algorithm) algorithm. Conceptually, we build a list of the starts and ends of all the intervals from both sets, sorted in genomic order. Then we “sweep” from left to right recording when we enter and exit intervals from each set, and output when we’re in an interval from both. The problem is complicated slightly because with very large sets of intervals we don’t want to construct the whole list at the start, so I wanted to do the plane sweep dynamically, using pre-sorted lists of both sides and popping off items as I go.\
After a bit of bashing, I got this working and the code is below: (I know this is verbose – it has quite a few comments)

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16<br />
17<br />
18<br />
19<br />
20<br />
21<br />
22<br />
23<br />
24<br />
25<br />
26<br />
27<br />
28<br />
29<br />
30<br />
31<br />
32<br />
33<br />
34<br />
35<br />
36<br />
37<br />
38<br />
39<br />
40<br />
41<br />
42<br />
43<br />
44<br />
45<br />
46<br />
47<br />
48<br />
49<br />
50<br />
51<br />
52<br />
53<br />
54<br />
55<br />
56<br />
57<br />
58<br />
59<br />
60<br />
61<br />
62<br />
63<br />
64<br />
65<br />
66<br />
67<br />
68<br />
69<br />
70<br />
71<br />
72<br />
73<br />
74<br />
75</p></td>
<td><p><code># uses a plane sweep to determine the overlaps between two sets of genomic intervals</code><br />
<code># they must come in as iterators</code><br />
<code># sorted, with the same chromosome order</code><br />
<code>def</code> <code>overlapGeneratorPlaneSweep(left, right):</code><br />
    <code># rightcache holds the points we've retrieved from the right set</code><br />
    <code># that match the current left set chromosome, but we haven't finished filing yet</code><br />
    <code>rightcache</code><code>=</code> <code>[]</code><br />
    <code># nextcache holds points from right that from a later chromosome</code><br />
    <code># to what we're currently looking at in left</code><br />
    <code>nextcache</code><code>=</code> <code>[]</code></p>
<p>    <code>lastchrom</code><code>=</code> <code>None</code><br />
    <code>for</code> <code>linterval</code><code>in</code> <code>left:</code><br />
        <code>lchrom, lstart, lend, lvalue</code><code>=</code> <code>linterval</code></p>
<p>        <code># slight complex mechanics to handle a change in chromosome in left</code><br />
        <code>if</code> <code>lchrom !</code><code>=</code> <code>lastchrom:</code><br />
            <code># still some left over in right</code><br />
            <code># make sure there aren't any points leftover in right</code><br />
            <code># from the last chromosome from left</code><br />
            <code>if</code> <code>rightcache !</code><code>=</code> <code>[]:</code><br />
                <code>righti</code><code>=</code> <code>rightcache[</code><code>-</code><code>1</code><code>]</code><br />
                <code>while</code> <code>righti[</code><code>0</code><code>] &lt; lchrom:</code><br />
                    <code>righti</code><code>=</code> <code>right.</code><code>next</code><code>()</code><br />
                <code>nextcache</code><code>=</code> <code>[ righti ]</code><br />
            <code>rightcache</code><code>=</code> <code>[]</code><br />
            <code>assert</code> <code>len</code><code>(nextcache) &lt;</code><code>=</code> <code>1</code><br />
            <code># the next cache is from a chromosome *before* the left chrom</code><br />
            <code># suck up all the points from before left's chrom</code><br />
            <code># - they will never overlap now</code><br />
            <code>if</code> <code>nextcache !</code><code>=</code> <code>[]</code><code>and</code> <code>nextcache[</code><code>0</code><code>][</code><code>0</code><code>] &lt; lchrom:</code><br />
                <code>righti</code><code>=</code> <code>nextcache[</code><code>0</code><code>]</code><br />
                <code>while</code> <code>righti[</code><code>0</code><code>] &lt; lchrom:</code><br />
                    <code>righti</code><code>=</code> <code>right.</code><code>next</code><code>()</code><br />
                <code>assert</code> <code>righti[</code><code>0</code><code>] &gt;</code><code>=</code> <code>lchrom</code><br />
                <code>nextcache</code><code>=</code> <code>[ righti ]</code><br />
            <code># if the point in nextcache matches the left chrom</code><br />
            <code># nextcache is now current, so make it rightcach</code><br />
            <code>if</code> <code>nextcache !</code><code>=</code> <code>[]</code><code>and</code> <code>nextcache[</code><code>0</code><code>][</code><code>0</code><code>]</code><code>=</code><code>=</code> <code>lchrom:</code><br />
                <code>rightcache</code><code>=</code> <code>nextcache[:]</code><br />
                <code>nextcache</code><code>=</code> <code>[]</code><br />
            <code>assert</code> <code>rightcache</code><code>=</code><code>=</code> <code>[]</code><code>or</code> <code>rightcache[</code><code>0</code><code>][</code><code>0</code><code>]</code><code>=</code><code>=</code> <code>lchrom</code></p>
<p>        <code># if the last point in the rightcache overlaps this left point,</code><br />
        <code># there might be more points in the right that are relevant now</code><br />
        <code>while</code> <code>nextcache</code><code>=</code><code>=</code> <code>[]</code><code>and</code> <code>(rightcache</code><code>=</code><code>=</code> <code>[]</code><code>or</code> <code>rightcache[</code><code>-</code><code>1</code><code>][</code><code>1</code><code>] &lt;</code><code>=</code> <code>lend):</code><br />
            <code>try</code><code>:</code><br />
                <code>righti</code><code>=</code> <code>right.</code><code>next</code><code>()</code><br />
            <code>except</code> <code>StopIteration:</code><br />
                <code>break</code><br />
            <code>if</code> <code>righti[</code><code>0</code><code>]</code><code>=</code><code>=</code> <code>lchrom:</code><br />
                <code>rightcache.append(righti)</code><br />
            <code>elif</code> <code>righti[</code><code>0</code><code>]</code><code>=</code><code>=</code> <code>lastchrom</code><code>or</code> <code>(righti[</code><code>0</code><code>]</code><code>=</code><code>=</code> <code>lchrom</code><code>and</code> <code>righti[</code><code>2</code><code>] &lt;</code><code>=</code> <code>lstart):</code><br />
                <code>pass</code><br />
            <code>else</code><code>:</code><br />
                <code>nextcache.append(righti)</code></p>
<p>        <code>removecount</code><code>=</code> <code>0</code><br />
        <code># the bit where we actually check and report the overlaps</code><br />
        <code>for</code> <code>rinterval</code><code>in</code> <code>rightcache:</code><br />
            <code># we've gone past this one. Remove it</code><br />
            <code>if</code> <code>rinterval[</code><code>2</code><code>] &lt;</code><code>=</code> <code>lstart:</code><br />
                <code>removecount</code><code>+</code><code>=</code> <code>1</code><br />
            <code># we haven't reached this one yet.</code><br />
            <code># We must have filed everything relevant from right.</code><br />
            <code>elif</code> <code>rinterval[</code><code>1</code><code>] &gt;</code><code>=</code> <code>lend:</code><br />
                <code>break</code><br />
            <code>else</code><code>:</code><br />
                <code># if we get here, this is an overlap. Yield it.</code><br />
                <code>yield</code><code>((linterval, rinterval))</code></p>
<p>        <code>rightcache</code><code>=</code> <code>rightcache[removecount:]</code></p>
<p>        <code>lastchrom</code><code>=</code> <code>lchrom</code></p></td>
</tr>
</tbody>
</table>

This operates on two iterators of intervals, `left` and `right` which must be sorted in the same chromosome order. We pop off an interval from left, and then pop off intervals from right, dropping those that are before the left interval and caching the others, until we go past the left interval. We report all the overlaps we found, and then pop the next left interval and continue.\
A lot of the complicated logic happens in making the transition between chromosomes, particularly when there are gaps; both sets of intervals do not have intervals from the same chromosomes. This is where the algorithm relies on the chromosomes from both being in *alphanumeric order*, so that we can see if a chromosome is “before” another one. This works if we presort the intervals using (for example) the shell sort utility, and I have some wrapper functions to make this work with bed files:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15</p></td>
<td><p><code>import</code> <code>csv</code></p>
<p><code>def</code> <code>sortBedGenerator(bedfile):</code><br />
    <code>sortfile</code><code>=</code> <code>bedfile</code><code>+</code> <code>".sort"</code><br />
    <code>cmd</code><code>=</code> <code>"sort -k 1.4,1.5 -k 2,2n %s &gt; %s"</code> <code>%</code> <code>(bedfile, sortfile)</code><br />
    <code>os.system(cmd)</code><br />
    <code>reader</code><code>=</code> <code>csv.reader(</code><code>open</code><code>(sortfile), delimiter</code><code>=</code><code>"\t"</code><code>)</code><br />
    <code>for</code> <code>row</code><code>in</code> <code>reader:</code><br />
        <code>yield</code> <code>(row[</code><code>0</code><code>],</code><code>int</code><code>(row[</code><code>1</code><code>]),</code><code>int</code><code>(row[</code><code>2</code><code>]),</code><code>1</code><code>)</code><br />
    <code>os.remove(sortfile)</code></p>
<p><code>def</code> <code>overlapBedFilesPlaneSweep(leftbedfile, rightbedfile):</code><br />
    <code>leftgen</code><code>=</code> <code>sortBedGenerator(leftbedfile)</code><br />
    <code>rightgen</code><code>=</code> <code>sortBedGenerator(rightbedfile)</code><br />
    <code>return</code> <code>overlapGeneratorPlaneSweep(leftgen, rightgen)</code></p></td>
</tr>
</tbody>
</table>

So we sort the bed file into a temp file, and then `yield` the sorted intervals one at a time.\
The plane sweep algorithm returns pairs of intervals, one from each set, which overlap. As well as the interval itself, it returns a 4th item which could be anything, so you could overlap objects (or whatever) to provide extra context in whatever application we’re writing. This is slightly different from the way the interval tree would be used, of which more later.\
I ran some timing tests on this to compare it to my old faithful cython interval tree, using one tree for each chromosome. My test data was based on the mouse genome. On one side, I used the csp6i restriction enzyme (GTAC), a 4-base cutter we use in a lot of our 4C and Hi-C work; there are 5436400 in the mouse genome. On the other side, I generated random bed files of increasing numbers of 50bp reads – 10, 1000, 10000, 1000000, 2000000 and 5000000. I also did a 10 read example, to make sure the plane sweep algorithm could cope with gaps in the chromosome roster. I made sure both the interval tree and plane sweep methods identified the same intervals (the 10 read examples is missing for this plot, but I did make sure the overlaps were the same). I realise this test scheme is a little arbitrary but this is a typical use case for me, so it seemed like a good place to start.\
Since it makes a difference whether you use the csp6i file as the tree or the query and as the “left” or “right” in the plane sweep, I tested it both ways. I did three tests of each. Below are the results, using the triplicate results to generate standard deviation error bars. Note that “git” stands for Genomic Interval Tree (one interval tree for each chromosome) and “PS” stands for Plane Sweep. Since the GIT method does not require the intervals to be sorted, every PS run includes the time it takes to sort the files first.\
[![](http://psaffrey.files.wordpress.com/2011/04/timings.png?w=300&h=225 "timings")](http://psaffrey.files.wordpress.com/2011/04/timings.png)\
The plot headings refer to whether the csp6i file was on the left (“big-left” – which means it was used to build the interval trees) or the right (“big-right” – used as the queries). The plane sweep seems to perform much better than the interval trees for all the file sizes. The times are starting to ramp up at the 5,000,000 read level, but are still 10 times faster than the GIT method. I haven’t checked this, but it also ought to be much more memory efficient, since we’re not storing all the intervals in trees – we just visit each interval once to check its overlaps as we make the sweep.\
As expected, the interval tree approach is much more efficient when we build a tree with the small number of intervals and make lots of queries on that tree. It makes a difference with the plane sweep too, since one side will have to make use of the more complicated caching machinery. I did a separate analysis to examine this:\
[![](http://psaffrey.files.wordpress.com/2011/04/side-by-side-timings.png?w=300&h=225 "side-by-side-timings")](http://psaffrey.files.wordpress.com/2011/04/side-by-side-timings.png)\
To me, this is a good start, and I have already used the plane sweep approach to do some analysis work. I could probably speed it up even more by allowing the use of pre-sorted files. However, there are quite a few caveats:

1.  This is a very specific set of conditions. Before we could conclusively say the plane sweep approach was faster, there are lots of other conditions I would need to test:
    - Different sizes of intervals.
    - Many intervals within each group that overlap each other.
    - Interval sizes that vary wildly within (and between) the groups.
2.  There is a lot of tuning possible on the structure of the interval trees that will affect performance.
3.  I’m not entirely comfortable with the reliance on coordinates in alphanumeric order here. It probably needs more error checking to make sure it reports if something goes wrong with that.
4.  As I mentioned, the way this should be used is different to the interval tree approach. With the csp6i sites, I usually want to look at a 4C read and say “is it near a csp6i site?” interval trees are a natural fit for this – I just query the tree, based on some tolerance for how near the site should be, and if I get any hits then I can act on those. To use the plane sweep approach, I need to change the layout of my code slightly to look for the presence of the read in the list of returned overlaps. I would also need to add an extra wrapper to generate intervals to take account of the tolerance I want to use. These are not insurmountable obstacles, but the plane sweep is not a drop-in replacement for the interval tree approach.
5.  Although comparing two sets of intervals is the basic use case, you always have to be wary of a one-size-fits-all approach. The plane sweep might be quicker for a pair of interval sets like this but, again with respect to the csp6i example, I might want to load the csp6i sites into a set of interval trees, and then query a list of 4C read-sets (lanes) against the tree. With only a few lanes of 4C, the plane sweep will probably still be quicker. How many lanes would I need before the interval tree approach becomes quicker again?

I guess for me the biggest caveat is that the plane sweep approach is more complex and is therefore more susceptible to edge cases and programmer error. The GIT method is mature and I trust it, so I will probably have more anxiety about the PS for a while. Still, these preliminary results look promising.\
I would also be interested to implement a multi-way plane sweep that overlaps several sets of intervals against a reference set. Maybe this would be quicker than building a tree for the reference set and comparing the others – something I still do very often.

-------------------------------------------------------------------
<http://psaffrey.wordpress.com/2010/07/05/chromosome-interactions-heatmaps-and-matplotlib/>

## Chromosome Interactions, Heatmaps and matplotlib

I’m currently working on some chromosome conformation data based on [3C](http://en.wikipedia.org/wiki/Chromosome_conformation_capture)and related technologies. These tools allow you to look at the physical structure of the nucleus and determine how chromosomes are positioned in 3 dimensions, and where regions of one chromosome physically touch regions of another. These touching regions affect the regulation of genes in those regions.\
The earlier iterations of these techniques are based on looking at interactions from a particular region or locus, to study the interaction with a specific set of genes. Later techniques allow a more flexible whole-genome approach. We are working with [Hi-C](http://www.jove.com/index/details.stp?id=1869) data, which combines 3C/4C methods with [paired-end sequencing](http://www.illumina.com/technology/paired_end_sequencing_assay.ilmn), so that each end of the pair is at a different point on the genome.\
After processing, this data is a neat list of interacting pairs, with each end as a chromosome coordinate. Our data produces between 2 and 10 million pairs. Most pairs are intra-chromosome (also called “cis” interactions) but around 5% are inter-chromosome (“trans” interactions).\
We now need a method to visualise this data. The prevalent method is the[heatmap](http://hic.umassmed.edu/heatmap/heatmap.php). The interaction points are grouped together into chromosomes, or regions of 100kb or 1mb (we could think of this as [quantization](http://en.wikipedia.org/wiki/Quantization_%28signal_processing%29)). Interacting regions are marked out in a 2-dimensional matrix and then drawn as a heatmap.\
For chromosome-chromosome heatmaps we might also normalise each spot based on the number of expected interactions. This is based on the length of the chromosomes – longer chromosomes should give more interactions on average. We also tend to remove cis-interactions – by reducing them to 0 – since otherwise they dominate and we can’t see the relative strengths of the trans-interactions.\
Given the pairs of interactions, it’s easy enough to build the matrix that represents the interactions. Rendering this is matplotlib provides quite a few options. I’ve generated some sample data and showed a few options below, using the following code:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16<br />
17<br />
18<br />
19<br />
20<br />
21<br />
22<br />
23<br />
24<br />
25<br />
26<br />
27<br />
28<br />
29<br />
30<br />
31<br />
32<br />
33</p></td>
<td><p><code>import</code> <code>pylab</code></p>
<p><code>chromnames</code><code>=</code> <code>[</code><code>'chr1'</code><code>,</code><code>'chr2'</code><code>,</code><code>'chr3'</code><code>,</code><code>'chr4'</code><code>,</code><code>'chr5'</code><code>,</code><code>'chr6'</code><code>,</code><code>'chr7'</code><code>,</code><code>'chr8'</code><code>,</code><code>'chr9'</code><code>,</code><code>'chr10'</code><code>,</code><code>'chr11'</code><code>,</code><code>'chr12'</code><code>,</code><code>'chr13'</code><code>,</code><code>'chr14'</code><code>,</code><code>'chr15'</code><code>,</code><code>'chr16'</code><code>,</code><code>'chr17'</code><code>,</code><code>'chr18'</code><code>,</code><code>'chr19'</code><code>,</code><code>'chr20'</code><code>,</code><code>'chr21'</code><code>,</code><code>'chr22'</code><code>,</code><code>'chrX'</code><code>,</code><code>'chrY'</code><code>]</code><br />
<code>values</code><code>=</code> <code>pylab.zeros((</code><code>24</code><code>,</code><code>24</code><code>))</code></p>
<p><code>values[</code><code>2</code><code>][</code><code>6</code><code>]</code><code>=</code> <code>3</code><br />
<code>values[</code><code>6</code><code>][</code><code>2</code><code>]</code><code>=</code> <code>3</code><br />
<code>values[</code><code>23</code><code>][</code><code>1</code><code>]</code><code>=</code> <code>13</code><br />
<code>values[</code><code>1</code><code>][</code><code>23</code><code>]</code><code>=</code> <code>13</code><br />
<code>values[</code><code>5</code><code>][</code><code>2</code><code>]</code><code>=</code> <code>5</code><br />
<code>values[</code><code>2</code><code>][</code><code>5</code><code>]</code><code>=</code> <code>5</code><br />
<code>values[</code><code>12</code><code>][</code><code>14</code><code>]</code><code>=</code> <code>11</code><br />
<code>values[</code><code>14</code><code>][</code><code>12</code><code>]</code><code>=</code> <code>11</code><br />
<code>values[</code><code>19</code><code>][</code><code>9</code><code>]</code><code>=</code> <code>6</code><br />
<code>values[</code><code>9</code><code>][</code><code>19</code><code>]</code><code>=</code> <code>6</code></p>
<p><code>f</code><code>=</code> <code>pylab.Figure()</code></p>
<p><code>#pylab.pcolor(values)</code><br />
<code>#imagename = "pcolor.png"</code></p>
<p><code># pylab.imshow(values)</code><br />
<code># imagename = "imshow.png"</code></p>
<p><code>pylab.matshow(values)</code><br />
<code>imagename</code><code>=</code> <code>"matshow.png"</code></p>
<p><code>pylab.xticks(</code><code>range</code><code>(</code><code>len</code><code>(chromnames)), chromnames, rotation</code><code>=</code><code>90</code><code>)</code><br />
<code>pylab.yticks(</code><code>range</code><code>(</code><code>len</code><code>(chromnames)), chromnames)</code></p>
<p><code>pylab.savefig(imagename)</code></p></td>
</tr>
</tbody>
</table>

My first stab was to use [imshow()](http://matplotlib.sourceforge.net/api/pyplot_api.html#matplotlib.pyplot.imshow), but this bleeds each coloured blob into the surroundings, which looks nice but makes it hard to read the individual blobs:\
![imshow()](http://psaffrey.files.wordpress.com/2010/07/imshow.png?w=450)\
There is also a [pcolor()](http://matplotlib.sourceforge.net/api/pyplot_api.html#matplotlib.pyplot.pcolor) function, but this plots from bottom to top (as you might expect) which is not ideal for what we want here. I’d have to turn the data and y axis labelling upside down, which is a little awkward:\
![pcolor()](http://psaffrey.files.wordpress.com/2010/07/pcolor.png?w=450)\
I finally plumped for [matshow()](http://matplotlib.sourceforge.net/api/pyplot_api.html#matplotlib.pyplot.matshow) (which stands for “matrix show”), which does things in the right order with solid blobs:\
![matshow()](http://psaffrey.files.wordpress.com/2010/07/matshow.png?w=450)\
However it’s clearly not without its problems. The xtick labels have not rotated, so I had to do it manually, like this:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3</p></td>
<td><p><code>im</code><code>=</code> <code>pylab.matshow(values)</code><br />
<code>for</code> <code>label</code><code>in</code> <code>im.axes.xaxis.get_ticklabels():</code><br />
    <code>label.set_rotation(</code><code>90</code><code>)</code></p></td>
</tr>
</tbody>
</table>

You might also be able to see that the top and bottom rows are half-height – I had to correct this by adding dummy row labels here. I don’t know why this happens.\
There are various other problems with the other plots too. I think it would be safe to say that matplotlib is a powerful tool but if you want plots in a particular way, it can take a lot of documentation bashing to get there. As always, I usually feel that the way I want it should be the standard and it should do that in the first place, but I can never tell how unreasonable I am being when I say this.\
An open question here is whether heatmaps are actually the best way to visualise this data. I’m also working on a student project at the moment where we use a graph-based visualisation instead. My theory is that we can use a [spring embedder](http://en.wikipedia.org/wiki/Force-based_algorithms_%28graph_drawing%29) algorithm to automatically lay out the graph of interactions between a pair of chromosomes and show their conformation. I think the usual wisdom here is that no visualisation is “right” – best to have several at your disposal to allow you to look at the data in a variety of different ways.

--------------------------------------------------------------
