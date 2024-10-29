---
title: "Bsides Jeddah CTF 2021 Writeup"
date: 2021-10-31T08:00:00+09:00
lastmod: 2021-10-31T08:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbsidesjeddah_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.bsidesjeddah.com/](https://ctf.bsidesjeddah.com/)
<br /><br />

2600点を獲得し、96位でした。

<br />

平日にやっていたCTFイベントです。

大きく分けて、pcap解析（+ Malware docxの解析）と Memory Dump解析になっています。

同じようなチャレンジや、その他いろいろと勉強になりそうなチャレンジが以下のサイトでいつでもできるようになっているようです。

[https://cyberdefenders.org/labs/](https://cyberdefenders.org/labs/)

時間があるときにやっておこうと思います。

<br><br>

以下、チャレンジの一覧です。

<br>

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall1.png" alt="bsidesjeddah_CTF_2021_Chall1.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall2.png" alt="bsidesjeddah_CTF_2021_Chall2.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall3.png" alt="bsidesjeddah_CTF_2021_Chall3.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall4.png" alt="bsidesjeddah_CTF_2021_Chall4.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall5.png" alt="bsidesjeddah_CTF_2021_Chall5.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall6.png" alt="bsidesjeddah_CTF_2021_Chall6.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall7.png" alt="bsidesjeddah_CTF_2021_Chall7.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall8.png" alt="bsidesjeddah_CTF_2021_Chall8.png">

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Chall9.png" alt="bsidesjeddah_CTF_2021_Chall9.png">


<br /><br />
## [PCAP]: Q#6 (50 points)
- - -
### Challenge
> What is the server certificate public key that was used in TLS session: 731300002437c17bdfa2593dd0e0b28d391e680f764b5db3c4059f7abadbb28e

<br />

### Solution
WireSharkのdisplay filterは以下を使いました。

<pre>
tls.handshake.session_id == 73:13:00:00:24:37:c1:7b:df:a2:59:3d:d0:e0:b2:8d:39:1e:68:0f:76:4b:5d:b3:c4:05:9f:7a:ba:db:b2:8e
</pre>

このときのpubkeyは以下になっていました。<br />
64089e29f386356f1ffbd64d7056ca0f1d489a09cd7ebda630f2b7394e319406

<br />

Flag: `64089e29f386356f1ffbd64d7056ca0f1d489a09cd7ebda630f2b7394e319406`



<br /><br />
<br /><br />
## [PCAP]: Q#8 (50 points)
- - -
### Challenge
> The attacker conducted a port scan on the victim machine. How many open ports did the attacker find?

<br />

### Solution
192.168.112.128 (attacker) が 192.168.112.139 に対してTCP Port Scanをしているので、以下のフィルターを使います。

<pre>
(ip.addr==192.168.112.139 && ip.addr==192.168.112.128) and tcp
</pre>

<br>
一旦別pcapファイルで保存し、coversationから、csvとして保存、3パケット以下はエディターを使って除外し、Port番号で sort。

<pre>
$ cat *.csv | cut -d, -f4 | sort -u
110
135
139
143
25
443
445
587
80
</pre>

あとはどのポートが実際にOpenなのか見ていくんですが、まぁ答えが1桁の数字なのはわかっているので、テキトーに数字を入れたら当たります。

<br />

Flag: `7`


<br /><br />
<br /><br />
## [PCAP]: Q#15 (100 points)
- - -
### Challenge
> What is the MD5 hash of the email attachment?

<br />

### Solution
Base64 Encodingされている部分をテキストに落としてからデコードします。

<pre>
$ cat attachment.txt | tr -d "\n" | base64 -d > web_server.docx

$ file web_server.docx
web_server.docx: Microsoft OOXML

$ md5sum web_server.docx
55e7660d9b21ba07fc34630d49445030  web_server.docx
</pre>

<br>

なお、Windowsで動く [NetworkMiner](https://www.netresec.com/?page=networkminer) というツールを使ってもファイルは取り出せるはずです。

<br />

Flag: `55e7660d9b21ba07fc34630d49445030`



<br /><br />
<br /><br />
## [PCAP]: Q#16 (100 points)
- - -
### Challenge
> What is the CVE number the attacker tried to exploit using the malicious document? Format: CVE-XXXX-XXXXX

<br />

### Solution
VirusTotalにすでにUploadされており、タグを見ると CVE-2021-40444 なのがわかります。

[https://www.virustotal.com/gui/file/c7073b7fc18b3ec20e476a7375e2a6695d273f671917a6627430e59534d3a138](https://www.virustotal.com/gui/file/c7073b7fc18b3ec20e476a7375e2a6695d273f671917a6627430e59534d3a138)

<br />

Flag: `CVE-2021-40444`



<br /><br />
<br /><br />
## [PCAP]: Q#17 (100 points)
- - -
### Challenge
> The malicious document file contains a URL to a malicious HTML file. Provide the URL for this file.

<br />

### Solution
VirusTotalの中で、Embedded URL を確認します。

<br />

Flag: `http://192.168.112.128/word.html`



<br /><br />
<br /><br />
## [PCAP]: Q#18 (100 points)
- - -
### Challenge
> What is the LinkType of the OLEObject related to the relationship which contains the malicious URL?

<br />

### Solution
docxをzip解凍して、LinkTypeをサーチします。

1行が長いので、trを使って見つけやすくしてます。

<pre>
$ unzip web_server.docx

$ grep -l -r LinkType .
./word/document.xml

$ cat ./word/document.xml | tr ">" "\n" | grep LinkType
&lt;o:LinkType
EnhancedMetaFile&lt;/o:LinkType

</pre>

<br />

Flag: `EnhancedMetaFile`



<br /><br />
<br /><br />
## [PCAP]: Q#19 (100 points)
- - -
### Challenge
> What is the Microsoft Office version installed on the victim machine?

<br />

### Solution
答えのフォーマットが XX.0.XXXX なのがわかっていたので、以下のように探しました。

<pre>
$ strings e3.pcap | egrep "\.0\."
User-Agent: Microsoft Office Word 2013 (15.0.4517) Windows NT 6.2
User-Agent: Microsoft Office Word 2013 (15.0.4517) Windows NT 6.2
:
:
</pre>

<br />

Flag: `15.0.4517`



<br /><br />
<br /><br />
## [PCAP]: Q#21 (100 points)
- - -
### Challenge
> The exploit takes advantage of a CAB vulnerability. Provide the vulnerability name?

<br />

### Solution
CVE-2021-40444 でググって見つけました。

[https://github.com/klezVirus/CVE-2021-40444](https://github.com/klezVirus/CVE-2021-40444) <br />
==> "Due to a Path traversal (ZipSlip) vulnerability in the CAB" だそうです。

<br />

Flag: `ZipSlip`



<br /><br />
<br /><br />
## [PCAP]: Q#23 (150 points)
- - -
### Challenge
> What is the path of malicious dll (msword.inf) after being dropped by the document file? Replace your username with IEUser

<br />

### Solution
.cabファイルをCuckooにあげてみたところ、以下が見つかりました。

<img src="https://captureamerica.github.io/writeups/img/bsidesjeddah_CTF_2021_Q23.png" alt="bsidesjeddah_CTF_2021_Q23.png">

<br />

多少Guessですが、Pathは同じだろうということで、フラグは以下です。

<br />

Flag: `C:\Users\IEUser\AppData\Local\Temp\msword.inf`




<br /><br />
<br /><br />
## [PCAP]: Q#24 (100 points)
- - -
### Challenge
> Analyzing the dll file, what is the API used to write the shellcode in the process memory?

<br />

### Solution
VirusTotalのOutputからわかります。

<br />

Flag: `WriteProcessMemory`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
