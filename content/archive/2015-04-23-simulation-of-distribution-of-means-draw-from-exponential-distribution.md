---
title: simulation of distribution of means draw from exponential distribution
author: Ming Tang
date: '2015-04-23T12:48:00-07:00'
lastmod: '2015-04-23T12:48:00-07:00'
slug: simulation-of-distribution-of-means-draw-from-exponential-distribution
tags:
- Central limit theorem
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/04/simulation-of-distribution-of-means.html
description: '"central limit theorem (CLT) states that, given certain conditions, the arithmetic mean of a sufficiently large number of iterates of independent random…'
summary: '"central limit theorem (CLT) states that, given certain conditions, the arithmetic mean of a sufficiently large number of iterates of independent random…'
header:
  caption: ''
  image: ''
---

"central limit theorem (CLT) states that, given certain conditions, the arithmetic mean of a sufficiently large number of iterates of independent random variables, each with a well-defined expected value and well-defined variance, will be approximately normally distributed, regardless of the underlying distribution." \[1\]

I am going to draw 40 numbers from exponential distribution \[2\] (you can do it for any distribution) for 1000 times and examine the distribution of the means. In R, you can do it by rexp(40, lambda),  where lambda is the rate parameter. The mean of exponential distribution is 1/lambda and the standard deviation is also 1/lambda. **Set lambda = 0.2 for all of the simulations for this specific test.**

see a gist below.

{{< gist crazyhottommy 0074d7cc752e648a1297 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/0074d7cc752e648a1297)

![](/img/archive/simulation-of-distribution-of-means-draw-from-exponential-distribution/01-1.png)\
![](/img/archive/simulation-of-distribution-of-means-draw-from-exponential-distribution/02-2.png)

\[1\] http://en.wikipedia.org/wiki/Central_limit_theorem\
\[2\] http://en.wikipedia.org/wiki/Exponential_distribution
