---
title: group the same ID to the same line by python, use awk to do SQL like groupby and sum
author: Ming Tang
date: '2013-10-18T12:01:00-07:00'
lastmod: '2013-10-18T12:01:00-07:00'
slug: group-the-same-id-to-the-same-line-by-python-use-awk-to-do-sql-like-groupby-and
tags:
- awk
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/10/group-same-id-to-same-line-by-python.html
description: 'I was on Seqanswer forum and found someone asked a question:\ \ "Hi guys,\ i hv got a GO file for my differentially expressed genes file, it goes like:'
summary: 'I was on Seqanswer forum and found someone asked a question:\ \ "Hi guys,\ i hv got a GO file for my differentially expressed genes file, it goes like:'
header:
  caption: ''
  image: ''
---

I was on Seqanswer forum and found someone asked a question:\
<http://seqanswers.com/forums/showthread.php?p=119263&posted=1#post119263>\
"Hi guys,\
i hv got a GO file for my differentially expressed genes file, it goes like:

FBgn00001 GO:0016301 \[Name:\*\*\*\*(annotation)\]\
FBgn00002 GO:0016301 \[Name:\*\*\*\*(annotation)\]\
FBgn00003 GO:0016301 \[Name:\*\*\*\*(annotation)\]\
FBgn00004 GO:0003700 \[Name:\*\*\*\*(annotation)\]\
FBgn00004 GO:0009651 \[Name:\*\*\*\*(annotation)\]\
FBgn00004 GO:0006355 \[Name:\*\*\*\*(annotation)\]\
FBgn00005 GO:0009556 \[Name:\*\*\*\*(annotation)\]\
FBgn00005 GO:0005515 \[Name:\*\*\*\*(annotation)\]\
FBgn00005 GO:0080019 \[Name:\*\*\*\*(annotation)\]\
FBgn00005 GO:0016563 \[Name:\*\*\*\*(annotation)\]\
FBgn00005 GO:0016627 \[Name:\*\*\*\*(annotation)\]\
FBgn00006 GO:0003700 \[Name:\*\*\*\*(annotation)\]\
FBgn00006 GO:0010018 \[Name:\*\*\*\*(annotation)\]

now i want to use WEGO ,so i need to convert it like:

FBgn00001 GO:0016301\
FBgn00002 GO:0016301\
FBgn00003 GO:0016301\
FBgn00004 GO:0003700 GO:0009651 GO:0006355\
FBgn00005 GO:0009556 GO:0005515 GO:0080019 GO:0016563 GO:0016627\
FBgn00006 GO:0003700 GO:0010018

I think this could be solved using a perl script. I am not able to do this since i am a beginner. Can someone help me out? A simple perl script is good enough for me^^ "

The question is on very basic text manipulation, and I quickly wrote a python script for that:

`import csv`\
`reader = csv.reader(open("GO.txt","r"), delimiter="\t")`\
`new={}`\
`for row in reader:`\
` if row[0] not in new.keys():`\
` new[row[0]] = [row[1]]`\
` else:`\
` new[row[0]].append(row[1])`

`with open("wego.txt","w") as f:`\
` for key, value in sorted(new.items()):`\
` f.write(key+"\t"+"\t".join(value)+"\n")`

`update 10/19/13, I used the gist in github to display the code`

{{< gist crazyhottommy 7057625 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/7057625)

basically, I store the ID as the key, and a list of GO terms as the value, and then write the dictionary\
to a txt file.\
I was thinking if there are any easier ways to do it, I mean by awk or sed.\
A quick google search\
<http://stackoverflow.com/questions/10286522/group-by-sum-from-shell/10289422#10289422>

the question is a little bit different from the one above:

"I have a large file containing data like this:

``` bash
a 23
b 8
a 22
b 1
```

I want to be able to get this:

``` bash
a 45
b 9
```

"

awk array is very handy for that purpose see a post here <http://www.thegeekstuff.com/2010/03/awk-arrays-explained-with-5-practical-examples/>:\
let's first creat the file:\
`tommy@tommy-ThinkPad-T420:~$ cat > foo.txt`\
`a 23`\
`b 8`\
`a 22`\
`b 1`\
`#``control+D to indicate cat this is the end of the file.`

`tommy@tommy-ThinkPad-T420:~$ cat foo.txt | awk '{new[$1]+=$2}END{for (key in new) print key, new[key]}'`\
`a 45`\
`b 9`

`#or you can redirect the result to a new file`

`tommy@tommy-ThinkPad-T420:~$ cat foo.txt | awk '{new[$1]+=$2}END{for (key in new) print key, new[key]}' > new.txt`

I do not know whether the array value can hold multiple strings like a list in python.

-------------------------------------------
updated on 10/21/13,  an awk one liner can solve this problem

`tommy@tommy-ThinkPad-T420:~$ cat foo.txt | awk '{ if (new[$1]) new[$1]=new[$1]"\t"$2; else new[$1]=$2;} END { for (i in new) print i, new[i]}' OFS="\t"`\
`a 23 22`\
`b 8 1`

Amazing awk!
