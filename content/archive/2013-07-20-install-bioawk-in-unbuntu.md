---
title: install bioawk in unbuntu
author: Ming Tang
date: '2013-07-20T20:21:00-07:00'
lastmod: '2013-07-20T20:21:00-07:00'
slug: install-bioawk-in-unbuntu
tags:
- unix
- NGS
- Ubuntu
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/07/install-bioawk-in-unbuntu.html
description: 'Bioawk is written by Heng Li, and it can handle formats like:'
summary: 'Bioawk is written by Heng Li, and it can handle formats like:'
header:
  caption: ''
  image: ''
---

Bioawk is written by Heng Li, and it can handle formats like:

``` text
bed:
     1:chrom 2:start 3:end 4:name 5:score 6:strand 7:thickstart 8:thickend 9:rgb 10:blockcount 11:blocksizes 12:blockstarts
sam:
    1:qname 2:flag 3:rname 4:pos 5:mapq 6:cigar 7:rnext 8:pnext 9:tlen 10:seq 11:qual
vcf:
    1:chrom 2:pos 3:id 4:ref 5:alt 6:qual 7:filter 8:info
gff:
    1:seqname 2:source 3:feature 4:start 5:end 6:score 7:filter 8:strand 9:group 10:attribute
fastx: 1:name 2:seq 3:qual 4:comment
```

 I followed the tutorial here <https://github.com/vsbuffalo/bioawk-tutorial>

my first try did not work

`tommy@tommy-ThinkPad-T420:~$ git clone git://github.com/lh3/bioawk.git && cd bioawk && make && mv awk bioawk && sudo cp bioawk /usr/local/bin/`\
`Cloning into 'bioawk'...`\
`remote: Counting objects: 163, done.`\
`remote: Compressing objects: 100% (89/89), done.`\
`remote: Total 163 (delta 95), reused 136 (delta 74)`\
`Receiving objects: 100% (163/163), 112.32 KiB, done.`\
`Resolving deltas: 100% (95/95), done.`\
`yacc -d awkgram.y`\
`make: yacc: Command not found`\
`make: *** [ytab.o] Error 127`

It looks like I do not have  `yacc or bison (the GNU`\
`equivalent)` installed.

`tommy@tommy-ThinkPad-T420:~/bioawk$ sudo synaptic`

search yacc, and install the bison.

after that, it worked.

`tommy@tommy-ThinkPad-T420:~/bioawk$ bioawk`\
`usage: bioawk [-F fs] [-v var=value] [-c fmt] [-H] [-f progfile | 'prog'] [file ...]`

a quick tutorial for git:\
<https://github.com/vsbuffalo/git-demo>
