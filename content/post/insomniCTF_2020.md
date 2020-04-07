---
title: "Insomnihack Writeup"
date: 2020-01-19T18:00:00+09:00
lastmod: 2020-01-19T18:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/insomnictf_2020/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Finsomnictf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

URL: [https://teaser.insomnihack.ch/challenges/](https://teaser.insomnihack.ch/challenges/)
<br /><br />
10問中1問解きました。ほとんどの人が解いた簡単なやつだけです。


ブログ書くか悩みましたが、とりあえず、参加した記録として残しておきます。
<br /><br />



<br /><br />
## [Web]: LowDeep
- - -
### Challenge
> Try out our new ping platform: lowdeep.insomnihack.ch/


<br />
### Solution
見るからにCommand Injectionなんです。

<img src="https://captureamerica.github.io/writeups/img/insomniCTF_LowDeep.png" alt="insomniCTF_LowDeep.png">

<br />
以下、"ls -l"の結果です。
<pre>
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.019 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.019/0.019/0.019/0.000 ms
total 28
drwxr-xr-x 3 root root 4096 Jan 17 09:57 .
drwxr-xr-x 3 root root 4096 Jan 7 16:52 ..
drwxr-xr-x 4 root root 4096 Jan 17 09:57 _res_
-rw-r--r-- 1 root root 1367 Jan 16 15:30 index.php
-rw-r--r-- 1 root root 6128 Jan 16 13:10 print-flag
-rw-r--r-- 1 root root 42 Jan 16 15:35 robots.txt
</pre>

<br />
print-flag には実行権が付いていないので、サーバ上では実行できません。なので、ダウンロードしてきます。

<pre>
$ file print-flag
print-flag: ELF 64-bit LSB shared object x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=72c589834f878a6a3267944f305c29166a1ace8b, stripped
</pre>

<br />
ChmodしてLinux上で実行してもフラグは取れますが、それ以前にstringsで取れます。

<pre>
$ strings print-flag | grep -i ins
INS{Wh1le_ld_k1nd_0f_forg0t_ab0ut_th3_x_fl4g}
</pre>


<br />
Flag: `INS{Wh1le_ld_k1nd_0f_forg0t_ab0ut_th3_x_fl4g}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

