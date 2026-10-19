---
title: define a python function--- find the biggest number of a list
author: Ming Tang
date: '2013-06-12T12:05:00-07:00'
lastmod: '2013-06-12T12:05:00-07:00'
slug: define-a-python-function-find-the-biggest-number-of-a-list
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/define-python-function-find-biggest.html
description: I was watching the udacity video introduction to computer science. I love the instructor, he is very clear and knowledgeable. I highly recommend this course…
summary: I was watching the udacity video introduction to computer science. I love the instructor, he is very clear and knowledgeable. I highly recommend this course…
header:
  caption: ''
  image: ''
---

I was watching the udacity video <https://www.udacity.com/course/cs101>  introduction to computer science. I love the instructor, he is very clear and knowledgeable. I highly recommend this course as a beginner of python.

In the problem sets of this course, I found several simple examples of  functions that make me think about how to solve a problem differently, though it may seem very obvious to an experienced programmer.

define a function (procedure) that finds the bigger number of two given numbers:

in ipython shell:

`In [1]: def bigger(a,b):`\
` ...: if a > b:`\
` ...: return a`\
` ...: return b`\
` ...:`

`In [2]: bigger(4,5)`\
`Out[2]: 5`

`In [3]: bigger(7,9)`\
`Out[3]: 9`

`In [4]: bigger(7,7)`\
`Out[4]: 7`

it is very straightforward in this sense.

next problem:\
define a function that finds the biggest of three numbers.

in this case, you may want to compare the three numbers to each other, but it looks too complicated.\
by using the bigger() function defined above, the problem is easier to be solved.

`In [5]: def biggest(a,b,c):`\
` ...: return bigger(a,bigger(b,c))`\
` ...:`

`In [6]: biggest(5,6,8)`\
`Out[6]: 8`

`In [7]: biggest(3,6,8)`\
`Out[7]: 8`

`In [8]: biggest(5,5,7)`\
`Out[8]: 7`

`In [9]: biggest(7,7,7)`\
`Out[9]: 7`

in the book PYTHON programming: an introduction to computer science  by Jonh Zelle\
from page 220 to page 225, it explains the details of decision structure for this problem.

let's extend the problem a little bit.\
define a function that finds the biggest in a number list. pre-condition: all the numbers are positive.

`In [11]: def greatest(p): # p is a list of positive numbers:`\
` ....: big=0`\
` ....: for number in p:`\
` ....: if number > big:`\
` ....: big=number`\
` ....: return big`\
` ....:`

`In [12]: greatest([7,5,9,3,1])`\
`Out[12]: 9`

in this function, we use a variable big to initiate its value to 0, since the numbers in the list are all positive. we then loop over the list, and compare big to each number in the list. if the number is bigger than "big", we update the "big" to that number, and finally return big.

what if the list contains negative numbers? you can not initiate  big=0, but you can initiate big =p\[0\], the first number of the list, and then compare to other numbers.

`def greates_1(p): #p is a list of any numbers:`\
` ....: big=p[0]`\
` ....: for number in p:`\
` ....: if number > big:`\
` ....: big=number`\
` ....: return big`\
` ....:`

`In [14]: greatest([-1,-3,5,7,-8])`\
`Out[14]: 7`

Of course, there is a built-in function called max can do this job fairly easily.\
usually, we do not  try to re-invent the wheel, but it is a good practise for learning functions.

`In [40]: max([4,7,8,-4,10])`\
`Out[40]: 10`

what if you want to sort the number list, how do you do it?\
I will write another blog about selection sort, bubble sort and merge sort later!
