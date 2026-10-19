---
title: long integers and float numbers in python
author: Ming Tang
date: '2013-04-01T18:44:00-07:00'
lastmod: '2013-04-01T18:44:00-07:00'
slug: long-integers-and-float-numbers-in-python
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/long-integers-and-float-numbers-in.html
description: lecture 5 of MIT open course on python
summary: lecture 5 of MIT open course on python
header:
  caption: ''
  image: ''
---

lecture 5 of MIT open course on python <http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-00-introduction-to-computer-science-and-programming-fall-2008/video-lectures/lecture-5/>

in python shell, type:

\>\>\> 2\*\*999

5357543035931336604742125245300009052807024058527668037218751941851755255624680612465991894078479290637973364587765734125935726428461570217992288787349287401967283887412115492710537302531185570938977091076523237491790970633699383779582771973038531457285598238843271083830214915826312193418602834034688L

notice that at the end, there is a mysterious L, it just means that it is a long integer( python's internal long format)

\>\>\>2\*\*1000

5357543035931336604742125245300009052807024058527668037218751941851755255624680612465991894078479290637973364587765734125935726428461570217992288787349287401967283887412115492710537302531185570938977091076523237491790970633699383779582771973038531457285598238843271083830214915826312193418602834034688L

what happens if 2\*\*1000 is divided by 2\*\*999?\
\>\>\> a= 2\*\*999\
\>\>\> b= 2\*\*1000\
\>\>\> b/a\
2L

L stays with 2.

float number

\>\>\> s=0\
\>\>\> for i in range(10):\
            s+=0.1

\>\>\> s

0.9999999999999999

it is not 1!\
it has to do with the ieee 754 standard to store numbers in computer\
<http://en.wikipedia.org/wiki/IEEE_floating_point>

\>\>\> print s\
1.0 

print statement round it to 1.

another example:\
\>\>\> import math\
\>\>\> a=math.sqrt(2)\
\>\>\>a

1.4142135623730951

\>\>\> a\*a==2\
False

in fact\
\>\>\> a\*a

2.0000000000000004

so, never test  a float number == some other number

usually test :\
abs(a\*a-2.0) \< epsilon\
where epsilon is a given small number
