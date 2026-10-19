---
title: check file and folder size in linux
author: Ming Tang
date: '2013-03-30T20:49:00-07:00'
lastmod: '2013-03-30T20:49:00-07:00'
slug: check-file-and-folder-size-in-linux
tags:
- unix
- Ubuntu
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/03/check-file-and-folder-size-in-linux.html
description: How to check file and folder size\
summary: How to check file and folder size\
header:
  caption: ''
  image: ''
---

How to check file and folder size\
<http://www.cyberciti.biz/faq/how-do-i-find-the-largest-filesdirectories-on-a-linuxunixbsd-filesystem/>

How do I find the largest top files and directories on a Linux or Unix like operating systems?

Sometime it is necessary to find out what file(s) or directories are eating up all your disk space. Further, it may be necessary to find out it at the particular location such as /tmp or /var or /home.\
There is no simple command available to find out the largest files/directories on a Linux/UNIX/BSD filesystem. However, combination of following three commands (using pipes) you can easily find out list of largest files:

- du : Estimate file space usage.
- sort : Sort lines of text files or given input data.
- head : Output the first part of files i.e. to display first 10 largest file.

Type the following command at the shell prompt to find out top 10 largest file/directories:\
`# du -a /var | sort -n -r | head -n 10`\
Output:

``` text
1008372 /var
313236  /var/www
253964  /var/log
192544  /var/lib
152628  /var/spool
152508  /var/spool/squid
136524  /var/spool/squid/00
95736   /var/log/mrtg.log
74688   /var/log/squid
62544   /var/cache
```

If you want more human readable output try:

``` text
$ cd /path/to/some/where
$ du -hsx * | sort -rh | head -10
```

Where,

- **du command -h option** : display sizes in human readable format (e.g., 1K, 234M, 2G).
- **du command -s option** : show only a total for each argument (summary).
- **du command -x option** : skip directories on different file systems.
- **sort command -r option** : reverse the result of comparisons.
- **sort command -h option** : compare human readable numbers. This is GNU sort specific option only.
- **head command -10 OR -n 10 option** : show the first 10 lines.

The above command will only work of GNU/sort is installed. Other Unix like operating system should use the following version (see comments below):

``` bash
for i in G M K; do du -ah | grep [0-9]$i | sort -nr -k 1; done | head -n 11
```

Updated for accuracy!
