---
title: Three ways to change a space delimited file to a tab delimited file
author: Ming Tang
date: '2013-09-26T11:24:00-07:00'
lastmod: '2013-09-26T11:24:00-07:00'
slug: three-ways-to-change-a-space-delimited-file-to-a-tab-delimited-file
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/09/three-ways-to-change-space-delimited.html
description: 'Let''s create a space delimited file:'
summary: 'Let''s create a space delimited file:'
header:
  caption: ''
  image: ''
---

Let's create a space delimited file:

`tommy@tommy-ThinkPad-T420:~$ cat > new.txt`\
`chr1 100 200`\
`chr2 300 400`\
`chr3 240 700`

ctrl + D to indicate cat that this is the last line.

use awk:

`tommy@tommy-ThinkPad-T420:~$ cat new.txt | awk -F ' ' '{print $1"\t"$2"\t"$3}'`\
`chr1 100 200`\
`chr2 300 400`\
`chr3 240 700`

use sed:

`tommy@tommy-ThinkPad-T420:~$ cat new.txt | sed 's/ /\t/g'`\
`chr1 100 200`\
`chr2 300 400`\
`chr3 240 700`

use tr :

`tommy@tommy-ThinkPad-T420:~$ cat new.txt | tr ' ' '\t'`\
`chr1 100 200`\
`chr2 300 400`\
`chr3 240 700`

`tommy@tommy-ThinkPad-T420:~$ rm new.txt`

you can redirect the newly generated tab delimited file to a new file\
`tommy@tommy-ThinkPad-T420:~$ cat new.txt | tr ' ' '\t' > new1.txt`

Linux commands are so cool!
