---
title: ubuntu 12.04 networking restart
author: Ming Tang
date: '2013-07-20T15:39:00-07:00'
lastmod: '2013-07-20T15:39:00-07:00'
slug: ubuntu-12-04-networking-restart
tags:
- Ubuntu
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/07/ubuntu-1204-networking-restart.html
description: If I suspend my computer at school, I can not connect to the internet when I get home. I searched on line and found the command at terminal:\…
summary: If I suspend my computer at school, I can not connect to the internet when I get home. I searched on line and found the command at terminal:\…
header:
  caption: ''
  image: ''
---

If I suspend my computer at school, I can not connect to the internet when I get home. I searched on line and found the command at terminal:\
` /etc/init.d/networking start`

but it did not work, it says the command is deprecated....

Instead, type

`t``ommy@tommy-ThinkPad-T420:~$ ifconfig`\
`eth0 Link encap:Ethernet HWaddr 00:21:cc:71:ab:50`\
` UP BROADCAST MULTICAST MTU:1500 Metric:1`\
` RX packets:0 errors:0 dropped:0 overruns:0 frame:0`\
` TX packets:0 errors:0 dropped:0 overruns:0 carrier:0`\
` collisions:0 txqueuelen:1000`\
` RX bytes:0 (0.0 B) TX bytes:0 (0.0 B)`\
` Interrupt:20 Memory:f2500000-f2520000`

`lo Link encap:Local Loopback`\
` inet addr:127.0.0.1 Mask:255.0.0.0`\
` inet6 addr: ::1/128 Scope:Host`\
` UP LOOPBACK RUNNING MTU:16436 Metric:1`\
` RX packets:3526 errors:0 dropped:0 overruns:0 frame:0`\
` TX packets:3526 errors:0 dropped:0 overruns:0 carrier:0`\
` collisions:0 txqueuelen:0`\
` RX bytes:440923 (440.9 KB) TX bytes:440923 (440.9 KB)`

`wlan0 Link encap:Ethernet HWaddr 60:d8:19:c7:42:9b`\
` UP BROADCAST MULTICAST MTU:1500 Metric:1`\
` RX packets:238663 errors:0 dropped:0 overruns:0 frame:0`\
` TX packets:168877 errors:0 dropped:0 overruns:0 carrier:0`\
` collisions:0 txqueuelen:1000`\
` RX bytes:310906582 (310.9 MB) TX bytes:20035785 (20.0 MB)`

`tommy@tommy-ThinkPad-T420:~$ sudo ifconfig wlan0 down`\
`[sudo] password for tommy:`\
`tommy@tommy-ThinkPad-T420:~$ sudo ifconfig wlan0 up`

Now, I had no problem! Thanks to my roommate YuRen!
