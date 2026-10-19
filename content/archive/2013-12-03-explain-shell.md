---
title: explain shell
author: Ming Tang
date: '2013-12-03T19:13:00-08:00'
lastmod: '2013-12-03T19:13:00-08:00'
slug: explain-shell
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/12/explain-shell.html
description: I just got to know this website from twitter explain shell\ It is very useful, especially when I have a complex linux command and can not figure out what\…
summary: I just got to know this website from twitter explain shell\ It is very useful, especially when I have a complex linux command and can not figure out what\…
header:
  caption: ''
  image: ''
---

I just got to know this website from twitter [explain shell](http://explainshell.com/)\
It is very useful, especially when I have a complex linux command and can not figure out what\
does each flag mean.

For example:  du -h -d 1 ~

![](/img/archive/explain-shell/01-selection_048.png)

This command is very useful to check the folder sizes.\
you can sort the folder by size:

`tommy@tommy-ThinkPad-T420:~$ du -h -d 1 | sort -hr`\
`63G .`\
`16G ./homer`\
`14G ./Desktop`\
`13G ./Downloads`\
`3.3G ./MochiView_v1.46`\
`1.9G ./anaconda`\
`1.2G ./scientific_writing`\
`1022M ./.cache`\
`938M ./SeqMonk`\
`845M ./mochi_view`\
`625M ./jorg_bungert`\
`587M ./.thunderbird`\
`400M ./.local`\
`379M ./R`\
`331M ./Datasets`\
`257M ./gatk-protected`

`........`
