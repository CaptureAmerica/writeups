---
title: "Jornadas CTF 2021 Writeup"
date: 2021-10-21T21:00:00+09:00
lastmod: 2021-10-21T21:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<img src="https://captureamerica.github.io/writeups/img/green_bar.png" alt="green_bar.png">
<img src="https://captureamerica.github.io/writeups/img/10_Nature_Themed_Icons_Cute_Earth_Icon.png" alt="Save The Earth!"> <b><font color="teal">Save The Earth! - 地球環境を守ろう！</font></b> <img src="https://captureamerica.github.io/writeups/img/10_Nature_Themed_Icons_Cute_Earth_Icon.png" alt="Save The Earth!">
<img src="https://captureamerica.github.io/writeups/img/green_bar.png" alt="green_bar.png">

{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fjornadas_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://jornadas2021.ctf.cert.rcts.pt/challenges](https://jornadas2021.ctf.cert.rcts.pt/challenges)
<br /><br />

2043点を獲得し、86位でした。

平日にやっていたCTFイベントで、あまりやってる時間がなかったです。チャレンジのリストには、もうアクセスできなくなっています。。

<br>

<img src="https://captureamerica.github.io/writeups/img/jornadas_CTF_2021_Score1.png" alt="jornadas_CTF_2021_Score1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/jornadas_CTF_2021_Score2.png" alt="jornadas_CTF_2021_Score2.png">


<br><br>
点数が高めのやつだけ、writeup残しておきます。

<br />
## [Network]: Find IP(v6) (380 points)
- - -
### Challenge
> Find the IPv6 address of the DNS root server which is published on the lowest autonomous system number.
<br /><br />
Flag format: flag{IPv6 address}
<br /><br />
Note: you only have 5 attempts!

<br />
### Solution
これは、頑張ってググるだけです。

以下のサイトを見つけました。<br>
[https://root-servers.org/](https://root-servers.org/)

Hのタブで書かれているRoot Serverが一番 ASN (autonomous system number) が小さいので、これのIPv6アドレスが答えとなります。

<br />

Flag: `flag{d1c037808d23acd0dc0e3b897f344571ddce4b294e742b434888b3d9f69d9944}`


<br /><br />
<br /><br />
## [Forensics]: Chirp (335 points)
- - -
### Challenge
> We got sent a file that sounds like a bird. There must be something hidden... Can you help us out?

Attachment:

- Chirp.wav

<br />
### Solution
WaveファイルをAudacityで開いて、Spectrogram表示するとモールス信号になっています。

<pre>
... .... --- .-. - ..- .-. .-.. .-.-.- .- - -..-. -.- -.-. --. -- -..-
</pre>

変換すると、[SHORTURL.AT/KCGMX](SHORTURL.AT/KCGMX) となり、実際にアクセスすると、以下の2つのファイルがダウンロードできます。

- bird.jpg
- corrupted.mp3

<br>

bird.jpgの方は、stegseekで、以下のテキストが得られます。

<pre>
$ cat secret.txt
Oh wait...
Why can't I play that song... Doesn't seem like everything is right with it...
And is that a 'k' on the file? Is it suppose to be there?
</pre>

<br>

corrupted.mp3 の方は、拡張子は .mp3 ですが、実際はテキストファイルです。

<pre>
$ file corrupted.mp3
corrupted.mp3: ASCII text

$ cat corrupted.mp3
0000000 fbff 64e0 0000 1308 c96d 6d15 02e0 908f
0000010 8047 28a0 3200 2f02 1949 80cd 1002 e145
0000020 00c3 00a0 5601 7bf3 3261 43d0 9054 ad3b
0000030 dc43 3cd6 230a 5e80 0234 28e3 3322 f372
0000040 2129 b131 1153 300d 5090 1910 cb69 5e96
:
(snip)
:
</pre>

<br>

bird.jpgの方で得られたヒントを元に 'k' をサーチしてみると、以下が見つかります。

<pre>
0000af0 98d6 3e81 f591 5ebf 0227 f866 68b0 b294
0000ak0 666c 6167 7b34 5564 6930 5f66 3072 336e  <==
0000ak0 2e73 3163 735f 3173 5f70 3431 6e7d 5555  <==
0000b00 4721 718e 67ad a22b a4ca 58b7 15c3 37b9
</pre>

<br>

Flag: `flag{4Udi0_f0r3n.s1cs_1s_p41n}`



<br /><br />
<br /><br />
## [Forensics]: Paintr (728 points)
- - -
### Challenge
> An ex-employee left this on our computers. Can you analyse it to see if its safe?

Attachment:

- abstract.png
- nautilus.jpg


<br />
### Solution
確か、abstract.png は使わなかったです。

nautilus.jpg の方から、stegseekで以下が取れます。

<pre>
$ cat nautilus.jpg.out
1111111010001100101111111
1000001001111010101000001
1011101011101101001011101
1011101011110101101011101
1011101010001001101011101
1000001000101100001000001
1111111010101010101111111
0000000000110011100000000
1111001010100111010011101
1111000010111111111101000
0000111111110100001011000
0011000001100111111001100
0010111101111101101111110
0110010100001001101111011
0110101101001010010011010
1010110111010011010000000
0011011110111100111111100
0000000011010001100010001
1111111001100000101010111
1000001001001011100011000
1011101000100000111111011
1011101010110010101111011
1011101010011011101010010
1000001010101101100000100
1111111010000110001000111
</pre>

<br>
直感で、QRコードなのがわかりました。

しばらくどうやって解こうか悩んだんですが、テキストになっているQRコードを読み取るツール、以前なにかのCTFで使ったことがあったんですよね。

[https://github.com/waidotto/strong-qr-decoder](https://github.com/waidotto/strong-qr-decoder)

1と0の表記にも対応しているので、このツールがそのまんま使えます。

<pre>
$ nkf -w -Lu --overwrite nautilus.jpg.out
$ python sqrd.py nautilus.jpg.out
flag{p13t_70_qr_wh0_7h0ug7h!}
</pre>


<br>

Flag: `flag{p13t_70_qr_wh0_7h0ug7h!}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
