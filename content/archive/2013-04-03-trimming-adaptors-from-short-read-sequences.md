---
title: Trimming adaptors from short read sequences
author: Ming Tang
date: '2013-04-03T09:49:00-07:00'
lastmod: '2013-04-03T09:49:00-07:00'
slug: trimming-adaptors-from-short-read-sequences
tags:
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/trimming-adaptors-from-short-read.html
description: One common post processing step for next generation, or short read, sequencing is to remove adaptor sequences. Sample preparation for sequencing often…
summary: One common post processing step for next generation, or short read, sequencing is to remove adaptor sequences. Sample preparation for sequencing often…
header:
  caption: ''
  image: ''
---

<http://bcbio.wordpress.com/2009/08/09/trimming-adaptors-from-short-read-sequences/>

## [Blue Collar Bioinformatics](http://bcbio.wordpress.com/)

## Trimming adaptors from short read sequences

[with 11 comments](http://bcbio.wordpress.com/2009/08/09/trimming-adaptors-from-short-read-sequences/#comments)

One common post processing step for next generation, or short read, sequencing is to remove adaptor sequences. Sample preparation for sequencing often involves selection of a particular fraction of interest like expressed genes, microRNAs, or copy number variant regions. Selecting, amplifying, and sequencing these regions can result in read sequences containing a common tag that needs to be identified and removed before continuing with other analysis steps like alignment to the genome. This is often the case when the reads of interest are small like in short RNAs or small sequence tags.\
We start with a short known sequence (the adaptor) that will be present in our sequences of interest. For each read, we want to determine the position of this adaptor and remove it. Additionally, reads may contain the adaptor but differ by one or more bases; these differences are likely due to sequencing errors. So our solution needs to allow some fuzzy matching of the adaptor to the read sequences.\
My first attempt involved using fuzzy string matching. In Python, several libraries exist to tackle this problem. The normal use case is in identifying misspelled words in text. For instance, you can calculate [Levenshtein (edit) distance](http://code.google.com/p/pylevenshtein/) or use [difflib](http://docs.python.org/library/difflib.html) in the standard library; Stack Overflow has a [nice discussion](http://stackoverflow.com/questions/682367/good-python-modules-for-fuzzy-string-comparison) of the different modules. Ultimately this approach proved to be a dead end; the properties that make English words similar do not overlap well with those differentiate DNA sequences.\
That realization led me to tackle the problem by computing [local alignments](http://en.wikipedia.org/wiki/Sequence_alignment) of the adaptor to each sequence. This effectively captures the biological intuition you need to trim a sequence with less than a specified number of errors. The downside to this rigorous approach is that it will be slower than purely string based methods.\
The [Biopython](http://biopython.org/) library contains a Python local alignment function suitable for quick alignment of short regions. We use this to align an adaptor region to a sequence and calculate the number of differences in the aligned region. To handle a common case where we can find the exact adaptor sequence, we first do an string match. This avoids the alignment overhead in many cases:

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
23</p></td>
<td><p><code>from</code> <code>Bio</code><code>import</code> <code>pairwise2</code></p>
<p><code>def</code> <code>trim_adaptor(seq, adaptor, num_errors, right_side</code><code>=</code><code>True</code><code>):</code><br />
    <code>gap_char</code><code>=</code> <code>'-'</code><br />
    <code>exact_pos</code><code>=</code> <code>str</code><code>(seq).find(adaptor)</code><br />
    <code>if</code> <code>exact_pos &gt;</code><code>=</code> <code>0</code><code>:</code><br />
        <code>seq_region</code><code>=</code> <code>str</code><code>(seq[exact_pos:exact_pos</code><code>+</code><code>len</code><code>(adaptor)])</code><br />
        <code>adapt_region</code><code>=</code> <code>adaptor</code><br />
    <code>else</code><code>:</code><br />
        <code>seq_a, adaptor_a, score, start, end</code><code>=</code> <code>pairwise2.align.localms(</code><code>str</code><code>(seq),</code><br />
                <code>str</code><code>(adaptor),</code><code>5.0</code><code>,</code><code>-</code><code>4.0</code><code>,</code><code>-</code><code>9.0</code><code>,</code><code>-</code><code>0.5</code><code>,</code><br />
                <code>one_alignment_only</code><code>=</code><code>True</code><code>, gap_char</code><code>=</code><code>gap_char)[</code><code>0</code><code>]</code><br />
        <code>adapt_region</code><code>=</code> <code>adaptor_a[start:end]</code><br />
        <code>seq_region</code><code>=</code> <code>seq_a[start:end]</code><br />
    <code>matches</code><code>=</code> <code>sum</code><code>((</code><code>1</code> <code>if</code> <code>s</code><code>=</code><code>=</code> <code>adapt_region[i]</code><code>else</code> <code>0</code><code>)</code><code>for</code> <code>i, s</code><code>in</code><br />
            <code>enumerate</code><code>(seq_region))</code><br />
    <code># too many errors -- no trimming</code><br />
    <code>if</code> <code>(</code><code>len</code><code>(adaptor)</code><code>-</code> <code>matches) &gt; num_errors:</code><br />
        <code>return</code> <code>seq</code><br />
    <code># remove the adaptor sequence and return the result</code><br />
    <code>else</code><code>:</code><br />
        <code>return</code> <code>_remove_adaptor(seq, seq_region.replace(gap_char, ""),</code><br />
                <code>right_side)</code></p></td>
</tr>
</tbody>
</table>

If the alignment contains fewer than the specified number of errors we’ve found an adaptor and remove it, returning the trimmed sequence. The attribute `right_side` allows us to specify whether the trimming should be done on the right (3′ end) or the left (5′ end):

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
17</p></td>
<td><p><code>def</code> <code>_remove_adaptor(seq, region, right_side</code><code>=</code><code>True</code><code>):</code><br />
    <code>"""Remove an adaptor region and all sequence to the right or left.</code><br />
    <code>"""</code><br />
    <code>if</code> <code>right_side:</code><br />
        <code>try</code><code>:</code><br />
            <code>pos</code><code>=</code> <code>seq.find(region)</code><br />
        <code># handle Biopython SeqRecords</code><br />
        <code>except</code> <code>AttributeError:</code><br />
            <code>pos</code><code>=</code> <code>seq.seq.find(region)</code><br />
        <code>return</code> <code>seq[:pos]</code><br />
    <code>else</code><code>:</code><br />
        <code>try</code><code>:</code><br />
            <code>pos</code><code>=</code> <code>seq.rfind(region)</code><br />
        <code># handle Biopython SeqRecords</code><br />
        <code>except</code> <code>AttributeError:</code><br />
            <code>pos</code><code>=</code> <code>seq.seq.rfind(region)</code><br />
        <code>return</code> <code>seq[pos</code><code>+</code><code>len</code><code>(region):]</code></p></td>
</tr>
</tbody>
</table>

Here is an example script using this function along with Biopython to trim reads from a [FASTQ format](http://en.wikipedia.org/wiki/FASTQ_format) file.Each read is analyzed to determine if it contains the adaptor, with up to 2 errors. If the adaptor is identified and trimmed, the final useful read is written to a FASTA format output file.

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
22</p></td>
<td><p><code>from</code> <code>__future__</code><code>import</code> <code>with_statement</code><br />
<code>import</code> <code>sys</code><br />
<code>import</code> <code>os</code></p>
<p><code>from</code> <code>Bio</code><code>import</code> <code>SeqIO</code></p>
<p><code>from</code> <code>adaptor_trim</code><code>import</code> <code>trim_adaptor</code></p>
<p><code>def</code> <code>main(in_file):</code><br />
    <code>adaptor_seq</code><code>=</code> <code>"TCGTATGCCGTCTTCTGC"</code><br />
    <code>num_errors</code><code>=</code> <code>2</code><br />
    <code>base, ext</code><code>=</code> <code>os.path.splitext(in_file)</code><br />
    <code>out_file</code><code>=</code> <code>"%s-trimmed.fa"</code> <code>%</code> <code>(base)</code></p>
<p>    <code>with</code><code>open</code><code>(in_file) as in_handle:</code><br />
        <code>with</code><code>open</code><code>(out_file,</code><code>"w"</code><code>) as out_handle:</code><br />
            <code>for</code> <code>rec</code><code>in</code> <code>SeqIO.parse(in_handle,</code><code>"fastq"</code><code>):</code><br />
                <code>trim</code><code>=</code> <code>trim_adaptor(rec.seq, adaptor_seq, num_errors)</code><br />
                <code>if</code> <code>len</code><code>(trim) &gt;</code><code>0</code> <code>and</code> <code>len</code><code>(trim) &lt;</code><code>len</code><code>(rec):</code><br />
                    <code>rec.letter_annotations</code><code>=</code> <code>{}</code><br />
                    <code>rec.seq</code><code>=</code> <code>trim</code><br />
                    <code>SeqIO.write([rec], out_handle,</code><code>"fasta"</code><code>)</code></p></td>
</tr>
</tbody>
</table>

You can get the [full trimming code](http://github.com/chapmanb/bcbb/blob/master/align/adaptor_trim.py), along with tests, from GitHub. The script takes about a half hour to process 15 million 36bp reads. Other current trimming approaches are often integrated into the aligners themselves: for instance,[NovoAlign](http://www.novocraft.com/products.html#novoalign) appears to have similar alignment based [trimming capabilities](http://www.novocraft.com/wiki/tiki-index.php?page=3+Prime+Adapter+Stripping).
