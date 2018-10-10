---
title: open files on remote with sublime by ssh
author: Ming Tang
date: '2018-10-10'
slug: open-files-on-remote-with-sublime-by-ssh
categories:
  - HPC
tags:
  - ssh
header:
  caption: ''
  image: ''
---

I am still suck at `vim` or `emacs`. I use `nano` to edit files on remote machines. But for more complicated editing, I prefer to use sublime.

use this https://github.com/randy3k/RemoteSubl for editing remote files.

Steps:

### on remote machine, install `rmate`
```bash
ssh bio1
curl -o ~/bin/rmate https://raw.githubusercontent.com/aurora/rmate/master/rmate

chmod u+x bin/rmate
```

### on your local computer, install `RemoteSubl`

on your **local** computer, open `sublime`, click `tools` --> `Command Palette` --> type Package control:Install Package --> type `RemoteSubl` to install.

### change your ssh config file

add `RemoteForward 52698 localhost:52698` to your `~/.ssh/config` file.

Now,  ssh to remote, and you can do `rmate my.txt` in your remote and open sublime in your local machine.
