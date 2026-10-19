---
title: slow google chromium in ubuntu solved
author: Ming Tang
date: '2013-04-23T06:49:00-07:00'
lastmod: '2013-04-23T06:49:00-07:00'
slug: slow-google-chromium-in-ubuntu-solved
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/slow-google-chromium-in-ubuntu-solved.html
description: My google chromium browser in my ubuntu 12.04 LTS frequently dies, and I have to restart my computer.
summary: My google chromium browser in my ubuntu 12.04 LTS frequently dies, and I have to restart my computer.
header:
  caption: ''
  image: ''
---

My google chromium browser in my ubuntu 12.04 LTS frequently dies, and I have to restart my computer.

google is the best teacher :)\
http://falcon1986.wordpress.com/2010/06/01/how-to-speed-up-google-chrome-on-ubuntu/

## \[HOW-TO\] Speed up Google Chrome on Ubuntu

[with 32 comments](http://falcon1986.wordpress.com/2010/06/01/how-to-speed-up-google-chrome-on-ubuntu/#comments)

![Ubuntu Logo](http://falcon1986.files.wordpress.com/2010/02/ubuntu-logo.png?w=700 "Ubuntu Logo")Although I use Mozilla Firefox more than I use [Google Chrome](http://www.google.com/chrome), the latter does come in handy when testing how websites display on a different browser or when there is need to diagnose internet connectivity problems. On Windows, Google Chrome is fast; perhaps even faster than Mozilla Firefox. However, on Ubuntu 9.10, I have noticed that it can be a little slow to initiate or even load a simple web page. In an effort to find a solution to this problem I stumbled across quite a few user-submitted posts that described a simple fix. After applying the fix, there was a significant improvement. Read on to find out how you can speed up Google Chrome on Ubuntu.\
Apparently, the slow response of Google Chrome on Ubuntu has to do with the slow DNS lookup of this browser. These instructions should not be limited to Chrome on Ubuntu 9.10 alone and should work on any version of Ubuntu where the latest version of Chrome is installed.

1.  Close any instance of Google Chrome that might be running.

2.  Open a terminal window by going to *Applications \> Accessories \> Terminal*.

3.  Enter the following command and press ENTER. It will open **nsswitch.conf** in gedit.

    `sudo gedit /etc/nsswitch.conf`

4.  Locate the following line within the file:

    `hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4`

5.  Replace that line with the following, save the file and exit gedit and the terminal.

    `hosts: files dns`

6.  Start up Google Chrome and experience a faster browsing experience!

----------------------------------------------------------------------------------------------------------
it worked for me.
