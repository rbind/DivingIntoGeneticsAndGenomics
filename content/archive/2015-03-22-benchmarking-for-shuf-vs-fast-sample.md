---
title: benchmarking for shuf vs fast_sample
author: Ming Tang
date: '2015-03-22T20:43:00-07:00'
lastmod: '2015-03-22T20:43:00-07:00'
slug: benchmarking-for-shuf-vs-fast-sample
tags:
- unix
- shuf
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/03/benchmarking-for-shuf-vs-fastsample.html
description: Sometimes we want to randomly select a proportion of lines from a txt file. The easiest way is to use the Unix command shuf. On a mac machine, you can…
summary: Sometimes we want to randomly select a proportion of lines from a txt file. The easiest way is to use the Unix command shuf. On a mac machine, you can…
header:
  caption: ''
  image: ''
---

Sometimes we want to randomly select a proportion of lines from a txt file. The easiest way is to use the Unix command shuf. On a mac machine, you can install it by home brew.

``` text
brew install coreutils
```

But you need to invoke it as gshuf. <https://www.topbug.net/blog/2013/04/14/install-and-use-gnu-command-line-tools-in-mac-os-x/>\
I also came across a tool called fast_sample that can do the same thing\
<https://github.com/earino/fast_sample>\
I did some benchmarking for them.

{{< gist crazyhottommy cd02742611c9707290be >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/cd02742611c9707290be)

The take home message for the benchmarking is that Unix tools sometimes are better than tools you write in terms of speed and memory efficiency.
