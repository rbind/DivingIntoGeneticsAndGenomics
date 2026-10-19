---
title: linux command join with header
author: Ming Tang
date: '2015-08-04T20:58:00-07:00'
lastmod: '2015-08-04T20:58:00-07:00'
slug: linux-command-join-with-header
tags:
- join
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/08/linux-command-join-with-header.html
description: They are small but can do small things well.\ Today, I am going to demonstrate a very useful linux command join.\ join is used to join two files based on a…
summary: They are small but can do small things well.\ Today, I am going to demonstrate a very useful linux command join.\ join is used to join two files based on a…
header:
  caption: ''
  image: ''
---

#### See the ipython notebook here 

#### <https://github.com/crazyhottommy/scripts-general-use/blob/master/Shell/Join_command.ipynb>

#### linux commands are very powerful.

They are small but can do small things well.\
Today, I am going to demonstrate a very useful linux command `join`.\
`join` is used to join two files based on a common "ID" column.\
This is a common task when you have information stored in two files\
and have a common column to link them together.\
I first created two dummy tab delimited files: A.txt and B.txt\
Let's have a look at the files:

``` text
 In [1]:
```

``` text
cat A.txt
```

``` text
Jack TX 5
John CA 3
Luke WA 4
Tammy FL 2
Tommy FL 1
```

``` text
 In [2]:
```

``` text
cat B.txt
```

``` text
1 450
2 300
3 400
5 150
6 600
```

column 3 in file A and column 1 in file B are common IDs that link these two files together.\
Let's do an **inner join**:

``` text
 In [3]:
```

``` text
join -1 3 -2 1 A.txt B.txt
```

``` text
5 Jack TX 150
```

-1 means file 1, which is A file. 3 means the third column.\
-2 means file 2, which is B file. 1 means the first column.\
The output is not we expected. A file and B file should have 4 rows linked together.\
**It turns out files need to be sorted first according to the common\
field for join to work.** Let's sort them first and join them:

``` text
 In [4]:
```

``` text
sort -k3,3 A.txt > A.sorted.txt
sort -k1,1 B.txt > B.sorted.txt
join -1 3 -2 1 A.sorted.txt B.sorted.txt
```

``` text
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
5 Jack TX 150
```

we can also do **right outer join** (-a 1) :

``` text
 In [5]:
```

``` text
join -1 3 -2 1 -a 1 A.sorted.txt B.sorted.txt
```

``` text
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
Luke WA 4
5 Jack TX 150
```

The default input seprator is space or tab, and output seprator is space.\
**we can change the output seprator to tab:**

``` text
 In [6]:
```

``` text
join -t$'\t' -1 3 -2 1 -a 1 -o auto A.sorted.txt B.sorted.txt
```

``` text
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
4 Luke WA
5 Jack TX 150
```

Let's do a **left outer join** (-a 2) and fill the empty field with NA.\
**Note that only GNU join has the auto flag.**

``` text
 In [7]:
```

``` text
join -t$'\t' -e "NA" -1 3 -2 1 -a 2 -o auto A.sorted.txt B.sorted.txt
```

``` text
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
5 Jack TX 150
6 NA NA 600
```

You may think the intermediate sorted file is annoying. One can avoid them using\
[process substitution](http://vincebuffalo.org/2013/08/08/the-mighty-named-pipe.html):

``` text
 In [8]:
```

``` text
join -1 3 -2 1 -t $'\t' <(sort -k3,3 A.txt) <(sort -k1,1 B.txt)
```

``` text
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
5 Jack TX 150
```

Wow! It is so magic! Using process substitution can speed up your workflow.

So far, the files we have are without headers. **What if they contain headers?**\
Let's make some dummy files first:

``` text
 In [9]:
```

``` text
printf "name\tstate\tid\n" | cat - A.txt > A_header.txt
printf "id\tsalary\n" | cat - B.txt > B_header.txt
```

``` text
 In [10]:
```

``` text
cat A_header.txt
```

``` text
name state id
Jack TX 5
John CA 3
Luke WA 4
Tammy FL 2
Tommy FL 1
```

``` text
 In [11]:
```

``` text
cat B_header.txt
```

``` text
id salary
1 450
2 300
3 400
5 150
6 600
```

Let's join them together:

``` text
 In [12]:
```

``` text
join -1 3 -2 1 -t $'\t' <(sort -k3,3 A_header.txt) <(sort -k1,1 B_header.txt)
```

``` text
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
5 Jack TX 150
id name state salary
```

Unfortunately, after sorting, the header went to the bottom of the file. Many times, the header may just go to the middle of the file. We can always delete the header first,\
join the file and then add the header back, but we will need to have intermediate files.\
I will use [`body` function](https://github.com/jeroenjanssens/data-science-at-the-command-line/blob/master/tools/body) which will ignore the header.

``` text
 In [13]:
```

``` text
join -1 3 -2 1 -t $'\t' <(cat A_header.txt | body sort -k3,3) <(cat B_header.txt | body sort -k1,1)
```

``` text
id name state salary
1 Tommy FL 450
2 Tammy FL 300
3 John CA 400
5 Jack TX 150
```
