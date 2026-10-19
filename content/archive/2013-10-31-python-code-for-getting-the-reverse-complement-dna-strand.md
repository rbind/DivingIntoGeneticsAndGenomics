---
title: python code for getting the reverse complement DNA strand
author: Ming Tang
date: '2013-10-31T12:58:00-07:00'
lastmod: '2013-10-31T12:58:00-07:00'
slug: python-code-for-getting-the-reverse-complement-dna-strand
tags:
- computation skills for biologist
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/python-code-for-getting-reverse.html
description: 'Following pythonforbiologist , I wrote a very simple python script to get the reverse complement DNA strand:'
summary: 'Following pythonforbiologist , I wrote a very simple python script to get the reverse complement DNA strand:'
header:
  caption: ''
  image: ''
---

Following pythonforbiologist <http://pythonforbiologists.com/index.php/business-card-solutions/>, I wrote a very simple python script to get the reverse complement DNA strand:

{{< gist crazyhottommy 7255638 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/7255638)

It is a very simple script but several things I want to mention:\
1. what is the fast string concatenation method in python?\
in the first function, we tend to write something like:

out_seq = ' '\
for base in reversed(seq):\
    out_seq += seq_dict\[base\]

but it turned out to be very slow. Read this post for more details  <http://www.skymind.com/~ocrow/python_string/>\
the list comprehension is the fastest and most elegant way to concatenate a string.

2. for more complicated IUPAC ambiguity codes, read this post:\
<http://pythonforbiologists.com/index.php/how-to-count-non-dna-bases-in-a-sequence-using-python/>

``` text
Nucleotide Code:  Base:
----------------  -----
A.................Adenine
C.................Cytosine
G.................Guanine
T (or U)..........Thymine (or Uracil)
R.................A or G
Y.................C or T
S.................G or C
W.................A or T
K.................G or T
M.................A or C
B.................C or G or T
D.................A or G or T
H.................A or C or T
V.................A or C or G
N.................any base
. or -............gap
```
