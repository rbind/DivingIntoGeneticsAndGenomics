---
title: recursion in python
author: Ming Tang
date: '2013-03-30T17:43:00-07:00'
lastmod: '2013-03-30T17:43:00-07:00'
slug: recursion-in-python
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/03/recursion-in-python.html
description: I just finished watching the MIT open course on python lecture 4\
summary: I just finished watching the MIT open course on python lecture 4\
header:
  caption: ''
  image: ''
---

I just finished watching the MIT open course on python lecture 4\
<http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-00-introduction-to-computer-science-and-programming-fall-2008/video-lectures/lecture-4/>

It talks about recursion at the end which I find is really interesting.

Two examples the professor gave are\
1. check a string is a palindrome or not.\
2.  Return the Fibonacci number of x.

python code:

def is_palindrome(s):\
    if len(s) \<= 1;\
         return True\
    else:\
          return s\[0\]=s\[-1\] and is_palindrome(s\[1:-1\])

def fib(x):\
    if x==0 or x==1 :\
          return 1\
    else:\
          return fib(x-1) + fib(x-2)

-------------------------------------------------------------------------
There are many algorithms to solve the first problem, but the pythonic way would be:\
def is_palindrom_v2(s):\
     return s==s\[::-1\]

\#simply reverse the string and check if it is the same as the original one.

update 06/13/13:\
it is very expensive for recursion in python in terms of computing time.\
watch Lesson 6 here:\
<https://www.udacity.com/course/viewer#!/c-cs101/l-48756019/m-48532681>
