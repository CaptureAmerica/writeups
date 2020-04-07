---
title: "Trendmicro CTF 2019 Quals Writeup"
date: 2020-01-25T17:00:00+09:00
lastmod: 2020-01-25T17:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/trendmicroctf_2019/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Ftrendmicroctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

(2020/01/25 - 復習しました)

URL: [https://www.trendmicro.com/ja_jp/campaigns/capture-the-flag.html](https://www.trendmicro.com/ja_jp/campaigns/capture-the-flag.html)
<br /><br />
2019年9月のアタマ辺りにやってたCTFです。ファイルだけダウンロードして、その後スルーしたやつなんですが、後日（2020/01/25）１個だけですが復習しました。
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後に行った復習です。他の方のWriteupとか参照してます。



<br /><br />
## [Forensics]: Forensics-exploit 100
- - -
### Challenge
> Analyze the pcap, check for clues, listen, follow the breadcrumbs and ultimately collect the flag!

Attachment:

- ctf.pcap (元ファイル名はこれじゃなかったかも)


<br />
### Solution
Wireshark > Statistics > Conversations

<img src="https://captureamerica.github.io/writeups/img/trandmicro2019_for1.png" alt="trandmicro2019_for1.png">

Port Bのところに、印字可能文字コードが並んでいるので、いくつか変換してみます。

<pre>
$ python
Python 3.7.3 (default, May 26 2019, 16:51:20) 
[Clang 10.0.1 (clang-1001.0.46.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> chr(84)
'T'
>>> chr(77)
'M'
>>> chr(67)
'C'
>>> exit()
</pre>

フラグになりそう。

ということで、

<pre>
$ tshark -r ctf.pcap -Tfields -e ip.addr -e tcp.dstport | grep "117.114.23.95" | awk '{print $2}' | tr "\n" " " ; echo
84 77 67 84 70 123 75 110 48 99 107 74 117 53 116 111 78 116 104 51 114 49 103 104 116 100 48 48 114 125 42740 125 125 42740 42740 125

$ python -c 'print("".join([chr(int(x)) for x in "84 77 67 84 70 123 75 110 48 99 107 74 117 53 116 111 78 116 104 51 114 49 103 104 116 100 48 48 114 125".split()]))'
TMCTF{Kn0ckJu5toNth3r1ghtd00r}
</pre>

Flag: `TMCTF{Kn0ckJu5toNth3r1ghtd00r}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

