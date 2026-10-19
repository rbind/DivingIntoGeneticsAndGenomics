---
title: Run APE plasmid editor in linux
author: Ming Tang
date: '2013-05-29T08:06:00-07:00'
lastmod: '2013-05-29T08:06:00-07:00'
slug: run-ape-plasmid-editor-in-linux
tags:
- APE
- Ubuntu
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/run-ape-plasmid-editor-in-linux.html
description: I used to like APE on windows. I want to run it on linux (ubuntu 12.04 LTS) also.
summary: I used to like APE on windows. I want to run it on linux (ubuntu 12.04 LTS) also.
header:
  caption: ''
  image: ''
---

I used to like APE on windows.  I want to run it on linux (ubuntu 12.04 LTS) also.

A quick google search:\
<http://www.allandebono.org/ubuntu,ape/plasmid-editor/>

I followed the instruction.

first download the APE for linux here <http://biologylabs.utah.edu/jorgensen/wayned/ape/Download/>

unzip it, you will find two folders, one for Linux, the other one for Mac_OS

copy the "APE linux" folder to  your home directory:  /home/tommy/APE_linux.    I rename it as APE_linux, as linux "hates" the empty space.

`cd /usr/bin`\
`ls tclsh8.5`

if you see it is there, you are fine. otherwise you have to install it:

*`sudo apt-get install tk8.5 tcl8.5`*\
*  *
`tclsh8.5 /home/tommy/APE_linux/AppMain.tcl`\
*  *

![](/img/archive/run-ape-plasmid-editor-in-linux/01-selection_010.png)

*  *
*  *
