---
title: custom sort in python, the built-in sorted()
author: Ming Tang
date: '2013-04-08T18:09:00-07:00'
lastmod: '2013-04-08T18:09:00-07:00'
slug: custom-sort-in-python-the-built-in-sorted
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/custom-sort-in-python-built-in-sorted.html
description: let's say you have a list, and you want to sort it. you can use the build in function sorted()
summary: let's say you have a list, and you want to sort it. you can use the build in function sorted()
header:
  caption: ''
  image: ''
---

let's say you have a list, and you want to sort it. you can use the build in function sorted()

Help on built-in function sorted in module \_\_builtin\_\_:

sorted(...)\
    sorted(iterable, cmp=None, key=None, reverse=False) --\> new sorted list\
(END)

it takes any iterable as input.\
for example:

\>\>\> a=\[4,2,1,6\]\
\>\>\> sorted(a)\
\[1, 2, 4, 6\]

\>\>\> sorted(a, reverse=True)\
\[6, 4, 2, 1\]

\>\>\> a=\['ccc','aaaa','d','bb'\]\
\>\>\> sorted(a)\
\['aaaa', 'bb', 'ccc', 'd'\]

if you want to sort according to the length of the string, pass a len function as the argument

\>\>\> sorted(a, key=len)\
\['d', 'bb', 'ccc', 'aaaa'\]

\>\>\> a=\['ccc','aaaz','a','bb'\]

you want to sort according to the last letter of the string

\>\>\> def last(s):\
...     return s\[-1\]\
...\
\>\>\> sorted(a, key=last)\
\['a', 'bb', 'ccc', 'aaaz'\]

let's say you have a list of tuples, the sorted function sorts according to the first element\
of the tuple, if they are the same, compare the second element of the tuple.

\>\>\> a=\[(1,'b'),(2,'a'),(1,'a')\]\
\>\>\> sorted(a)\
\[(1, 'a'), (1, 'b'), (2, 'a')\]

so, if you want to sort by something first and then sort by another thing next, you can write\
a key function to return a tuple.
