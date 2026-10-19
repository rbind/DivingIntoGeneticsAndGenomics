---
title: sshfs on ubuntu and ssh key
author: Ming Tang
date: '2017-04-19T13:54:00-07:00'
lastmod: '2017-04-19T13:54:00-07:00'
slug: sshfs-on-ubuntu-and-ssh-key
tags:
- ssh
- sshfs
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/04/sshfs-on-ubuntu-and-ssh-key.html
description: 'Two things I want to keep a note here:'
summary: 'Two things I want to keep a note here:'
header:
  caption: ''
  image: ''
---

Two things I want to keep a note here:

First, if you ever have set up a shh key for connecting to remote server, you need to be aware that password-less shh key only works when your home directory on the server is not 777 (writable by others).

see this [stackexchange post ](https://unix.stackexchange.com/questions/37164/ssh-and-home-directory-permissions).

Second, I was following https://www.cyberciti.biz/faq/how-to-mount-remote-directory-filesystems-with-sshfs-on-linux/ to set up sshfs on my ubuntu machine. I put down a gist below.

{{< gist crazyhottommy 271166a3a168ef8f9c1b03b761786559 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/271166a3a168ef8f9c1b03b761786559)
