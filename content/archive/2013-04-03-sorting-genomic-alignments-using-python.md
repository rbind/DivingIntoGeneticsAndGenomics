---
title: Sorting genomic alignments using Python
author: Ming Tang
date: '2013-04-03T09:53:00-07:00'
lastmod: '2013-04-03T09:53:00-07:00'
slug: sorting-genomic-alignments-using-python
tags:
- genomics
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/sorting-genomic-alignments-using-python.html
description: Recent, I was asked to sort a MAF (Multiple Alignment Format) file containing genomic alignments. This was a large vertebrate alignment file and the…
summary: Recent, I was asked to sort a MAF (Multiple Alignment Format) file containing genomic alignments. This was a large vertebrate alignment file and the…
header:
  caption: ''
  image: ''
---

## <http://bcbio.wordpress.com/2009/07/26/sorting-genomic-alignments-using-python/>

## [Blue Collar Bioinformatics](http://bcbio.wordpress.com/)

## Sorting genomic alignments using Python

[leave a comment »](http://bcbio.wordpress.com/2009/07/26/sorting-genomic-alignments-using-python/#comments)

Recent, I was asked to sort a [MAF (Multiple Alignment Format)](https://cgwb.nci.nih.gov/goldenPath/help/maf.html) file containing genomic alignments. This was a large vertebrate alignment file and the researchers were interested in manually examining the longest matches. The tricky part of doing this is that the entire file cannot comfortably fit in memory on a standard machine.\
The common solution to dealing with large memory intensive files is to pre-parse the file and provide a way to look up individual records based on an identifier. For instance, items could be pushed into a relational database table and retrieved based on primary keys. While moving to disk access is less efficient, it allows your code to scale to these type of real life problems without resorting to buying large memory machines.\
We will use the [bx-python](http://bitbucket.org/james_taylor/bx-python/wiki/Home) library to read the MAF alignment files, prepare an index for accessing the alignments individually, and ultimately perform the sorting.\
The first step is to define the index retrieval method. bx-python has [index retrival functionality](http://bitbucket.org/james_taylor/bx-python/src/tip/lib/bx/interval_index_file.py) that allows querying and retrieving records based on genomic location. We don’t need that advanced query capability here, and can retrieve records based on position in the file as well. We build the index by looping through each record and recording the file position and genomic locations in the index.

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
18</p></td>
<td><p><code>from</code> <code>bx.align</code><code>import</code> <code>maf</code><br />
<code>from</code> <code>bx</code><code>import</code> <code>interval_index_file</code></p>
<p><code>def</code> <code>build_index(in_file, index_file):</code><br />
    <code>indexes</code><code>=</code> <code>interval_index_file.Indexes()</code><br />
    <code>with</code><code>open</code><code>(in_file) as in_handle:</code><br />
        <code>reader</code><code>=</code> <code>maf.Reader(in_handle)</code><br />
        <code>while</code> <code>1</code><code>:</code><br />
            <code>pos</code><code>=</code> <code>reader.</code><code>file</code><code>.tell()</code><br />
            <code>rec</code><code>=</code> <code>reader.</code><code>next</code><code>()</code><br />
            <code>if</code> <code>rec</code><code>is</code> <code>None</code><code>:</code><br />
                <code>break</code><br />
            <code>for</code> <code>c</code><code>in</code> <code>rec.components:</code><br />
                <code>indexes.add(c.src, c.forward_strand_start,</code><br />
                        <code>c.forward_strand_end, pos,</code><code>max</code><code>=</code><code>c.src_size )</code></p>
<p>    <code>with</code><code>open</code><code>(index_file,</code><code>"w"</code><code>) as index_handle:</code><br />
        <code>indexes.write(index_handle)</code></p></td>
</tr>
</tbody>
</table>

Now we have a unique index to retrieve a record — the position in the file. The second step is to loop through the file again and build a list of alignment sizes and positions. Alignment size is the first value in each list item, since this is the criteria to sort by. This list easily fits in memory, and we sort it with the largest alignments first.

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
10</p></td>
<td><p><code>rec_info</code><code>=</code> <code>[]</code><br />
<code>with</code><code>open</code><code>(in_file) as in_handle:</code><br />
    <code>reader</code><code>=</code> <code>maf.Reader(in_handle)</code><br />
    <code>while</code> <code>1</code><code>:</code><br />
        <code>pos</code><code>=</code> <code>reader.</code><code>file</code><code>.tell()</code><br />
        <code>rec</code><code>=</code> <code>reader.</code><code>next</code><code>()</code><br />
        <code>if</code> <code>rec</code><code>is</code> <code>None</code><code>:</code><br />
            <code>break</code><br />
        <code>rec_info.append((rec.text_size, pos))</code><br />
<code>rec_info.sort(reverse</code><code>=</code><code>True</code><code>)</code></p></td>
</tr>
</tbody>
</table>

Finally, we loop though the sorted list of sizes and use the associated positions to get records from our index. Each record is then written to an output file.

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
6</p></td>
<td><p><code>index</code><code>=</code> <code>maf.Indexed(in_file, index_file)</code><br />
<code>with</code><code>open</code><code>(out_file,</code><code>"w"</code><code>) as out_handle:</code><br />
    <code>writer</code><code>=</code> <code>maf.Writer(out_handle)</code><br />
    <code>for</code> <code>size, pos</code><code>in</code> <code>rec_info:</code><br />
        <code>rec</code><code>=</code> <code>index.get_at_offset(pos)</code><br />
        <code>writer.write(rec)</code></p></td>
</tr>
</tbody>
</table>

The [full script](http://github.com/chapmanb/bcbb/blob/master/align/maf_sort_by_size.py) puts this together into a usable program. This could be adjusted to deal with other alignment formats supported by bx-python, like [axt files](http://genome.ucsc.edu/goldenPath/help/axt.html).
