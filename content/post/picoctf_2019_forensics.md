---
title: "picoCTF 2019 Writeup (Forensics)"
date: 2019-10-13T12:00:00+09:00
lastmod: 2019-10-13T12:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://2019game.picoctf.com/](https://2019game.picoctf.com/)
<br /><br />
お疲れ様でした！ （実際、ほんとに疲れた）

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Score.png" alt="pico2019_Score.png">

<br />
イベント終了時点で、ユーザは4万人弱でした。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Users.png" alt="pico2019_Users.png">

<img src="https://captureamerica.github.io/writeups/img/pico2019_Rank.png" alt="pico2019_Rank.png">

PwnとWeb系があんまりできてないけど、スコアも切りのいい20000に行ったし、Globalで目標の300位以内 (283位) にも入れたのでかなり満足です。

2週間の間、土日もずっとピコってたし、一日年休も取って頑張りました。

去年出た問題に似てるものも結構あったので、picoCTF 2018をそれなりにやった人は結構アドバンテージがあったと思います。



<br /><br />
Forensics の Writeupです。




<br /><br />
# [Forensics]: WhitePages (250 points)
- - -
## Challenge
> I stopped using YellowPages and moved onto WhitePages... but the page they gave me is all blank!


<br />
## Solution
てっきりwhitespaceというesolangだと思ったので、少し手こずりました。

あと、たぶんだけど、チャレンジが始まった頃にダウンロードしたファイルと、後日ダウンロードし直したファイルが異なってた気がします。。。

スペースとタブ（？）の2種類の文字があるので、それを0と1に変えてから、2進数を文字に変えるのが解き方です。

```
$ xxd whitepages.txt | cut -c 10- | cut -c -40 | sed -e 's/e2//g' | sed -e 's/80//g' | sed -e 's/20/1/g' | sed -e 's/83/0/g' | tr -d " \n" ; echo
00001010000010010000100101110000011010010110001101101111010000110101010001000110000010100000101000001001000010010101001101000101010001010010000001010000010101010100001001001100010010010100001100100000010100100100010101000011010011110101001001000100010100110010000000100110001000000100001001000001010000110100101101000111010100100100111101010101010011100100010000100000010100100100010101010000010011110101001001010100000010100000100100001001001101010011000000110000001100000010000001000110011011110111001001100010011001010111001100100000010000010111011001100101001011000010000001010000011010010111010001110100011100110110001001110101011100100110011101101000001011000010000001010000010000010010000000110001001101010011001000110001001100110000101000001001000010010111000001101001011000110110111101000011010101000100011001111011011011100110111101110100010111110110000101101100011011000101111101110011011100000110000101100011011001010111001101011111011000010111001001100101010111110110001101110010011001010110000101110100011001010110010001011111011001010111000101110101011000010110110001011111011000110011000100110110001101110011000000110100001100000110001100110111001100110011100001100101001110000110001001100011011000010110010100110010001100010011000000111001011001010110011000110100011000100110010100110101001110010011011000110000011000100011000101111101000010100000100100001001
```

Flag: `picoCTF{not_all_spaces_are_created_equal_c167040c738e8bcae2109ef4be5960b1}`




<br /><br />
<br /><br />
# [Forensics]: c0rrupt (250 points)
- - -
## Challenge
> We found this file. Recover the flag.
<br /><br />
Hints : Try fixing the file header


Attachment:

- mystery


<br />
## Solution
先頭の16バイトくらい直せば通るかと思いきや、結構壊れててハマりました。

1番目のIDATチャンクのとこも修正が必要。

CRCは、pngcsumで直しました。<br />
[http://schaik.com/png/pngcsum.html](http://schaik.com/png/pngcsum.html)

```
root@kali:~/picoCTF_2019# pngcsum mystery mystery_crcfixed.png
IHDR ( 13 ) - csum = 7c8bab78
sRGB (  1 ) - csum = aece1ce9
gAMA (  4 ) - csum = 0bfc6105
pHYs (  9 ) - csum = 495224f0 -> 38d82c82
IDAT (65445 ) - csum = 6927db59
IDAT (65524 ) - csum = ba6bc1fa
IDAT (65524 ) - csum = 5997d200
IDAT (6304 ) - csum = 6ff175b8
IEND (  0 ) - csum = ae426082
```


Flag: `picoCTF{c0rrupt10n_1847995}`




<br /><br />
<br /><br />
# [Forensics]: shark on wire 2 (300 points)
- - -
## Challenge
> We found this packet capture. Recover the flag that was pilfered from the network. You can also find the file in /problems/shark-on-wire-2_0_5b5597f90483360b4480373bed30738e.

Attachment:

- capture.pcap


<br />
## Solution
udpのフローを順番に見ていくと、フラグっぽい文字列が出てきます。

"kfdsalkfsalkico{N0t_a_fLag}"<br />
"icoCTF{StaT31355e"<br />

あと、<br />
"start"<br />
"end"<br />
というのも出てくるんですが、その間のPayloadにはフラグっぽい文字列はなかったので、最初無視してました。

udp.port==8888 あたりを集中して見てたんですが、どうしてもフラグにはならなくて、その後、tcp あたりをがっつり見てそこもさっぱりフラグにならなかったので、挫けそうでした。

"start" "end"が出てるのは (udp.port==22) で、眺めていたらSource Portが全部バラけてて、しかも文字になりそうな値になっているのに気づきました。

一旦、フィルターしたものを別ファイル（capture_filtered.pcap）として保存し、Scapyで読み込みました。

```Python
#!/usr/bin/env python
from scapy.all import *

pkt=rdpcap("./capture_filtered.pcap")
for i in range(len(pkt)):
    sys.stdout.write(chr(pkt[i].sport-5000))
```

Flag: `picoCTF{p1LLf3r3d_data_v1a_st3g0}`





<br /><br />
<br /><br />
# [Forensics]: WebNet0 (350 points)
- - -
## Challenge
> We found this packet capture and key. Recover the flag.
<br /><br />
Hints :<br />
Try using a tool like Wireshark<br />
How can you decrypt the TLS stream?


Attachment:

- capture.pcap
- picopico.key


<br />
## Solution
Go to Preference -> TLS -> RSA key list <br />
172.31.22.220, 443, http, C:\Temp\picopico.key


Flag: `picoCTF{nongshim.shrimp.crackers}`



<br /><br />
<br /><br />
# [Forensics]: WebNet1 (450 points)
- - -
## Challenge
> We found this packet capture and key. Recover the flag.
<br /><br />
Hints :<br />
Try using a tool like Wireshark<br />
How can you decrypt the TLS stream?


Attachment:

- capture.pcap
- picopico.key


<br />
## Solution
復号したトラフィックから、HTTP objectを抽出し、stringsコマンドでフラグサーチ。


Flag: `picoCTF{honey.roasted.peanuts}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

