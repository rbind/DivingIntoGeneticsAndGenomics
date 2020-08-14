---
title: setting up Travis CI for github repos
author: Ming Tang
date: '2020-07-15'
slug: setting-up-travis-ci-for-github-repos
categories:
  - bioinformatics
tags:
  - reproducible research
header:
  caption: ''
  image: ''
  preview: yes
---

I wanted to set up Travis CI long time ago. Finally, I have a chance to do so.
There are already many posts on how to do it. I write it down here for my own future reference.
Thanks [Tao Liu](https://twitter.com/fooliu) for the instructions. I have been learning a lot from him in collabrating with the [MAESTRO](https://github.com/liulab-dfci/MAESTRO) project.

* read the official documentation https://docs.travis-ci.com/user/tutorial/
* Jean Fan's blog post https://jef.works/blog/2019/02/17/automate-testing-of-your-R-package/
* R specific https://docs.travis-ci.com/user/languages/r/
* Blog post from Julia https://juliasilge.com/blog/beginners-guide-to-travis/

### Step 1

go to github marketplace: https://github.com/marketplace.
Search for `Travis CI`. It is free for open source project. choose the free plan. Click `install for free`.

### step 2

Inside the github repo, create a `.travis.yml` file. See an example for the MAESTRO:https://github.com/liulab-dfci/MAESTRO/blob/master/.travis.yml

I also have one for my [scclusteval](https://github.com/crazyhottommy/scclusteval/blob/master/.travis.yml) R package.

After setting it up, after each commit or pull request, a test is automatically run in travis and you will get notification to see if the build passed or not. This is really nice!

### step 3 Embedding Status Images to the github README

You can embed a buid status badge in the README.md of your github repo following:
https://docs.travis-ci.com/user/status-images/

