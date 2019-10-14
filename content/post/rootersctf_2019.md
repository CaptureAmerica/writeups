---
title: "Rooters CTF 2019 Writeup"
date: 2019-10-14T16:00:00+09:00
lastmod: 2019-10-14T16:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://rootersctf.in/](https://rootersctf.in/)
<br /><br />
picoCTF 2019の直後でほとんど時間がなかったですが、ちょっとだけやりました。


<br /><br />
# [Forensics]: You Can't See Me
- - -
## Challenge
> See for yourself

Attachments:

- youcantseeme.pdf

<br />
## Solution
100 ページくらい真っ白が続くPDFが与えられます。

やったこと：

- pdf-parser.py<br />
- pdfid.py<br />
- Adobe Reader（特定のページになんかしらテキストっぽいのがあるのがわかるんですが、フォントの色の変え方がわかんなかったです。）<br />
- Libre Office (少しずつフォントの色は変えられたんですが、全ページまとめてやる方法がわかんなかったです。)<br />
- PDFStreamDumper (しばらく使わないうちに、起動がうまくいかなくなってた。。。)

<br />
以下でフラグをゲットしました。

- pdftotext -f 16 youcantseeme.pdf text.txt

(16を指定したのは、最初に見つけたテキストが16ページだったからです。)

Flag: `rooters{Ja1_US1CT}ctf`



<br /><br />
<br /><br />
# [Forensics]: Find The Pass
- - -
## Challenge
> Find the admin Password. Put the flag in rooters{}ctf

Attachments:

- inc0gnito.cap (inc0gnito.pcapにリネームしました)


<br />
## Solution
```
# aircrack-ng inc0gnito.pcap

                                                      Aircrack-ng 1.5.2 


                                         [00:00:01] Tested 50922 keys (got 20006 IVs)

   KB    depth   byte(vote)
    0    0/  1   FF(32256) CE(25856) 0F(25600) 07(24832) 59(24832) 73(24576) 7B(24320) CC(24320) F5(24320) 
    1   41/ 54   00(22272) 11(22016) 1E(22016) 25(22016) 33(22016) 3F(22016) 52(22016) 64(22016) 88(22016) 
    2   11/ 19   AD(23808) EB(23808) F4(23808) 06(23808) 5F(23552) 8A(23552) 98(23552) 29(23552) 58(23296) 
    3    2/  5   BE(26112) 58(25600) A7(25600) EA(25088) BF(24320) 5C(24064) 70(24064) B9(24064) BE(24064) 
    4    0/ 10   EF(28928) 8F(27392) 30(27392) BF(26112) AD(25856) 6A(25856) 04(25344) 19(25344) C5(24832) 

                         KEY FOUND! [ FF:DE:AD:BE:EF ] 
	Decrypted correctly: 100%
```

Wireshark > Protocols > IEEE 802.11 > Decryption KeysでWEP Keyを設定して復号します。

文字列 "admin" をサーチしたら、Basic Authの箇所 (Frame #2209057) がありました。

<pre>
Credentials: admin:blu3_c0rp_p4ss
</pre>

Flag: `rooters{blu3_c0rp_p4ss}ctf`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

