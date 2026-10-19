---
title: use Entrez Direct to access NCBI database
author: Ming Tang
date: '2015-01-26T06:44:00-08:00'
lastmod: '2015-01-26T06:44:00-08:00'
slug: use-entrez-direct-to-access-ncbi-database
tags:
- Entrez
- NCBI
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/01/use-entrez-direct-to-access-ncbi.html
description: I was reading Applied Bioinformatics 2014 lecture 3 and learned that one can use Entrez Direct to access NCBI database (Pubmed, nucleotide, protein sequence…
summary: I was reading Applied Bioinformatics 2014 lecture 3 and learned that one can use Entrez Direct to access NCBI database (Pubmed, nucleotide, protein sequence…
header:
  caption: ''
  image: ''
---

I was reading [Applied Bioinformatics 2014](http://www.personal.psu.edu/iua1/courses/2014-BMMB-852.html) lecture 3 and learned that one can use [Entrez Direct](http://www.ncbi.nlm.nih.gov/books/NBK179288/) to access NCBI database (Pubmed, nucleotide, protein sequence etc).

After installing Entrez Direct, I played around with it:

{{< gist crazyhottommy 18c3796c87b04ba1422f >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/18c3796c87b04ba1422f)

Commonly-used fields for PubMed queries include:

``` text
  [AFFL]  Affiliation       [FILT]  Filter              [MESH]  MeSH Terms
  [ALL]   All Fields        [JOUR]  Journal             [PTYP]  Publication Type
  [AUTH]  Author            [LANG]  Language            [WORD]  Text Word
  [FAUT]  Author - First    [MAJR]  MeSH Major Topic    [TITL]  Title
  [LAUT]  Author - Last     [SUBH]  MeSH Subheading     [TIAB]  Title/Abstract
```

` [PDAT] Date - Publication [UID] UID`

Filters that limit search results to subsets of PubMed include:

``` text
  humans [MESH]                has abstract [FILT]
  pharmacokinetics [MESH]      historical article [FILT]
  chemically induced [SUBH]    loprovflybase [FILT]
  all child [FILT]             randomized controlled trial [FILT]
  english [FILT]               clinical trial, phase ii [PTYP]
  free full text [FILT]        review [PTYP]
```

Sequence databases are indexed with a different set of search fields, including:

``` text
  [ACCN]  Accession       [GENE]  Gene Name            [PROT]  Protein Name
  [ALL]   All Fields      [JOUR]  Journal              [SQID]  SeqID String
  [AUTH]  Author          [KYWD]  Keyword              [SLEN]  Sequence Length
  [GPRJ]  BioProject      [MLWT]  Molecular Weight     [SUBS]  Substance Name
  [ECNO]  EC/RN Number    [ORGN]  Organism             [WORD]  Text Word
  [FKEY]  Feature Key     [PACC]  Primary Accession    [TITL]  Title
  [FILT]  Filter          [PROP]  Properties           [UID]   UID
```

and a sample query in the protein database is:

``` text
  "alcohol dehydrogenase [PROT] NOT (bacteria [ORGN] OR fungi [ORGN])"
```

``` text
Please refer to the documents for more examples http://www.ncbi.nlm.nih.gov/books/NBK179288/
```
