---
title: choose random lines from a file
author: Ming Tang
date: '2013-10-07T08:42:00-07:00'
lastmod: '2013-10-07T08:42:00-07:00'
slug: choose-random-lines-from-a-file
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/choose-random-lines-from-file.html
description: I have a bed file which contains promoter regions. I want to get 1000 random promoters from it. First I think I would write a python script using the random…
summary: I have a bed file which contains promoter regions. I want to get 1000 random promoters from it. First I think I would write a python script using the random…
header:
  caption: ''
  image: ''
---

I have a bed file which contains promoter regions. I want to get 1000 random promoters from it. First I think I would write a python script using the random module, but before I do it, I did a quick google search since it is such an ordinary job, there may be tools out there already.

<http://stackoverflow.com/questions/9245638/select-random-lines-from-a-file-in-bash>\
<http://stackoverflow.com/questions/448005/whats-an-easy-way-to-read-random-line-from-a-file-in-unix-command-line>

I like the sort  -R solution

Sort the file randomly and pick first `100` lines:

``` text
$ sort -R input | head -n 100 >output
```

but it would be very slow if the file is very huge. The promoter file only contains 50k lines, so it is not a very big deal.

It turns out that shuf is a built-in command for this kind of task.

Use [`shuf`](http://linux.die.net/man/1/shuf) as shown below, to get `N` random lines:

``` text
shuf -n N input > output
```

of course, there are other solutions using awk and sed, but why not use this simple one:)\
Linux is awesome!

By the way, the random command from the bedtools <http://bedtools.readthedocs.org/en/latest/content/tools/random.html>  generates random bed Intervals from a genome. If that's what you want, use it.
