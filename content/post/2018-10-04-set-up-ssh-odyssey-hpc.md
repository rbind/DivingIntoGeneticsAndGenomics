---
title: set up ssh odyssey HPC
author: Ming Tang
date: '2018-10-04'
slug: set-up-ssh-odyssey-hpc
categories:
  - HPC
tags:
  - ssh
header:
  caption: ''
  image: ''
---

1. request an account following [here](https://www.rc.fas.harvard.edu/resources/faq/how-do-i-get-a-research-computing-account/).

2. Harvard odyssey HPC requires a two-factor security login. First set up the VPN following [here](https://www.rc.fas.harvard.edu/resources/vpn-setup/)

3. Then read about OpenAuth [here](https://www.rc.fas.harvard.edu/resources/documentation/openauth/). One can download mobile apps such as `Duo` and `google-Authenticator` on your phone, but then each time you will need to type the password to the terminal. Or you can download the java program for desktop, if you have a FAS research computing account, you should be able to download it from the link FASRC send you. I downloaded a folder like `mtang-openauth`. The prefix will be your HPC account name.

```bash 
cd mtang-openauth
./mtang-openauth.sh
No Java runtime present, requesting install.
```
You will have to install `java` first. My mac prompt a window for downloading, click `More Info`, it will bring you to a webpage for downloading.

After you install `java`, type `./mtang-openauth.sh`, a little green box with the 6 digits verification code will show up in the upper-right corner of your screen.

Fire-up iterm2:

```bash
ssh mtang@odyssey.rc.fas.harvard.edu

# it will prompt for password and then the verification code.
# type in the password for your account
# copy the 6-digits verification code and paste to your iterm window
```
This is kind of annoying if I have to do this everytime. It takes me ~2 seconds to type or copy/paste from the java desktop app, but what if I automate this. suppose I log into the HPC 10 times a day and 300 days per year. I save `2x10x300 = 6000` seconds, that's  100 minutes a year :) The following may take me ~1 hour to set up, but I hope it can save you some time by following this.

#### generate ssh key for password-less login

Note, I have done this a couple of times, but I still need to google it everytime I set it up. I have told you googling is an essential skill for bioinformaticians :)

On your mac:

```bash
ssh-keygen -b 2048

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/mingtang/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/mingtang/.ssh/id_rsa.
Your public key has been saved in /Users/mingtang/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:8LOJnp1rv5BZY9yoHVcrQarTnhW5Ih+eK8dA1+rxYao mingtang@huitadmins-MacBook-Pro.local
The key's randomart image is:
+---[RSA 2048]----+
|            .    |
|           o .   |
|      .   ..+ .  |
|       o.+.o.= . |
|       .S.@.* .  |
|       ..^oXo.   |
|      . B+B= .   |
|     . ooo=..    |
|      o.E*+.     |
+----[SHA256]-----+

```

tell `ssh-agent` about our key, we use `ssh-add`:

```bash
ssh-add
```

This creates a private key at `~/.ssh/id_rsa` and a public key at `~/.ssh/id_rsa.pub`.

Now, `ssh` to **odyssey HPC**:

```bash
ssh mtang@odyssey.rc.fas.harvard.edu
# type in the passowrd
# copy paste the 6-digit verification code in the green box
# you will be logged in your home directory
 
cd .ssh
```
append your public key file (id_rsa.pub, not your private key!) on your mac (open the file, copy it) to `~/.ssh/authorized_keys` on the HPC. 

I asked Aaron in the group and it turns out one can not skip the 2-factor system for password-less login on the `odyssey.rc.fas.harvard.edu` login node.


#### OTP Token Paster for OS X

Follow [here](https://github.com/jwm/os-x-otp-token-paster)

* Install Homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install oath-toolkit
```

* If it isn't already present, enter `secret=YOUR_BASE32_ENCODED_SECRET` in `~/.JAuth.rc`.

```bash
# I have Services folder in my mac
mkdir -p ~/Library/Services
git clone https://github.com/jwm/os-x-otp-token-paster
cd os-x-otp-token-paster
mv 'Enter Current TOTP Token.workflow' ~/Library/Services
```

* Open System Preferences -> Keyboard -> Keyboard Shortcuts -> Services -> Enter Current OTP Token (probably way down at the bottom).
* Click 'add shortcut' and enter the key combination you want to use.I use `control + shift + U`

```bash
ssh mtang@odyssey.rc.fas.harvard.edu
# type in password
# when prompt for verificartion code
# use the short-cut: control+shift+U 
```

#### ControlMaster for ssh 

Please follow [here](https://www.rc.fas.harvard.edu/resources/documentation/linux/using-ssh-controlmaster-for-single-sign-on/) to set it up.

with this, you only need to type in the password and verification code once. The subsequent logins will be password-less for the `odyssey.rc.fas.harvard.edu` login node.


>OpenSSH has an option called ControlMaster that enables the sharing of multiple sessions over a single network connection. This means that you can connect to Odyssey once, enter your password and Verification code, and have all other subsequent ssh sessions (including svn, rsync, etc. that run over ssh) piggy-back off the initial connection without need for re-authentication. You can specify such options each time on the command line, but it's easiest if you put it in your ssh client configuration file so that it applies every time.

### Mosh, a mobile shell

If your internet is not stable you may want to install [mosh](https://mosh.org/)

>Remote terminal application that allows roaming, supports intermittent connectivity, and provides intelligent local echo and line editing of user keystrokes.

After installing it, instead of doing 

```bash
ssh mtang@odyssey.rc.fas.harvard.edu

# do this
mosh mtang@odyssey.rc.fas.harvard.edu
```
Even your internet is dropped or the VPN is disconnected, after you reconnect, you shell 
session will be alive without logging in. Pretty cool!
