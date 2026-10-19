---
title: My personal suggestions to get started with bioinformatics
author: Ming Tang
date: '2013-06-08T19:45:00-07:00'
lastmod: '2013-06-08T19:45:00-07:00'
slug: my-personal-suggestions-to-get-started-with-bioinformatics
tags:
- computation skills for biologist
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/my-personal-suggestions-to-get-started.html
description: updated 07/20/13\ see a post by Nick Loman
summary: updated 07/20/13\ see a post by Nick Loman
header:
  caption: ''
  image: ''
---

updated 07/20/13\
see a post by Nick Loman <http://pathogenomics.bham.ac.uk/blog/2013/07/i-want-to-learn-bioinformatics-a-guide-for-complete-beginners/>

I am following him on twitter, and I am glad I knew most points mentioned in the article above.

another blog by Titus Brown the author of h-kmer\
<http://ivory.idyll.org/blog/2013-sesync-meeting.html>

---------------------------------------------------------------------------
I started doing bioinformatics ( bioinformatics is really a deep river, it may not sound I am doing any real bioinformatics to many bioinformaticians)  last year in April. The past one year has been tremendously exciting as I learn new stuff every day and become more confident with huge data analysis.

Several suggestions I have for newbies  in this area:

1. operation system\
yes, switch to Unix system if you are still using windows.

I installed bio-linux <http://nebc.nerc.ac.uk/tools/bio-linux/bio-linux-7-info>\
it is based on ubuntu 12.04 LTS

once you have it installed, you need to get familiar with the linux command lines.\
start from here <http://www.linuxcommand.org/index.php>

useful commands for text manipulations are:

less, cat\
head, tail\
cut, paste, join,\
wc, sort, uniq, grep

then sed and awk are two commonly used commands for more complex text manipulation.

2. you have to understand regular expression <http://en.wikipedia.org/wiki/Regular_expression>\
use text editor like Jedit in linux. Another real powerful text editor would be Vim or emacs.\
I have used emacs for a while, you have to remember many hot keys.

I have configured it to run python <http://www.jesshamrick.com/2012/09/18/emacs-as-a-python-ide/> and ESS (R) <http://ess.r-project.org/>

3. programming languages\
I started with python last year, and it is fairly easy to understand.

python courses:

## [Learn to Program: The Fundamentals ](https://class.coursera.org/programming1-2012-001/class/index)

## [https://class.coursera.org/programmin](https://class.coursera.org/programming1-2012-001/class/index)[g1-2012-001/class/index](https://class.coursera.org/programming1-2012-001/class/index)

## Introduction to Computer Science and Programming

<http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-00sc-introduction-to-computer-science-and-programming-spring-2011/index.htm>

## Introduction to Computer Science

<https://www.udacity.com/>

once you get familiar with all the computer concepts and terms, you can learn C++.

I just started reading  C++ Primer Plus <http://www.amazon.com/Primer-Plus-Edition-Developers-Library/dp/0321776402>

R, bioconductor. <http://www.bioconductor.org/>\
There are so many packages developed for specific uses like microarray analysis, NGS data analysis.\
I've used bioconductor a little bit for microarray analysis, detect differential expressed genes, generating heatmaps etc. I am impressed by its ability to generate complex graphs.

4. stay the current of bioinformatics

### See the post here <http://gettinggeneticsdone.blogspot.com/2012/05/how-to-stay-current-in.html>

it is very important that you know what tools are out there, and you just learn how to use them.

5\. know how to run programs in a remote server though shh command.\
UF hpc center <http://wiki.hpc.ufl.edu/doc/Getting_Started>\
Cloud computing is becoming very popular. <http://www.biostars.org/p/132/>\
<http://bcbio.wordpress.com/2011/11/29/making-next-generation-sequencing-analysis-pipelines-easier-with-biocloudcentral-and-galaxy-integration/>

6\. Database manage knowledge.\
learn mysql or htsql <http://htsql.org/>

7\. learn git, the version control  system.\
<http://git-scm.com/>

I just started reading the Pro Git book received on Friday. It is fun! 

8\. Last, the most important thing is that  you do a project.  PhDs learn things by doing.\
During a project, you will always encounter problems, search them in google, tackle it and you gain the precious experience.

I will keep posting new stuff I learn.
