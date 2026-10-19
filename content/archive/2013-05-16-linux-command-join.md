---
title: linux command join
author: Ming Tang
date: '2013-05-16T11:15:00-07:00'
lastmod: '2013-05-16T11:15:00-07:00'
slug: linux-command-join
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/05/linux-command-join.html
description: Linux command join serves for this purpose.\ note that two files need to be sorted first.
summary: Linux command join serves for this purpose.\ note that two files need to be sorted first.
header:
  caption: ''
  image: ''
---

## I have a list of gene names in one column at one file, and I have another file contains the gene names, expression levels etc in columns in another file, I wanted to extract the lines in my second file by the gene names in the first file.

Linux command join  serves for this purpose.\
note that two files need to be sorted first.

## see here :<http://www.thegeekstuff.com/2012/10/15-linux-split-and-join-command-examples-to-manage-large-files/>

Linux Join Command Examples

### 8. Basic Join Example

Join command works on first field of the two files (supplied as input) by matching the first fields.\
Here is an example :

``` text
$ cat testfile1
1 India
2 US
3 Ireland
4 UK
5 Canada

$ cat testfile2
1 NewDelhi
2 Washington
3 Dublin
4 London
5 Toronto

$ join testfile1 testfile2
1 India NewDelhi
2 US Washington
3 Ireland Dublin
4 UK London
5 Canada Toronto
```

So we see that a file containing countries was joined with another file containing capitals on the basis of first field.

### 9. Join works on Sorted List

If any of the two files supplied to join command is not sorted then it shows up a warning in output and that particular entry is not joined.\
In this example, since the input file is not sorted, it will display a warning/error message.

``` text
$ cat testfile1
1 India
2 US
3 Ireland
5 Canada
4 UK

$ cat testfile2
1 NewDelhi
2 Washington
3 Dublin
4 London
5 Toronto

$ join testfile1 testfile2
1 India NewDelhi
2 US Washington
3 Ireland Dublin
join: testfile1:5: is not sorted: 4 UK
5 Canada Toronto
```

### 10. Ignore Case using -i option

When comparing fields, the difference in case can be ignored using -i option as shown below.

``` text
$ cat testfile1
a India
b US
c Ireland
d UK
e Canada

$ cat testfile2
a NewDelhi
B Washington
c Dublin
d London
e Toronto

$ join testfile1 testfile2
a India NewDelhi
c Ireland Dublin
d UK London
e Canada Toronto

$ join -i testfile1 testfile2
a India NewDelhi
b US Washington
c Ireland Dublin
d UK London
e Canada Toronto
```

### 11. Verify that Input is Sorted using –check-order option

Here is an example. Since testfile1 was unsorted towards the end so an error was produced in the output.

``` text
$ cat testfile1
a India
b US
c Ireland
d UK
f Australia
e Canada

$ cat testfile2
a NewDelhi
b Washington
c Dublin
d London
e Toronto

$ join --check-order testfile1 testfile2
a India NewDelhi
b US Washington
c Ireland Dublin
d UK London
join: testfile1:6: is not sorted: e Canada
```

### 12. Do not Check the Sortness using –nocheck-order option

This is the opposite of the previous example. No check for sortness is done in this example, and it will not display any error message.

``` text
$ join --nocheck-order testfile1 testfile2
a India NewDelhi
b US Washington
c Ireland Dublin
d UK London
```

### 13. Print Unpairable Lines using -a option

If both the input files cannot be mapped one to one then through -a\[FILENUM\] option we can have those lines that cannot be paired while comparing. FILENUM is the file number (1 or 2).\
In the following example, we see that using -a1 produced the last line in testfile1 (marked as bold below) which had no pair in testfile2.

``` text
$ cat testfile1
a India
b US
c Ireland
d UK
e Canada
f Australia

$ cat testfile2
a NewDelhi
b Washington
c Dublin
d London
e Toronto

$ join testfile1 testfile2
a India NewDelhi
b US Washington
c Ireland Dublin
d UK London
e Canada Toronto

$ join -a1 testfile1 testfile2
a India NewDelhi
b US Washington
c Ireland Dublin
d UK London
e Canada Toronto
f Australia
```

### 14. Print Only Unpaired Lines using -v option

In the above example both paired and unpaired lines were produced in the output. But, if only unpaired output is desired then use -v option as shown below.

``` text
$ join -v1 testfile1 testfile2
f Australia
```

### 15. Join Based on Different Columns from Both Files using -1 and -2 option

By default the first columns in both the files is used for comparing before joining. You can change this behavior using -1 and -2 option.\
In the following example, the first column of testfile1 was compared with the second column of testfile2 to produce the join command output.

``` text
$ cat testfile1
a India
b US
c Ireland
d UK
e Canada

$ cat testfile2
NewDelhi a
Washington b
Dublin c
London d
Toronto e

$ join -1 1 -2 2 testfile1 testfile2
a India NewDelhi
b US Washington
c Ireland Dublin
d UK London
e Canada Toronto
```
