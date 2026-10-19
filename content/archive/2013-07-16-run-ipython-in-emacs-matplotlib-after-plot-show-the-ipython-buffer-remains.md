---
title: run ipython in emacs, matplotlib after plot.show( ), the ipython buffer remains hanging
author: Ming Tang
date: '2013-07-16T07:31:00-07:00'
lastmod: '2013-07-16T07:31:00-07:00'
slug: run-ipython-in-emacs-matplotlib-after-plot-show-the-ipython-buffer-remains
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/07/run-ipython-in-emacs-matplotlib-after.html
description: I was learning how to plot in python using the matplotlib library.\ when I started in the terminal with
summary: I was learning how to plot in python using the matplotlib library.\ when I started in the terminal with
header:
  caption: ''
  image: ''
---

I was learning how to plot in python using the matplotlib library.\
when I started in the terminal with

`tommy@tommy-ThinkPad-T420:~$ ipython --pylab`

`In [1]: plot(range(4))`\
`Out[1]: [<matplotlib.lines.Line2D at 0x37b4b50>]`\
the figure showed up automatically, and the prompt was active in the shell.

However, when I run it through emacs, the figure did not show up until I typed plot.show( ),\
and the ipython buffer is inactive unless you close the figure.  If you type plot.show( ) again,\
nothing will show up.

A quick google:

<http://stackoverflow.com/questions/9753885/pylab-matplotlib-show-waits-until-window-closes>

" Add `pylab.ion()` ([interactive mode](http://matplotlib.sourceforge.net/api/pyplot_api.html#matplotlib.pyplot.ion)) before the `pylab.show()` call. That will make the UI run in a separate thread and the call to `show` will return immediately."
