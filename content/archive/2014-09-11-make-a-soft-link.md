---
title: make a soft link
author: Ming Tang
date: '2014-09-11T12:07:00-07:00'
lastmod: '2014-09-11T12:07:00-07:00'
slug: make-a-soft-link
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/09/make-soft-link.html
description: I got to know the ln linux command when I was reading The linux command line. I did not realize why I need this command to make links to the same data or file.
summary: I got to know the ln linux command when I was reading The linux command line. I did not realize why I need this command to make links to the same data or file.
header:
  caption: ''
  image: ''
---

I got to know the ln linux command when I was reading [The linux command line](http://www.amazon.com/The-Linux-Command-Line-Introduction/dp/1593273). I did not realize why I need this command to make links to the same data or file.

One situation I faced is that I have a lot of bam files  with long names in one folder, and when I execute a program in another folder, I have to pass the program a full path to the bam files.\
while you think the TAB auto-completion helps most of the time,  it is still too much to specify the full path + long names (ugly). So I want to make a copy of the bam files in the current working director to save me from typing the full path. This is where ln command comes to rescue.

The other scenario is when you have multiple executable files in one folder ~/myprogram/bin, you want to execute the program from anywhere, you could link it to /usr/local/bin, or you can just copy the executable to /usr/local/bin or you can add ~/myprogram/bin to $PATH. see a link here <http://unix.stackexchange.com/questions/116508/adding-to-path-vs-linking-from-bin>

Let me demonstrate the usage of ln 

look at the man page

man ln

\# I have two files in the playground directory and make two directories 

mkdir dir1 dir2\
ls

output: tommy@tommy-ThinkPad-T420\[playground\] ls                              \[ 2:32PM\]\
dir1  dir2  genes_h.txt  genes.txt

I then move the two txt files into dir1 and redirect to dir2 and make soft links of files in dir1 to dir2

mv \*txt dir1\
cd dir2\
ln -s ../dir1/\*txt .\
ls

if we go to dir1 and make a softlink from there

cd ../dir1\
ln -s ./\*txt  ../dir2

The links will not work (too many levels of symbolic levels error message if you want to use the link ), instead you need to specify the full path

ln -s $PWD/\*txt  ../dir2

see links here <http://www.thegeekstuff.com/2010/10/linux-ln-command-examples/>\
and <http://www.thegeekstuff.com/2010/10/linux-ln-command-examples/>
