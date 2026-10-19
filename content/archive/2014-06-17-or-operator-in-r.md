---
title: '&  or && operator in R'
author: Ming Tang
date: '2014-06-17T12:57:00-07:00'
lastmod: '2014-06-17T12:57:00-07:00'
slug: or-operator-in-r
tags:
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/06/or-operator-in-r.html
description: I will defend next Tuesday. To relieve my anxiety a little bit, I decide to blog a little bit.
summary: I will defend next Tuesday. To relieve my anxiety a little bit, I decide to blog a little bit.
header:
  caption: ''
  image: ''
---

I will defend next Tuesday. To relieve my anxiety a little bit, I decide to blog a little bit.

I am confused about the operator & and && in R, which one should I use?

It turns out:\
["The shorter ones are vectorized, meaning they can return a vector, The longer form evaluates left to right examining only the first element of each vector"](http://stackoverflow.com/questions/6558921/r-boolean-operators-and)

an example would suffice to explain it.\
`> x<-c(1:10)`\
`> x`\
` [1] 1 2 3 4 5 6 7 8 9 10`\
`> x>8`\
` [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE TRUE TRUE`\
`> logical1<- x>8`\
`> logical2<- x<5`\
`> logical1`\
` [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE TRUE TRUE`\
`> logical2`\
` [1] TRUE TRUE TRUE TRUE FALSE FALSE FALSE FALSE FALSE FALSE`\
`> logical1 | logical2`\
` [1] TRUE TRUE TRUE TRUE FALSE FALSE FALSE FALSE TRUE TRUE`\
`> logical1 & logical2`\
` [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE`\
`> logical1 && logical2`\
`[1] FALSE`\
one can subset the vector by the logical vectors:\
`> x[logical1]`\
`[1] 9 10`\
`> x[logical2]`\
`[1] 1 2 3 4`
