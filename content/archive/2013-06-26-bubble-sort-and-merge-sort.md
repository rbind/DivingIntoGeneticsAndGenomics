---
title: bubble sort and merge sort
author: Ming Tang
date: '2013-06-26T13:44:00-07:00'
lastmod: '2013-06-26T13:44:00-07:00'
slug: bubble-sort-and-merge-sort
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/bubble-sort-and-merge-sort.html
description: I promised to write about bubble sort and merge sort in my previous blog define a python function--- find the biggest number of a list
summary: I promised to write about bubble sort and merge sort in my previous blog define a python function--- find the biggest number of a list
header:
  caption: ''
  image: ''
---

I promised to write about bubble sort and merge sort in my previous blog [define a python function--- find the biggest number of a list](/archive/define-a-python-function-find-the-biggest-number-of-a-list/)

Finally, I got some time to write it. My undergraduate helped me to do some molecular cloning stuff today, I guess I will be too lazy to do bench work :)

The examples I am going to use are from the MIT open course lecture 9 and 10\
<http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-00-introduction-to-computer-science-and-programming-fall-2008/video-lectures/lecture-9/>

<http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-00-introduction-to-computer-science-and-programming-fall-2008/video-lectures/lecture-10/>

The question is that you have a list of numbers, and how do you sort it from small to big.

I will lay down the python code first.

A trick....I copied (M-w) the code from my emacs editor and tried to paste (ctrl-v)  in the blog, but it did not work. In fact, click the  middle mouse button did the trick :)

\# a demonstration of buble sort and merge sort, p is a list of integers\
\# for bubble sort  swap the values if p\[i+1\] \< p\[i\]

`def bubblesort(p):`\
` for j in range(len(p)):`\
` for i in range(len(p)-1):`\
` if p[i] > p[i+1]:`\
` temp = p[i]`\
` p[i] = p[i+1]`\
` p[i+1] = temp`\
` return p`

`# for bubble sort, we can do better, when there is no swap, we stop the for loop`

`def bubblesort2(p):`\
` swapped = True`\
` while swapped:`\
` swapped = False`\
` for i in range(len(p)-1):`\
` if p[i] > p[i+1]:`\
` temp = p[i]`\
` p[i] = p[i+1]`\
` p[i+1] = temp`\
` swapped = True`\
` return p`

---------------------------------------------------------------------------

\# divide and conqure method merge-sort

\# divide the list in half, continue until we have single list, merge sub-lists

d`ef mergesort(p):`\
` # print p`\
` if len(p) < 2 :`\
` return p[:]`\
` else:`\
` middle = len(p) / 2`\
` left = mergesort(p[:middle]) # this is the beauty of recursion, further break down the list to smaller lists`\
` right =mergesort(p[middle:])`\
` together = merge(left, right)`\
` #print "merged", together`\
` return together`

` # now we need to define the merge function, this function merges two sorted lists of numbers.`

`def merge(left, right):`\
` result = []`\
` i,j = 0,0`\
` while i < len(left) and j < len(right):`\
` if left[i] <= right[j]:`\
` result.append(left[i])`\
` i = i+1`\
` else:`\
` result.append(right[j])`\
` j = j+1`\
` while i< len(left):`\
` result.append(left[i])`\
` i = i + 1`\
` while j < len(right):`\
` result.append(right[j])`\
` j = j + 1`\
` return result`

--------------------------------------------------------------

The code above is pretty self-explanatory.\
for bubblesort, we have a nested loop, the inner loop put the biggest number to the end of the list every time when execute the outer loop.

bubblesort2 uses a variable "swapped" to track  if there is any swap occurs. if no swap occurs, the list is already pre-sorted, quit the loop.  return the result.

mergesort splits the list into smaller sub-list until only single number list. Then, we merge back the sorted list into a big list.

Let's have a look at the performance of these three functions by Ipython's magic function %time

`In [15]: time a=bubblesort(range(10000))`\
`CPU times: user 7.86 s, sys: 0.00 s, total: 7.86 s`\
`Wall time: 7.88 s`

`In [16]: time a=bubblesort2(range(10000))`\
`CPU times: user 0.00 s, sys: 0.00 s, total: 0.00 s`\
`Wall time: 0.00 s`

`In [17]: time a=mergesort(range(10000))`\
`CPU times: user 0.05 s, sys: 0.01 s, total: 0.06 s`\
`Wall time: 0.05 s`

in this particular test, I generated a list of range(10000): \[0,1,2,3,4,5......999\]\
you will see bubblesort took the longest time, because it is not aware that the list is already pre-sorted.\
bubblesort2 caught that and return the result instantly.\
performance of mergesort is also decent. If we had a not pre-sorted list, the mergesort would out-perform the bubblesort2.

Finally, I want to show off my Emacs python editor :)

![](/img/archive/bubble-sort-and-merge-sort/01-selection_011.png)
