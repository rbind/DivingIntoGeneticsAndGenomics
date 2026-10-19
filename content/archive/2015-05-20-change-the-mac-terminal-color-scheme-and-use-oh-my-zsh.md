---
title: change the mac terminal color scheme and use oh my zsh!
author: Ming Tang
date: '2015-05-20T14:53:00-07:00'
lastmod: '2015-05-20T14:53:00-07:00'
slug: change-the-mac-terminal-color-scheme-and-use-oh-my-zsh
tags:
- color scheme
- unix
- mac
- terminal
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/05/change-mac-terminal-color-scheme-and.html
description: I just got my lab mac pro. I wanted to change the color scheme of the default terminal, which is just black and white.
summary: I just got my lab mac pro. I wanted to change the color scheme of the default terminal, which is just black and white.
header:
  caption: ''
  image: ''
---

I just got my lab mac pro. I wanted to change the color scheme of the default terminal, which is just black and white.

I googled around, and found [solarized ](http://ethanschoonover.com/solarized).

![](/img/archive/change-the-mac-terminal-color-scheme-and-use-oh-my-zsh/01-screenshot-2015-05-20-16.31.20.png)

you can download it and unzip it.\
Then, open your terminal (you can search "terminal" at the upper right corner)

![](/img/archive/change-the-mac-terminal-color-scheme-and-use-oh-my-zsh/02-screenshot-2015-05-20-16.34.37.png)

![](/img/archive/change-the-mac-terminal-color-scheme-and-use-oh-my-zsh/03-screenshot-2015-05-20-16.36.59.png)

go to the preferences:

![](/img/archive/change-the-mac-terminal-color-scheme-and-use-oh-my-zsh/04-screenshot-2015-05-20-16.38.02.png)

![](/img/archive/change-the-mac-terminal-color-scheme-and-use-oh-my-zsh/05-screenshot-2015-05-20-16.40.43.png)

and import the color scheme for terminals in the folder **osx-terminal.app-colors-solarized**, which is in the unzipped the folder solarized. I preferred the Solarized Dark ansi.terminal , and set it as Default.

Now,  download [oh my zsh!](http://ohmyz.sh/) by:

$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh \| sh

You will have to install git first for the upper command to work.\
Now, when you fire up your terminal, it looks much more prettier! (there are many other schemes for oh my zsh, I found the default is good. You can change it by modifying the .zshrc file in your home directory.)

![](/img/archive/change-the-mac-terminal-color-scheme-and-use-oh-my-zsh/06-screenshot-2015-05-20-16.49.48.png)
