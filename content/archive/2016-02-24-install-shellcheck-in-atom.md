---
title: install shellcheck in atom
author: Ming Tang
date: '2016-02-24T16:00:00-08:00'
lastmod: '2016-02-24T16:00:00-08:00'
slug: install-shellcheck-in-atom
tags:
- atom
- editor
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2016/02/install-shellcheck-in-atom.html
description: First, download and install Atom.
summary: First, download and install Atom.
header:
  caption: ''
  image: ''
---

First, download and install [Atom](https://atom.io/).

then  install [shellcheck](https://github.com/koalaman/shellcheck)  and linter:

``` text
brew install shellcheck
```

``` text
apm install linter
```

``` text
apm install linter-shellcheck
```

You may need to edit the config file to specify the shellcheck path:

![](/img/archive/install-shellcheck-in-atom/01-screenshot-2016-02-24-17.56.03.png)

Then, re-open Atom, and write some shell scripts:

![](/img/archive/install-shellcheck-in-atom/02-screenshot-2016-02-24-17.49.40.png)

See the warnings down in the bottom. Pretty cool!
