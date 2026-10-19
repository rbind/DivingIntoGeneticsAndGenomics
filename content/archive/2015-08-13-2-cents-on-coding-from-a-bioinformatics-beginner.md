---
title: 2 cents on coding from a bioinformatics beginner
author: Ming Tang
date: '2015-08-13T09:31:00-07:00'
lastmod: '2015-08-13T09:31:00-07:00'
slug: 2-cents-on-coding-from-a-bioinformatics-beginner
tags:
- computation skills for biologist
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/08/2-cents-on-coding-from-bioinformatics.html
description: 'I was trained as a wet biologist and I started learning coding in 2012 April with my first ever python book: python programming for absolute beginners. I…'
summary: 'I was trained as a wet biologist and I started learning coding in 2012 April with my first ever python book: python programming for absolute beginners. I…'
header:
  caption: ''
  image: ''
---

I was trained as a wet biologist and I started learning coding in 2012 April with my first ever python book: [python programming for absolute beginners.](http://www.amazon.com/Python-Programming-Absolute-Beginner-Edition/dp/1435455002)  I still remember the days that after work I would sit down in front of the computer and go through the book until 10pm everyday.  

It was not that practical in terms of translating what I have learned to what I want to analyze in the lab, but still I have entered into a new world!

In the Fall semester of 2012, I took a beginner bioinformatics course at University of Florida using [practical computing for biologists](http://practicalcomputing.org/) as a reference book.  It is a great book and it taught me regular expression, Unix commands and some python stuffs that directly related to biology. I was deeply attracted by the beauty of codes and was surprised/satisfied that how useful learning coding can be.

Lessons I learned from that class:\
**Regular expression is extremely useful**! At least one needs to know the basics and you can then always google and find solutions there. 

Bioinformatics is a field that evolves so fast that many tools you use may become obsolete tomorrow. However, **unix skills will never fade.** I urge every wet biologist like me to learn Unix commands first. It will take time for you to be fluent in the terminal. It took me 2 years to feel really confortable working in the terminal, so stop worrying and take your time. 

Statistical programming language R is very popular in the bioinformatics field. I started using R because I can take advantage of the rich packages in bioconductor.  I started from the basics with [The art of R programming](https://www.nostarch.com/artofr.htm). After getting the basics, learn to use packages like dplyr, ggplot2 will greatly reduce the complexity of your code and enhancer your productivity. Surprisingly, all these awesome packages were developed by the same person: [Hadley Wikham](http://priceonomics.com/hadley-wickham-the-man-who-revolutionized-r/).

Learn some [git.](https://git-scm.com/)  Git is a version control system that tracks your code. I am still a beginner, but I realized how important it is to version control my codes.  For this reason, I have a [github repo](https://github.com/crazyhottommy?tab=repositories) where I put my codes.  I am still learning git everyday.

When the project grows big, you need to well manage it. There are several resources that I recommend you to read before any project:

1.    [A Quick Guide to Organizing Computational Biology Projects](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424)

2\.    [Designing project by Vince Buffalo ](http://nicercode.github.io/blog/2013-04-05-projects/)Vince Buffalo has a book which I highly recommend for everyone: [Bioinformatics data skills.](http://shop.oreilly.com/product/0636920030157.do) It covers many points that I want to say in this post. I might write a review on it after finishing all the chapters.

3\.   [Best Practices for Scientific Computing](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745)

The take home message for me is that it is not enough for you to just run the code, get some results and then publish them.

One needs to be aware that:\
1.   **Computers make mistakes**. They can give you non-sense results and exit without error, so make extensive tests before running your code. 

2.   **Share your codes**. Even your codes are correct, you need to share them so that other people can look at them and may improve them.

3.   **Make your codes reusable**. Do not hard code your scripts. If it takes a file path as input, make it as an argument in your scripts.

4.   **Modulate your scripts. **Data could come in different stage of formats.  Take ChIP-sequencing data analysis as an example, if you have a script that starts processing the data from fastq to the final peaks.  You may want to modulate your scripts to two modules: one for mapping fastq to bam, and the other for bam to peaks. **Modulate your scripts** so that one can use your script when the data come in a bam format.

5.  **Heavily comment your scripts**. It will not only make other people to understand your codes better, but also help the future you to understand what you did.

6.   **You need to make your analysis reproducible**. Each step of your analysis should be documented in a markdown file. I say every step, yes, every command that you strike in the terminal getting the intermediate files need to be taken down. Moreover, how, when and where did you download the data need to be documented. This will save the future you! Many experienced programmers overlook this point.

I am glad that I have come to this stage. I love what I am doing now and feel satisfied when I learn new things everyday. I want to encourage all the wet biologists: believe you can program as well :)
