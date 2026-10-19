---
title: IBash Notebook for reproducible research
author: Ming Tang
date: '2015-05-04T20:48:00-07:00'
lastmod: '2015-05-04T20:48:00-07:00'
slug: ibash-notebook-for-reproducible-research
tags:
- bash
- Ipython notebook
- unix
- reproducible research
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/05/ibash-notebook-for-reproducible-research.html
description: I use command line a lot. It is awesome for data processing, text data formatting and even exploratory data analysis.
summary: I use command line a lot. It is awesome for data processing, text data formatting and even exploratory data analysis.
header:
  caption: ''
  image: ''
---

I use command line a lot. It is awesome for data processing, text data formatting and even exploratory data analysis.

Last week, one of my colleagues complained that she forgot how she got the data and processed the data. With a future "ME" in mind, one needs to do extensive documentations of where, when and how you download and process the data, and document the versions of the tools used in the analysis.

Although there are many ways to make tasks on command line reproducible such as using  [Drake](https://github.com/Factual/drake) and [GNU make](http://www.gnu.org/software/make/), it is still not as straightforward as using Ipython Notebook for python and R markdown files for R, respectively.

Luckily, I got to know from [Jeroen Janssens](http://jeroenjanssens.com/2015/02/19/ibash-notebook.html), who wrote the ["Data science at command line"](http://datascienceatthecommandline.com/) Book, that there is a bash_kernal for Ipython notebook, and I gave it a try.\
see a screenshot of the notebook:

![](/img/archive/ibash-notebook-for-reproducible-research/01-screenshot-2015-05-04-22.52.37.png)

see the whole notebook here: <http://nbviewer.ipython.org/gist/crazyhottommy/71e0dcb6d678c137733c#>

Essentially, I copied the .ipynb file (it is a JSON file) and pasted it to a [gist,](https://gist.github.com/crazyhottommy/71e0dcb6d678c137733c) and insert the gist link to the nbviewer website <http://nbviewer.ipython.org/>

With IBash Notebook, one can document the linux commands in a real-time manner and make his research more reproducible!
