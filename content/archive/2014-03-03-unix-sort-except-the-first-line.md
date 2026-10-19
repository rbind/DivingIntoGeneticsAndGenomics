---
title: Unix sort except the first line
author: Ming Tang
date: '2014-03-03T11:49:00-08:00'
lastmod: '2014-03-03T11:49:00-08:00'
slug: unix-sort-except-the-first-line
tags:
- sort
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/03/unix-sort-except-first-line.html
description: 'Many times I have a file with a header needs to be sorted, but I do not want to sort the header.\ I saw it on Twitter:'
summary: 'Many times I have a file with a header needs to be sorted, but I do not want to sort the header.\ I saw it on Twitter:'
header:
  caption: ''
  image: ''
---

Many times I have a file with a header  needs  to be sorted, but I do not want to sort the header.\
I saw it on Twitter:

 Retweeted by [Ming Tang](https://twitter.com/tangming2005)

[![](https://pbs.twimg.com/profile_images/1369601363/cosplay_g02_bigger.jpg)**razor** ‏@razZ0r ](https://twitter.com/razZ0r) [Mar 1](https://twitter.com/razZ0r/status/439829653172547584 "10:28 AM - 1 Mar 2014")\
todays fav linux oneliner: command \| (read -r; printf "%s\n" "$REPLY"; sort) \> output sort everything except first line.

REPLY is the default variable for the read command.

one more general use of the "body" function\
<http://stackoverflow.com/questions/9281449/unix-skip-header-bash-function>\
<http://unix.stackexchange.com/questions/11856/sort-but-keep-header-line-in-the-at-the-top>

``` bash
# print the header (the first line of input)
# and then run the specified command on the body (the rest of the input)
# use it in a pipeline, e.g. ps | body grep somepattern
body() {
    IFS= read -r header
    printf '%s\n' "$header"
    "$@"
}
```

IFS is the bash Shell Bourne variable: A list of characters that separate fields used by the shell to split text strings.

``` text
$#    Stores the number of command-line arguments that
      were passed to the shell program.
$?    Stores the exit value of the last command that was
      executed.
$0    Stores the first word of the entered command (the
      name of the shell program).
$*    Stores all the arguments that were entered on the
      command line ($1 $2 ...).
"$@"  Stores all the arguments that were entered
      on the command line, individually quoted ("$1" "$2" ...).
```

Or an awk solution `awk 'NR == 1; NR > 1 {print $0 | "sort -n"}'`\
It is very handy to use.

Test:\
`tommy@tommy-ThinkPad-T420:~$ cat body_sort.txt`\
`header`\
`4`\
`6`\
`9`\
`7`\
`14`\
`8`

`tommy@tommy-ThinkPad-T420:~$ cat body_sort.txt |( read -r; printf "%s\n" "$REPLY"; sort -n)`\
`header`\
`4`\
`6`\
`7`\
`8`\
`9`\
`14`
