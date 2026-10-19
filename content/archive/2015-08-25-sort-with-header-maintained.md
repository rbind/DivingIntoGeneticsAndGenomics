---
title: sort with header maintained
author: Ming Tang
date: '2015-08-25T13:36:00-07:00'
lastmod: '2015-08-25T13:36:00-07:00'
slug: sort-with-header-maintained
tags:
- awk
- unix
- sort
- VCF
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/08/sort-with-header-maintained.html
description: We usually want to maintain header when we do something for the body of the text.\ You can find a body function here which can only accept streams and…
summary: We usually want to maintain header when we do something for the body of the text.\ You can find a body function here which can only accept streams and…
header:
  caption: ''
  image: ''
---

We usually want to maintain header when we do something for the body of the text.\
You can find a [body function](https://github.com/crazyhottommy/data-science-at-the-command-line/blob/master/tools/body) here which can only accept streams and assumes only one line header.

Use [subshells](http://bash.cyberciti.biz/guide/What_is_a_Subshell%3F) and awk can do the same job and potentially more flexible.

{{< gist crazyhottommy 85de6ed22efa541ada17 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/85de6ed22efa541ada17)

The original credits go to [Aaron Quinlan](https://gist.github.com/arq5x/ab78b7be4d4f2a989a31). see a gist below to **sort vcf files in natural chromosome order** :chr1 chr2 chr3.... rather than chr1 chr10 chr11....

{{< gist crazyhottommy e778ceb39cebfa20739a >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/e778ceb39cebfa20739a)
