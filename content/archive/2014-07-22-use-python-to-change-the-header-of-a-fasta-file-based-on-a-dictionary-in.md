---
title: use python to change the header of a fasta file based on a dictionary in another file
author: Ming Tang
date: '2014-07-22T12:37:00-07:00'
lastmod: '2014-07-22T12:37:00-07:00'
slug: use-python-to-change-the-header-of-a-fasta-file-based-on-a-dictionary-in
tags:
- fasta
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/07/use-python-to-change-header-of-fasta.html
description: I saw on SEQanwser, someone asked this question here, and I used python to resolve this problem.\ I have not written in python for so long, I've been using…
summary: I saw on SEQanwser, someone asked this question here, and I used python to resolve this problem.\ I have not written in python for so long, I've been using…
header:
  caption: ''
  image: ''
---

I saw on SEQanwser, someone asked this question [here](http://seqanswers.com/forums/showthread.php?p=145619&posted=1#post145619), and I used python to resolve this problem.\
I have not written in python for so long, I've been using R for a long time. This provide me a good practice opportunity.

The basic problem is to format some files, and I know it can be done by other tools like sed and awk ( see an awk solution below), but python just gives you much more flexibility in terms of what you can do and gives you more readability.\
myfasta file

\>comp1558_c0_seq1 len=204 path=\[4976:0-203\]\
ATTGTACTTAATCTA.....\
\>comp8142_c0_seq1 len=222 path=\[8835:0-221\]\
GTACAATCACGGCACATT......\
\>comp8570_c0_seq1 len=232 path=\[3344:0-232\]\
GCATCGCTTATCGGTTTA......

annotation file:

comp1558_c0_seq1 repressor protein\
comp8142_c0_seq1 aspartate aminotransferase\
comp8357_c0_seq1 l-xylulose reductase\
comp8387_c0_seq1 succinyl- synthetase alpha\
comp8570_c0_seq1 fatty acid synthase beta subunit dehydratase

see the code below:

{{< gist crazyhottommy a59df92b48e6ad3f4630 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/a59df92b48e6ad3f4630)

output:\
\>comp1558_c0_seq1 repressor protein\
ATTGTACTTAATCTA.....\
\>comp8142_c0_seq1 aspartate aminotransferase\
GTACAATCACGGCACATT......\
\>comp8570_c0_seq1 fatty acid synthase beta subunit dehydratase\
GCATCGCTTATCGGTTTA......

if the dictionary file has only two records for each line, this awk should work:\
<http://stackoverflow.com/questions/11678939/replace-text-based-on-a-dictionary>

Usage: `awk -f foo.awk dict.dat user.dat`\
<http://www.gnu.org/software/gawk/manual/html_node/String-Functions.html>\
<http://www.gnu.org/software/gawk/manual/html_node/Arrays.html>

``` perl
NR == FNR {
  rep[$1] = $2
  next
}

{
    for (key in rep) {
      gsub(key, rep[key])
    }
    print
}
```
