---
title: "RITSEC CTF 2023 Writeup"
date: 2023-04-03T16:00:00+09:00
lastmod: 2023-04-03T16:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fritsec_ctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.ritsec.club/challenges](https://ctf.ritsec.club/challenges)
<br /><br />

847ポイントで、最終順位は269番でした。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_score1.png" alt="ritsec_ctf_2023_score1.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_score2.png" alt="ritsec_ctf_2023_score2.png">

<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_chall_list1.png" alt="ritsec_ctf_2023_chall_list1.png"> <br />
<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_chall_list2.png" alt="ritsec_ctf_2023_chall_list2.png"> <br />
<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_chall_list3.png" alt="ritsec_ctf_2023_chall_list3.png">
<br /><br />

あまり、根気強くできなかったです。。。(すぐ諦めちゃったのが多いです)


<br /><br />
以下、Writeupです。

## [Forensics]: Web of Lies (98 points)
- - -
### Challenge
> We found more weird traffic. We're concerned he's connected to a web of underground criminals.

Attachments:

- weboflies.pcapng

<br />

### Solution

以下のように、`GET /flag` と `GET /fl4g` のHTTPリクエストがランダムに出てきます。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_forensics.png" alt="ritsec_ctf_2023_forensics.png">

それ以外は、特に怪しいところはありません。

2つのパターンがある場合は、フラグが2進数の文字列になっているケースが多いです。

<pre>
$ tshark -r weboflies.pcapng -Y http.request | cut -d/ -f2 | cut -d' ' -f1
flag
fl4g
flag
fl4g
:
(snip)
:
fl4g
fl4g
fl4g
flag
fl4g
</pre>

<br />

テキストエディタなどで、"flag" を "0" に、"fl4g" を "1" に変換し、あとはそれを文字列に変換するだけです。(今回は、CyberChefを使いました。)

<br />

Flag: `RS{Refr3shTh3P4g3}`

<br />

実際、このようにデータを抜かれたら (data exfiltration)、ファイアウォールなどで防ぐのは難しそうですよね。

このpcapの例に限っては、宛先が localhost (127.0.0.1) なので、データは漏洩していませんが。




<br /><br />
<br /><br />
## [Web]: X-Men Lore (238 points)
- - -
### Challenge
> The 90's X-Men Animated Series is better than the movies. Change my mind.
<br /><br />
[https://xmen-lore-web.challenges.ctf.ritsec.club/](https://xmen-lore-web.challenges.ctf.ritsec.club/)
<br /><br />
Hint1: The flag is in it's own file, it shouldn't be too hard to find. Once you're in, try to read the flags
<br />
HInt2: The flag file does not have a file extension


<br />

### Solution

X-menの各メンバーの情報が見れるサイトが用意されています。

Jubilee をクリックしたときにセットされる Cookie は、以下のようでした。

<pre>
Cookie: xmen=PD94bWwgdmVyc2lvbj0nMS4wJyBlbmNvZGluZz0nVVRGLTgnPz48aW5wdXQ+PHhtZW4+SnViaWxlZTwveG1lbj48L2lucHV0Pg==
</pre>

<br />

Base64 Decodeをすると、以下になります。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_web1.png" alt="ritsec_ctf_2023_web1.png">


<br />

XXE攻撃ができそうです。（だから、チャレンジ名がX-Menなのかな。）

<br />

以下の文字列を用意し、Base64 Encodeして、Cookieにセットします。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2023_web2.png" alt="ritsec_ctf_2023_web2.png">


<br />

その状態で、https://xmen-lore-web.challenges.ctf.ritsec.club/xmen にアクセスしたら、フラグが得られました。


<br />

Flag: `RS{XM3N_L0R3?_M0R3_L1K3_XM3N_3XT3RN4L_3NT1TY!}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
