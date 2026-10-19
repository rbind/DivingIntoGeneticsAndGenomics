---
title: convert html to pdf by pandoc
author: Ming Tang
date: '2015-11-02T14:27:00-08:00'
lastmod: '2015-11-02T14:27:00-08:00'
slug: convert-html-to-pdf-by-pandoc
tags:
- html
- pdf
- pdflatex
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/11/convert-html-to-pdf-by-pandoc.html
description: 'Pandoc is a very useful tool to convert common formats.\ First install pandoc on mac by:'
summary: 'Pandoc is a very useful tool to convert common formats.\ First install pandoc on mac by:'
header:
  caption: ''
  image: ''
---

[Pandoc](http://pandoc.org/) is a very useful tool to convert common formats.\
First install pandoc on mac by:

``` text
brew install pandoc
```

pandoc requires pdflatex to convert to pdfs.\
install [mactex](https://tug.org/mactex/):\
download it and just double click it should install it.\
next, put executables including latexpdf to the path.

``` text
echo export PATH=$PATH:/usr/texbin/ >> .bashrc
source ~/.bashrc
```

You can [specify margins](http://pandoc.org/faqs.html) of the pdf by `-V geometry:margin=1in`:

``` text
pandoc http://quinlanlab.org/tutorials/cshl2013/gemini.html -V geometry:margin=1in -o gemini.pdf`
```

### when codes in html are too long, they get cut-off

Very thankful, I found the answer in this [post](http://tex.stackexchange.com/questions/179926/pandoc-markdown-to-pdf-without-cutting-off-code-block-lines-that-are-too-long):\
Save the following as `listings-setup.tex`

``` text
% Contents of listings-setup.tex
\usepackage{xcolor}

\lstset{
    basicstyle=\ttfamily,
    numbers=left,
    keywordstyle=\color[rgb]{0.13,0.29,0.53}\bfseries,
    stringstyle=\color[rgb]{0.31,0.60,0.02},
    commentstyle=\color[rgb]{0.56,0.35,0.01}\itshape,
    numberstyle=\footnotesize,
    stepnumber=1,
    numbersep=5pt,
    backgroundcolor=\color[RGB]{248,248,248},
    showspaces=false,
    showstringspaces=false,
    showtabs=false,
    tabsize=2,
    captionpos=b,
    breaklines=true,
    breakatwhitespace=true,
    breakautoindent=true,
    escapeinside={\%*}{*)},
    linewidth=\textwidth,
    basewidth=0.5em,
}
```

Then, invoke pandoc:

``` text
pandoc https://cran.r-project.org/web/packages/gapmap/vignettes/tcga_example.html --listings -H listings-setup.tex -o gapmap_TCGA.pdf
```
