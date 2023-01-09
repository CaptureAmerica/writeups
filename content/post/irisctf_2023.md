---
title: "IrisCTF 2023 Writeup"
date: 2023-01-09T13:00:00+09:00
lastmod: 2023-01-09T13:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Firisctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2023.irisc.tf/home](https://2023.irisc.tf/home)
<br /><br />

397点を獲得し、最終順位は167位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/irisctf_2023_score.png" alt="irisctf_2023_score.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/irisctf_2023_score2.png" alt="irisctf_2023_score2.png">

<br><br>

主に、Network カテゴリのpcap問題に挑戦しました。

難しかった残りの２問（"Needle in the Haystack Secure" と "MICHAEL"）は、それなりに頑張ったんですが解けませんでした。。



<br /><br />
<br /><br />
## [Network]: wi-the-fi (247 points)
- - -
### Challenge
> You're probably used to pcaps captured at layer 3 in promiscuous mode, but do you know what to do with a pcap captured at layer 2 in monitor mode?

Attachment:

- BobertsonNet.cap


<br>

### Solution
WiFi通信なので、Aircrack-ng を使います。これは、過去にも何度かやってます。

参考：
- [Rooters CTF 2019](https://captureamerica.github.io/writeups/post/rootersctf_2019/)
- [UMDCTF 2021](https://captureamerica.github.io/writeups/post/umdctf_2021/)

<br />

<pre>
$ aircrack-ng BobertsonNet.cap -w /usr/share/wordlists/rockyou.txt
Reading packets, please wait...
Opening BobertsonNet.cap
Read 20980 packets.

   #  BSSID              ESSID                     Encryption

   1  A0:28:ED:C3:CC:C1  BobertsonNet              WPA (1 handshake)

Choosing first network as target.

Reading packets, please wait...
Opening BobertsonNet.cap
Read 20980 packets.

1 potential targets

                               Aircrack-ng 1.6

      [00:00:04] 14469/14344392 keys tested (3623.73 k/s)

      Time left: 1 hour, 5 minutes, 54 seconds                   0.10%

                           KEY FOUND! [ billybob1 ]


      Master Key     : 84 F1 82 B2 91 7C D3 DD 26 B2 19 34 C0 7B 27 2F
                       AC CF 32 B2 DF A3 56 01 9E FE 8C 7A 23 38 15 F8

      Transient Key  : BA 3F 2A 6D D0 DB 35 F8 D4 60 FF BB EF CB B5 1A
                       CE 60 FD 5F 58 D0 1F C4 3A EA 03 EA D4 DE 41 C9
                       4B 0B 91 19 1F 47 2D E3 8A 26 19 85 69 89 37 F3
                       2D 24 11 A4 C7 31 F9 D6 8F 6D 87 4F 17 87 01 9D

      EAPOL HMAC     : 80 8E 84 12 3F EA 18 4E 2D 25 9A 19 10 54 E2 51
</pre>



<br /><br />

見つかった Key (`billybob1`) を、Wireshark にて wpa-pwd に設定します。

<br />

あとは、`tcp contains "iris"` でサーチするとフラグが見つかります。Decrypted CCMP dataのタブの方に出てきます。

<img src="https://captureamerica.github.io/writeups/img/irisctf_2023_wifi.png" alt="irisctf_2023_wifi.png">


<br /><br />


Flag: `irisctf{4ircr4ck_g0_brrrrrrrrrrrrrrr}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />