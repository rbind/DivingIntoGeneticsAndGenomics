---
title: mount a remote server on mac using SSHFS
author: Ming Tang
date: '2015-09-03T14:33:00-07:00'
lastmod: '2015-09-03T14:33:00-07:00'
slug: mount-a-remote-server-on-mac-using-sshfs
tags:
- mac
- mount remote server
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/09/mount-remote-server-on-mac-using-sshfs.html
description: I work frequently with remote computing cluster. I want to mount a remote server to my local computer (a mac). In this post, I will show you how to use…
summary: I work frequently with remote computing cluster. I want to mount a remote server to my local computer (a mac). In this post, I will show you how to use…
header:
  caption: ''
  image: ''
---

I work frequently with remote computing cluster.  I want to mount a remote server to my local computer (a mac). In this post,  I will show you how to use SSHFS to achieve that.

First, you need to read OSXfuse [wiki](https://github.com/osxfuse/osxfuse/wiki/SSHFS). You might also want to read this [post](https://amaral.northwestern.edu/resources/guides/mounting-remote-folder-os-x-over-ssh).

For me, I have [Homebrew](http://brew.sh/) and [brew cask](http://caskroom.io/) installed, so I just need to:

``` text
brew cask install osxfuse
brew install sshfs
```

Then, go to [Macfusion](http://macfusionapp.org/). Install [macfuse](http://code.google.com/p/macfuse/downloads/detail?name=MacFUSE-2.0.3%2C2.dmg) first. Download Macfusion, unzip it and put it to your Application folder.

In the terminal, type commands below:

``` text
cd /Applications/Macfusion.app/Contents/PlugIns/sshfs.mfplugin/Contents/Resources
mv sshfs-static sshfs-static.orig
ln -s /usr/local/bin/sshfs sshfs-static
```

You then can click the Macfusion GUI and set up the server. If everything is OK, you will see the server is mounted in the /Volumes folder. If you want /Volumes folder to be shown under the Finder.\
[on command line do](http://www.milkcarton.com/blog/CommentView,guid,11135e07-6332-4c64-a883-61810d48b80c.aspx):\
sudo SetFile -a v /Volumes

![](/img/archive/mount-a-remote-server-on-mac-using-sshfs/01-screenshot-2015-09-03-16.29.00.png)
