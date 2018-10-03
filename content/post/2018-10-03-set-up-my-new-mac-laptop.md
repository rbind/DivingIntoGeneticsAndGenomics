---
title: set up my new mac laptop
author: Ming Tang
date: '2018-10-03'
slug: set-up-my-new-mac-laptop
categories:
  - config
tags:
  - mac setup
header:
  caption: ''
  image: ''
---

I am starting my first day at Harvard FAS informatics and I will keep a note here on how I set up my new laptop.



### customize terminal 

following [this gist](https://gist.github.com/kevin-smets/8568070):

download iterm2 for mac [here](https://iterm2.com/downloads.html).

### install on-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

You will have to install git first for the upper command to work.
Now, when you fire up your terminal, it looks much more prettier! (there are many other schemes for oh my zsh, I found the default is good. You can change it by modifying the .zshrc file in your home directory.)

**Selecting a Theme**

Once you find a theme that you'd like to use, you will need to edit the ~/.zshrc file. You'll see an environment variable (all caps) in there that looks like:

`ZSH_THEME="robbyrussell"`
To use a different theme, simply change the value to match the name of your desired theme. For example:

`ZSH_THEME="agnoster"` # (this is one of the fancy ones)
see https://github.com/robbyrussell/oh-my-zsh/wiki/Themes#agnoster


### install conda

install `conda`, go to https://www.anaconda.com/download/#macos and download the graphic installer. It is ~600 MB and take some time.

Usually `conda` is installed in the home directory. Somehow, it was installed in the `/` root.

add the following to the `~/.zshrc` file

put conda after the `$PATH`, otherwise somehow `oh-my-zsh` will not work properly (the folders are not displayed with color etc)
```
export PATH=$PATH:"/anaconda3/bin"
```

### file editor 

`sublime` or `Atom`

download `sublime` from https://www.sublimetext.com/3 and install it.

if you want to open sublime in terminal

```
open /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl

# make a symbolic link for it
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime

# now you can
sublime my.txt
```
### install R and Rstudio

The current R version is R3.5.1. Download it from [here](https://cran.cnr.berkeley.edu).

Install Rstudio v1.2.1013-1.
I installed the [preview version](https://www.rstudio.com/products/rstudio/download/preview/) of Rstudio because it can evalute python code directly throught [`reticulate`](https://github.com/rstudio/reticulate). I've used `pycharm` before, but it is too clunky for me. One can of course set up both R and python inside `sublime` or `Atom`, but I do not want to leave `Rstudio` :)

### install docker

follow https://docs.docker.com/docker-for-mac/install/
### install slack

[Slack](https://slack.com) For team communication
### install Notion

[Notion](https://www.notion.so) For to-do list