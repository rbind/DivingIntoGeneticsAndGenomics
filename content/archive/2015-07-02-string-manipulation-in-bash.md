---
title: String manipulation in bash
author: Ming Tang
date: '2015-07-02T20:45:00-07:00'
lastmod: '2015-07-02T20:45:00-07:00'
slug: string-manipulation-in-bash
tags:
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2015/07/string-manipulation-in-bash.html
description: Update on 04/24/2016. see a post Shell parameter substitution
summary: Update on 04/24/2016. see a post Shell parameter substitution
header:
  caption: ''
  image: ''
---

Update on 04/24/2016. see a post [Shell parameter substitution](http://www.cyberciti.biz/tips/bash-shell-parameter-substitution-2.html)

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td>${parameter:-defaultValue}</td>
<td>Get default shell variables value</td>
</tr>
<tr>
<td>${parameter:=defaultValue}</td>
<td>Set default shell variables value</td>
</tr>
<tr>
<td>${parameter:?”Error Message”}</td>
<td>Display an error message if parameter is not set</td>
</tr>
<tr>
<td>${#var}</td>
<td>Find the length of the string</td>
</tr>
<tr>
<td>${var%pattern}</td>
<td>Remove from shortest rear (end) pattern</td>
</tr>
<tr>
<td>${var%%pattern}</td>
<td>Remove from longest rear (end) pattern</td>
</tr>
<tr>
<td>${var:num1:num2}</td>
<td>Substring</td>
</tr>
<tr>
<td>${var#pattern}</td>
<td>Remove from shortest front pattern</td>
</tr>
<tr>
<td>${var##pattern}</td>
<td>Remove from longest front pattern</td>
</tr>
<tr>
<td>${var/pattern/string}</td>
<td>Find and replace (only replace first occurrence)</td>
</tr>
<tr>
<td>${var//pattern/string}</td>
<td>Find and replace all occurrences<br />
</td>
</tr>
</tbody>
</table>

String manipulation is very useful in bash scripts; especially when processing a lot of files (in a for loop or xargs) 

with different names, and you want to name your output of each file with part of the input file name.\
`command file part_file.result`\
I am going to demonstrate string manipulation below:\
Let's create a variable `file` and print it out:

``` text
 In [1]:
```

``` text
file=foo.txt
echo "$file"
```

``` text
foo.txt
```

I want to change the suffix from `txt` to `pdf`. one of the commonly known ways is to use the\
 `basename` built-in function:

``` text
 In [2]:
```

``` text
echo "$(basename $file .txt).pdf"
```

``` text
foo.pdf
```

However, there are other built-in ways to get the same task done\
[references here](http://www.thegeekstuff.com/2010/07/bash-string-manipulation/)\
**find and replace**\
Replace only first match:\
`${string/pattern/replacement}`\
Replace all the matches:\
`${string//pattern/replacement}`\
`Following syntax replaces with the replacement string,`\
only when the pattern matches beginning of the $string.\
${string/#pattern/replacement}

`Following syntax replaces with the replacement string,`

``` text
only when the pattern matches at the end of the given $string.

${string/%pattern/replacement}
```

For more complex replacement, use `sed`. see my previous blog post [here](/archive/rename-a-bunch-of-files-with-bash-by-regular-expression/)

``` text
 In [3]:
```

``` text
echo "${file/txt/pdf}"
```

``` text
foo.pdf
```

``` text
 In [4]:
```

``` text
# a more complex exmaple
file_1=foo.txt.foo.txt
echo "${file_1//foo/bar}"
```

``` text
bar.txt.bar.txt
```

``` text
 In [5]:
```

``` text
echo "${file_1/foo/bar}"
```

``` text
bar.txt.foo.txt
```

``` text
 In [6]:
```

``` text
echo "${file_1/#foo/bar}"
```

``` text
bar.txt.foo.txt
```

``` text
 In [7]:
```

``` text
echo "${file_1/%txt/pdf}"
```

``` text
foo.txt.foo.pdf
```

`${string%substring} will delete the shortest match of substring`from back\
`${string%%substring}` will delete the longest match of substring from back

``` text
 In [8]:
```

``` text
echo "${file_1%txt*}pdf"
```

``` text
foo.txt.foo.pdf
```

``` text
 In [9]:
```

``` text
echo "${file_1%%txt*}pdf"
```

``` text
foo.pdf
```

`{string#substring}` will delete the shortest match of substring from the begining\
`{string##substring}` will delete the longest match of substring from the begining

``` text
 In [10]:
```

``` text
echo "bar${file_1#foo*}"
```

``` text
bar.txt.foo.txt
```

``` text
 In [11]:
```

``` text
echo "bar${file_1##foo*}.pdf"
```

``` text
bar.pdf
```

**string slicing**

``` text
${string:position}
Extract substring from $string at $position
${string:position:length}
```

Extract `$length of characters substring from $string starting from $position`

``` text
 In [12]:
```

``` text
echo "${file_1:4}"
```

``` text
txt.foo.txt
```

``` text
 In [13]:
```

``` text
echo "${file_1:4:7}"
```

``` text
txt.foo
```

Finally, the length of the string:

``` text
 In [14]:
```

``` text
echo "${#file_1}"
```

``` text
15
```
