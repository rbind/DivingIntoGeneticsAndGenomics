---
title: set up odyssey HPC dot files
author: Ming Tang
date: '2018-10-09'
slug: set-up-odyssey-hpc-dot-files
categories:
  - HPC
tags:
  - dotfile
header:
  caption: ''
  image: ''
---

### dot files

Those files are originally got from [Samir Amin](https://sbamin.com/about), my labmate in [Roel Verhaak's lab](https://www.jax.org/research-and-faculty/faculty/roel-verhaak). Thanks for sharing!

`.screenrc`  
`.bashrc`  
`.bash_profile`  

and inside `.profile.d` folder, there is a file named `01_odyssey_config.sh`. It was executed when you login the shell.

You can grab my dot files in my github [repo](https://github.com/crazyhottommy/odyssey_dot_files).

Inside the `.bash_profile`:

```bash
if [ -d $HOME/.profile.d ]; then
  for i in $HOME/.profile.d/*.sh; do
    if [ -r $i ]; then
          if [ "${-#*i}" != "$-" ]; then
            . "$i" >/dev/null 2>&1
        else
            . "$i" >/dev/null 2>&1
        fi
    fi
  done
  unset i
fi
```

add or remove path in the `01_odyssey_config.sh` file. load modules in this file as well.

### install conda

install conda to my home directory `/n/home02/mtang`

After finishing installing, it will ask

```
Do you wish the installer to initialize Anaconda3
in your /n/home02/mtang/.bashrc ? [yes|no]
```

I chosed `no`. It showed me

```
You may wish to edit your /n/home02/mtang/.bashrc to setup Anaconda3:

source /n/home02/mtang/anaconda3/etc/profile.d/conda.sh

Thank you for installing Anaconda3!
```

I guess I got why Samir set up the dot files (has a .profile.d folder) like this. `conda` is doing similar with `source /n/home02/mtang/anaconda3/etc/profile.d/conda.sh`

**add conda to PATH**:

In the `~/.profiled.d/01_odyssey_config.sh`, add one line:

`mypathmunge ${HOME}/anaconda3/bin`

`mypathmunge` is a function inside the `.bash_profile`

```bash

mypathmunge () {
    case ":${PATH}:" in
        *:"$1":*)
            ;;
        *)
            if [ "$2" = "after" ] ; then
                PATH=$PATH:$1
            else
                PATH=$1:$PATH
            fi
    esac
}
```

it will add the `${HOME}/anaconda3/bin` in front of the $PATH. If you want to append it to $PATH, do `mypathmunge ${HOME}/anaconda3/bin after`

```bash
source .bash_profile
which conda
~/anaconda3/bin/conda
```

A side note. see [here](https://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc) for the difference between `.bashrc` and `.bash_profile`.

>.bash_profile is executed for login shells, while .bashrc is executed for interactive non-login shells.

>When you login (type username and password) via console, either sitting at the machine, or remotely via ssh: .bash_profile is executed to configure your shell before the initial command prompt.

>But, if you’ve already logged into your machine and open a new terminal window (xterm) then .bashrc is executed before the window command prompt. .bashrc is also run when you start a new bash instance by typing /bin/bash in a terminal.

>On OS X, Terminal by default runs a login shell every time, so this is a little different to most other systems, but you can configure that in the preferences.

[This](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html) is a very helpful blog post to understand the differences.
### Dedicating a folder for installing tools

usually, install tools with `conda`. If there is no recipe in conda. I will downlad the source to `~/apps` and compile there.

Then in the `~/.profiled.d/01_odyssey_config.sh` file, add the executable to the PATH.

e.g. I want to install `ncdu` to check disk usage

```bash
mkdir apps
cd apps
mkdir ncdu; cd ncdu
wget https://dev.yorhel.nl/download/ncdu-1.13.tar.gz
tar xvzf  ncdu-1.13.tar.gz
cd ncdu-1.13

./configure --prefix=${HOME}/apps/ncdu/ncdu-1.13
make 
make install
```

In the `~/.profiled.d/01_odyssey_config.sh` file, add two lines:

`export ODYAPPS="${HOME}/apps`  
`mypathmunge $ODYPPS/ncdu/ncdu-1.13/bin after`

```bash
source ~/.bash_profile
# now ready to go
ncdu
```
