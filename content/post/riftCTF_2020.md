---
title: "RiftCTF 2020 Writeup | フラxxグゲット"
date: 2020-03-21T15:30:00+09:00
lastmod: 2020-03-21T15:30:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
URL: [http://riftctf.iiitnr.ac.in:8000/challenges](http://riftctf.iiitnr.ac.in:8000/challenges)
<br /><br />
stringsコマンドで解けるやつ（3つもあった）とか、ウェブで調べものをするやつとか、答えはわかっててもフラグがIncorrectになるやつとか、そんなのが多かったです。

<br />

とりあえず、記録だけ残しておきます。

<img src="https://captureamerica.github.io/writeups/img/riftctf_2020_Score1.png" alt="riftctf_2020_Score1.png">

<img src="https://captureamerica.github.io/writeups/img/riftctf_2020_Score2.png" alt="riftctf_2020_Score2.png">

<img src="https://captureamerica.github.io/writeups/img/riftctf_2020_Score3.png" alt="riftctf_2020_Score3.png">

<br /><br />

ちなみに、以下は、解けなかったやつです。


<br />
## [Forensics]: 0x0004 (100 points)
- - -
### Challenge
> this is one of those challenges where you can't solve it until and unless you recognize the stuff.
<br />
flag format ritsCTF{<---flag-here--->}.
<br />
Good Luck.

Attachment:

- razorgirl.mp3


<br />
### (Unsolved)
まず、問題文に出てくるフラグフォーマット、ritsCTF{}が間違っています。正しくは、riftCTF{}のはず。

（ていうか、Discordでも指摘されてたし、こういうのはすぐ直さなきゃ。）

<br>
RX-SSTVを使って簡単に画像は取れるんですけど、正確になんて書いてあるのかが分からなくて、いろんなパターンを試したけど結局フラグが通りませんでした。。


<img src="https://captureamerica.github.io/writeups/img/riftctf_2020_razorgirl.png" alt="riftctf_2020_razorgirl.png">


<br /><br />
## [Forensics]: 0x0003 (300 points)
- - -
### Challenge
> 1. identify the file format.
2. read about the file format.
3. see which properties this particular file has.
4. and fix the file to get the flag.
5. brute-forcing won't help but you can do whatever you want.
6. flag format ritsCTF{<---flag-here--->}. Good Luck!
<br />

Attachment:

- flag.zip
- readme.txt


<br />
### (Unsolved)
何気にこのチャレンジも、間違ったフラグフォーマット ritsCTF{} になってますね。。

<br />
flag.zip には、flag.txtとreadme.txtが含まれています。

<pre>
captureamerica@kali:~/CTF/RiftCTF_2020$ zipinfo flag.zip
Archive:  flag.zip
Zip file size: 492 bytes, number of entries: 2
-rw-a--     3.1 fat       54 BX defN 20-Mar-16 15:52 readme.txt
-rw-a--     3.1 fat       40 BX defN 20-Mar-16 14:29 flag.txt
2 files, 94 bytes uncompressed, 96 bytes compressed:  -2.1%
</pre>


<br>
Zipの末尾にはゴミもついてます。

<pre>
captureamerica@kali:~/CTF/RiftCTF_2020$ xxd flag.zip 
00000000: 504b 0304 1400 0900 0800 837e 7050 1883  PK.........~pP..
00000010: e814 4200 0000 3600 0000 0a00 0000 7265  ..B...6.......re
00000020: 6164 6d65 2e74 7874 943c 6031 fbe9 5e4a  adme.txt.<`1..^J
00000030: a6c0 2318 c24f 5e3f cbb5 6cdc 922b b57c  ..#..O^?..l..+.|
00000040: 5ee4 c158 5993 cae9 d125 e7a6 b172 91f3  ^..XY....%...r..
00000050: 2943 7153 3857 a343 f728 1c2d 32a6 6878  )CqS8W.C.(.-2.hx
00000060: 1b09 4e8b 16aa ed0a b234 504b 0708 1883  ..N......4PK....
00000070: e814 4200 0000 3600 0000 504b 0304 1400  ..B...6...PK....
00000080: 0900 0800 a073 7050 5f42 5254 3600 0000  .....spP_BRT6...
00000090: 2800 0000 0800 0000 666c 6167 2e74 7874  (.......flag.txt
000000a0: 14d3 f8b4 6160 6e4e 7969 0fee 41b2 fd09  ....a`nNyi..A...
000000b0: 19e7 eb3e 06d5 baf4 db08 c1d2 ee0b 7ec4  ...>..........~.
000000c0: 037c acec ddc3 e20d 7887 a7e3 7651 c8ca  .|......x...vQ..
000000d0: 8de6 fa2d a0ec 504b 0708 5f42 5254 3600  ...-..PK.._BRT6.
000000e0: 0000 2800 0000 504b 0102 1f00 1400 0900  ..(...PK........
000000f0: 0800 837e 7050 1883 e814 4200 0000 3600  ...~pP....B...6.
00000100: 0000 0a00 2400 0000 0000 0000 2000 0000  ....$....... ...
00000110: 0000 0000 7265 6164 6d65 2e74 7874 0a00  ....readme.txt..
00000120: 2000 0000 0000 0100 1800 0fbf b7be 7cfb   .............|.
00000130: d501 0fbf b7be 7cfb d501 7801 02ac 7cfb  ......|...x...|.
00000140: d501 504b 0102 1f00 1400 0900 0800 a073  ..PK...........s
00000150: 7050 5f42 5254 3600 0000 2800 0000 0800  pP_BRT6...(.....
00000160: 2400 0000 0000 0000 2000 0000 7a00 0000  $....... ...z...
00000170: 666c 6167 2e74 7874 0a00 2000 0000 0000  flag.txt.. .....
00000180: 0100 1800 bda8 f122 71fb d501 bda8 f122  ......."q......"
00000190: 71fb d501 626e 39ec 70fb d501 504b 0506  q...bn9.p...PK..
000001a0: 0000 0000 0200 0200 b600 0000 e600 0000  ................
000001b0: 0000 4c4f 4c20 594f 5520 4341 4e54 2052  ..LOL YOU CANT R
000001c0: 4541 4420 5448 4520 464c 4147 204c 4f4c  EAD THE FLAG LOL
000001d0: 4c4f 4c4f 4c4f 4c4f 4c4f 4c4f 4c4f 4c4f  LOLOLOLOLOLOLOLO
000001e0: 4c4f 4c4f 4c4f 4c4f 4c4f 4c4f            LOLOLOLOLOLO
</pre>

<br>
これは、既知平文攻撃で pkcrack でやっつける問題だと思うんですが、残念ながら解けませんでした。。。



<br /><br />
- - -
<br /><br />
<br /><br />

