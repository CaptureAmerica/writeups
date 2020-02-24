---
title: "HackCon'19 Writeup | フラxxグゲット"
date: 2019-08-25T10:00:00+09:00
lastmod: 2019-11-04T19:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
(2019/11/04 - 復習しました)

URL: [https://hackcon.online/](https://hackcon.online/)
<br /><br />
平日に開催されたので、早起き（朝4時）してちょっとやりました。
<br /><br />
結局、あんまり時間も取れなかったし、1問だけ解いて終了〜。


<br /><br />
## [Stego]: Small icon much wow
- - -
### Challenge
> One of my friends like to hide data in images.Help me to find out the secret in image.

Attachment:

- stego.jpg


<br />
### Solution

```
$ exiftool stego.jpg 
ExifTool Version Number         : 11.47
File Name                       : stego.jpg
Directory                       : .
File Size                       : 47 kB
File Modification Date/Time     : 2019:08:23 05:57:40+09:00
File Access Date/Time           : 2019:08:23 06:02:09+09:00
File Inode Change Date/Time     : 2019:08:23 05:57:40+09:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Big-endian (Motorola, MM)
X Resolution                    : 1
Y Resolution                    : 1
Resolution Unit                 : None
Y Cb Cr Positioning             : Centered
Compression                     : JPEG (old-style)
Thumbnail Offset                : 202
Thumbnail Length                : 13391
Comment                         : Compressed by jpeg-recompress
Image Width                     : 1116
Image Height                    : 102
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1116x102
Megapixels                      : 0.114
Thumbnail Image                 : (Binary data 13391 bytes, use -b option to extract)
```

exiftoolの結果の最後のところで、「サムネイルが-bで取り出せるよ」と出てますね。

問題文もSmall iconと言っているし、怪しさ100倍です。

以下で取り出せます。
```
$ exiftool -b -ThumbnailImage stego.jpg > thumbnail.jpg
```

<br />
QR codeが取れるので、あとはリーダーで読み込むだけです。

Flag: `d4rk{flAg_h1dd3n_1n_th3_thumbnail}c0de`



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。


<br /><br />
## [Stego]: GIMP IT
- - -
### Challenge
> One of my logo designer left his job in the middle and hide my flag in it.Help me to get the flag.

Attachment:

- stego1.xcf


<br />
### Solution
stego1.xcf をテキストエディタ（自分はEmacs使ってます）で開いて眺めていくと、以下のテキストが見つかります。
<pre>
(text "504b 0304 1403 0000 0800 d005 d14e 2057\n40de 7505 0000 c416 0200 0800 0000 6461\n7461 2e74 7874 ecdd d16e 1331 1445 d15f\nbafe ff9f 03aa 628a 844c 508e 0fa9 baf6\na3a7 93eb acf4 6592 a633 9224 4992 2449\n9224 4992 2449 9224 4992 2449 9224 497a\n8956 a4f9 4399 9f7b f4e8 ec32 3b3d 767e\n64e2 c489 0722 4e7c 1127 7e7d 5ae6 e788\n1327 4e9c f8b9 c319 b173 57b8 09d7 30e8\n4e5b e126 5cc3 a03b 6d85 9b70 0d83 eeb4\n156e c235 0cba d356 b809 d730 e84e 5be1\n265c c3a0 3b6d 859b 700d 83ee b415 6ec2\n350c bad3 56b8 09d7 30e8 4e5b e126 5cc3\n2073 9dfc e8b9 ffbe 8367 f672 ae61 409c\n38f1 e434 e2c4 8913 cf4e 234e 9c38 f1ec\n34e2 af2f 7e7e bc7b afdb 3d03 e2c4 8927\na711 1fe2 c489 47a7 111f e2c4 8947 a711\n9f97 127f f4f5 c81c 7595 4ffc c634 e243\n9c38 f1e8 34e2 439c 38f1 e834 e243 fc13\n887f 2872 559e f974 3833 a361 903e 8338\n71e2 cf4b 1227 9e3b 373d 2d23 499c 78ee\ndcf4 b48c 2471 e2b9 73d3 d332 925f 43fc\n5c46 e3d5 d7fe 52e4 7bbd 2b52 fa99 ffaf\nb553 c46f ac9d 227e 63ed 14f1 1b6b a788\ndf58 3b45 fcc6 da29 e237 d64e 11bf b176\n8af8 8db5 5329 f174 99bf 70ce 7cb2 3c5f\n22e2 ed88 b723 de8e 783b e2ed 88b7 23de\n8e78 3be2 29a1 7b57 b86b 97fe 74f8 dedd\n69e6 5866 a78d 1d10 df95 7640 7c57 da01\nf15d 6907 c477 a51d 10df 9576 407c 57da\n01f1 5d69 07c4 77a5 1d10 4f4d 5bbb cc7f\n59ca ecb4 3183 787b 06f1 f60c e2ed 19c4\ndb33 88b7 6710 6fcf 20de 9e41 bc3d 8378\n7bc6 f702 df05 cdcc 4d7f 769c f1cb fc1e\n7c8c f8fb 21e2 4f1f 253e c4df 22fe 7e88\nf8d3 4789 0ff1 b788 bf1f 22fe f451 e243\nfcb7 32f7 1e4d bf57 9079 3720 7d8f 9bc6\n553e 71e2 c489 1327 4e7c 8638 71e2 c433\ncfe3 67c4 5f4d 3c73 b59d b99e 9e49 bccf\n9079 af20 734f 9a5f 1127 fe23 e2c4 8913\nffc6 debd 6437 0a04 0110 bc52 73ff cb8d\nb540 c67e 33e3 05d9 e55f e4da aa46 216f\n10d0 22de d4bc 23e2 d788 137f 449c 7827\nbeef 974e 9b67 73f7 e95e 4ace fcd7 33e2\nc43f 8838 71e2 c489 1327 4e9c f833 e2c7\naf11 af77 4a6a deef bed5 ea79 67c4 a7e6\n9d11 9f9a 7746 7c6a de19 f1a9 7967 c4a7\ne69d 119f 9a77 467c 6ade 19f1 a979 67c4\na7e6 bd29 7d6e 76dd a8b9 4fba f976 61df\n7f62 2359 af46 9c38 71e2 ed6a c489 1327\ndeae 469c 38f1 9f2f 5e3f 19bb ef48 1bc9\nb576 ddc7 4d9c f83f 234e 9cf8 ed35 9abf\n234e 9c38 71e2 7f8d 38f1 21f1 d9fb 86f7\n9dc7 3777 60d7 67fe c489 1327 4e9c 3871\ne2f7 5fd1 5835 5388 1327 4ebc 38eb 6df6\n653a 929a 4fa6 d678 469c f8ed 883f 22fe\n8c38 f1db 117f 44fc 1971 e2b7 23fe 88f8\nb5e2 7aed ecf7 07cd be51 cd95 e523 d120\n4e7c 11bf bd06 71e2 c489 1327 4e9c 38f1\nef27 3e71 85b7 596d ad62 0fa6 faee e8ff\n479c 3871 e2c4 8913 ef22 4e9c 38f1 36e2\n33e2 fbce 899b 2bd0 cd31 37bf 6fda ec23\n4c9c f822 4e9c 3871 e2c4 893f 227e fbbd\n1127 fefd c56b e746 68df f702 fb9c ebd7\n12bf d6ac 4b9c 3871 e267 cdba c489 1327\n7ed6 ac4b 9cf8 4f17 afef 667e df8e 7da3\n9adf c2a9 af36 1327 4e9c 3871 e2c4 df57\n4812 274e 9cf8 6b85 24f1 9de2 fb7e 93a6\n596d 6272 a34b 9cf8 224e 9c38 71e2 1f45\n9cf8 edd5 2626 1327 4efc ab34 714d b871\n5e2f ddbf cbfb f33f 19e2 d311 9f8e f874\nc4a7 233e 1df1 e988 4f47 7c3a e22f 7dfa\nf5df d79a 3d79 ebb3 f7e6 337a 463c 9bb7\n928e 24e2 9788 67f3 56d2 9144 fc12 f16c\nde4a 3a92 885f 229e cd5b 4947 12f1 4bc4\nb379 2ba9 7e16 b479 aab6 b973 b9d9 a1a9\nb627 4e9c 3871 e2c4 89bf 8d38 71e2 c489\nff36 f17d 67c7 cd0e 48fb 9eaf 7d54 3ecd\n4b9c 7877 ccc4 8913 27de 1e33 71e2 c489\nb7c7 4c9c f89f f6ed 1887 4118 88a2 e095\n9cfb 5f2e 0d12 4189 94c2 cf80 c4fc 1ab3\nec6c 055a ee21 ded4 6da6 507f 0318 5b88\n139f af4b 9c38 71e2 6d5d e2c4 8913 6feb\n1227 7e37 f146 add9 93ae dfcf 7f84 38f1\ne98e 8813 3f84 38f1 e98e 8813 3f84 38f1\ne98e 8813 ffc8 d2bb 34cf 5c4f b599 dbeb\nd46d e62d 0bce 1227 4e9c 787b 9638 71e2\nc4db b3c4 8913 7f92 f8bf 5cf3 87ea bacd\ne5e6 baf1 15e2 c489 1721 be87 3871 e245\n88ef 214e 9c78 11e2 63bc 0150 4b01 023f\n0314 0300 0008 00d0 05d1 4e20 5740 de75\n0500 00c4 1602 0008 0000 0000 0000 0000\n0020 80b4 8100 0000 0064 6174 612e 7478\n7450 4b05 0600 0000 0001 0001 0036 0000\n009b 0500 0000 00")
</pre>


"504b 0304" はZipマジックナンバーです。

スペースと"\n"をを取り除いて stego1.text と保存したのち、バイナリに変換してZip解凍します。
```
$ xxd -p -r stego1.text > steg1.zip
$ file steg1.zip
steg1.zip: Zip archive data, at least v?[0x314] to extract

$ unzip steg1.zip
Archive:  steg1.zip
  inflating: data.txt  
```

data.txtは、0と1がずらーっと並ぶテキストファイルです。

ここまではイベント中にできたんですが、ここから先がわからずに詰みました。。。


<br />
他の方のWriteupを見たら、これはQRコードになるようです。

{{% admonition tip "Tips" %}}
ファイルサイズが平方根とれる場合、正方形の画像にできる可能性あり！
{{% /admonition %}}


<br />
136900 = 370 x 370
```
$ ls -al data.txt | awk '{print $6, $10}'
136900 data.txt

$ python3
Python 3.7.4 (default, Sep  7 2019, 18:27:02) 
[Clang 10.0.1 (clang-1001.0.46.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import math
>>> sqrt(136900)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'sqrt' is not defined
>>> math.sqrt(136900)
370.0
```

<br />
PILを使って、pngとして保存します。<br />
（このコードは、他の方のWriteupのパクリになっちゃうので、省略します。）


<br />
Flag: `d4rk{L0t5_0f_th1ng5_t0_d0_1n_th15_chAll@ng3}c0de`



<br /><br />
<br /><br />
## [Stego]: Too cold for steg
- - -
### Challenge
> The challenge name is enough for you to solve it. Everything you need is present in the text file.

Attachment:

- final.txt


<br />
### Solution
今回、stegsnow のことを初めて知りました。

stegsnow - whitespace steganography program <br />
[http://manpages.ubuntu.com/manpages/bionic/man1/stegsnow.1.html](http://manpages.ubuntu.com/manpages/bionic/man1/stegsnow.1.html)

<br />
Kaliには入ってなかったので、インストールしました。
```
root@kali:~# apt-get install stegsnow
```


<br />
stegsnowで使うパスワード自体も、final.txtの中に入ってました。
<pre>
cells_--are sometimes found, particularly [password is : d4rkc0de-IIITD ]when resistant substances,
</pre>

<br />
```
root@kali:~/Hackcon_2019# stegsnow -C -p "d4rkc0de-IIITD" final.txt 
d4rk{h@ving_fun_w1th_st3gsn0w?}c0de
```

<br />
Flag: `d4rk{h@ving_fun_w1th_st3gsn0w?}c0de`



<br /><br />
<br /><br />
## [Misc]: Weird Text
- - -
### Challenge
> Someone sent me this file (mysterious.txt) .It contains only ><+-.,[] symbols and no other letters or numbers.

Attachment:

- mysterious.txt


<br />
### Solution
最初のBrainfuckはすぐわかるんですが、その後がさっぱりでした。

<pre>
D'`$@"][[ZX{Wx0/S-t1O`on&m*6jF3ge{SRQ`_u)9xqYun43kpihg-,jihgfH%c\aZ~}|VUZYXQVOsSRQPOHGkK-,BGF?cba`#"8=<;:3W705.RQ1qp.-&J*j('&}C{"!x}|u;yxqYun432jonmf,jihgfH%c\aZ~}|?UZSRvPONMRQPImMFEDIBfed'=BA@?>7[;{32V65.3,PONon,+*#"!EfeBzyx}v<;:9wponsrk1RQmlejc)('_d]b[!BXWV[ZSwWVONSLKonmMLK-IBf)dD&BA:98\[Z:3y76/St,+Op('KJI)"!~Ded"y~}|u;yxqYun432pongfed*hgfH%c\aZ~}|\[Z<Rv9ONSRQJImMFEDIBfed>&<A@?>7[;{32V65.3,PON.nm%$)"F&feBzyx}v<;:9wponsrk1RQmlejc)('eG]\[Z~X]\[ZYRvPUNSLpoONGFjJIBA@E>b%A@?87[5492VUTSt,+Op('KJI)"!~Ded"y~}|u;yxqYun432jonmf,jihgfH%c\aZ~}|{[TSRWVOsS54JOHlk.JIHAeEDCB;@?8\6|:32V05.R21q/('K%$#"FEfeBz!xw|uzs9wpotsrqj0Qgfe+chg`_^$bDZ_^]VzZYRWVUNrRQ3IHGLEiI+AF?cb%A@?87[|49876/.R2+*/(L,+*j(!~D$dc!x}vu;sxwpun4Ukj0nmf,diha'_dcb[!BXWV[ZSwW9OTSLKPImMLK-IBf@(>=BA@9]=<5:32V65.3,Pqp.-&J*#(!~D|#"yx>|^]yxq7Xnsrkjoh.fNdcha'&%$bDZ_^]VzZYRWVUNrqpJOHlLEJCg*@?DC<`_^>=<5:32V65.3,P*p.-&JI#('~D$#"!x>_{tyr8YXtmrk10/.fN+Lhg`_d]#"!_^WVUZSwQPUNMLp3INGFEihg*)ED=aA#"8=<;:3W705.RQP*/(-&J$)"'~Ded"y~}v<;:xq7Xnsrkjoh.fNdcha'&%]baZ~X]V[ZSwWVUNSRQPImlkKDIHG@?c=aA:?>7[ZYX8765.3,P*p.-&JIH(!&}C#c!x}|u;yxqYun432pongfed*hgfH%c\aZ~}|{[TSRWVOsS54JOHlkjDIBGFE>b%A@?87[5492VUT.-,P0)o'&J*j('&}CBA@a}v{zyr8YXtmrk10Qg-kd*hgfH%c\aZ~A]V[TSXWPtT6LKJImlLE-IBAeED=%;_?>=}|:3W165.-Qr*Non&%I#"F~}$#z@~`_{ts98vXnsrkjoh.fNdcha'&%$bDZ_^]VzZYRWVUNr54POHGFj-,BGF?cb%A@?87[5492V0/.R2+0)('KJ*)"!E%|#"y?`vut:xqYun43qponmfN+Lhg`_d]#"!~^]\UTYRvPUNSLpJONMLEiI+AF?cb%A@?87[5492V0/.R2+0)('KJ*)"!E}|{A!~}_uzs9qYun43qponmfN+Lhg`_d]#aZYX|V[ZSRWPtsMRQPIHGkK-,BGF?cba`#"8=<;:3W705.R,10/.-&J*j('&}CBcb~w|u;yxqYun4321onmfN+Lhg`_d]#"!~^]\UTYRvPUNSLpJIHMLEDhBGFE>C<;_^>=6;4X276/S32+*N.nm%IH('&feBz!x}vu;yxqYun432ponmfN+c)gfHdc\"Z_XW\UySXQPt7SRQPOHGkK-IHGF?>bB;#?>=<5Yzy765432+*N.-&%Ij"'&}C#c!xw|ut:[wvun4Ukjong-eMib(fH%cba`_X|V[ZYXQVOsSRQPONGk.JCHGF?>b%A@?87[;:9876/S3,+*)(L,+$#G'&%$d"y?}v{tyr8vuWVrkjongf,jihgfeG]#[`_X|\[ZYX:Pt7SRQPOHGkKJIHG@d>=<A:98\<54X87w5.-210/(L,+$j"'~D${"!x>|{]sxwpo5Vrkjongf,diba`edc\"CB^W\Uy<;WPUTSRQJn1MLEDhg*)E>b%A@?87[;4X81w/.32+O)o-,%$H"!&}C#c!x}|u;yrwvo5mlqponmf,jibg`e^$baZ~X]V[ZSwWVUNSRQPImlkKDIBf)dD=B;:^!~<;4X87wv432+*N.'m%$)(!EfeB"!~}_u;yxwvo5Vrkjongf,Mchgf_^$bDZ_^]VzZYXWVOsSRQJnHGFjJIBA@E>b%A@?87[;:3W1UT43210/.'K%$#G!&%|BA@x}v{zyr8vXnsrkjoh.leMihg`&d]baZYX|V[TSRQVONrLKPOHMFjDIHAe(>=<`#">765Y9270T.-,P0)o'&J$#G!~D|#"!x>|{ts9Zvutsrkpi/mfNjibg`_^$b[Z~^@?UZSRvV87MqQ32NMLEDh+*FE>=<`@">76;4X816/.-Q+0/.',%I)"!E%$#z@~`_{ts9qvon4Uqpingf,jihgfH%c\"`_^]\[TSwQPUNMqQJnH0LKJIBAe(DC<;:9]~};:9870T4t2+*Non&+$H(!&}C{Ayxw|uzs98votsrk1onmfN+ihaf_^$ED`Y^WVz=<XWPOsS54JImMFKJIHAe(DC<;_^]7<;:981U5.3,P0).'&J*)"!~}C{"!xw|ut:[qvon4Ukpi/gled*Ka`edc\"`_^@?UZSRvPt7SRQPOHGkEDIHG@d>C<;:9]=6Z49876/.R210)o'&J*)i!~D|#"yx>vuzyrwpun4Uqpihg-kjihgfH%c\"`B^W{UTSRQVONrLKJOHlLE-IBAeEDCBA#"8\<5:32V65.R21*p.-&J$)"'~D|#"!x>|uts9wpotsrqj0hmlkjihgfe^]#DZ_^]VzZYXWVOsSRQJnHMFj-IHG@d'=BA:?87[;4z8765432+*Non&+$H"'&}|{"y?w|uzs9Zvutsrkpi/mleMihg`&d]\aZ_X|\[TYXQuUTSR43ImM/KJIBAe(>=BA@987[|:9876/.3,P*Non&+$)"F&}C{"yx}v<;:[Zvotm3qponmfN+chg`_^$b[Z~^]V[ZYRvPUTSLpJINGFjJIHG@dD&BA:^87<5Y987w/43,P0)o'&J*j('&}CBA!x}|u;y[wpun4Ukjong-NMcba'eGcba`_X|\[ZYXQPUNMqQPIHGkEDIHG@d>C<;@987[549270Tu-2+O)o'&J$#G'~}${A!x>|{ts9Zvutsrkpi/gOejc)gIed]b[`Y}@?UZSRvVOTSLpPIHGkKJIHG@d>&<;@9]=6|:3W70/.R210)o'&J*)('&}Cd"!x>_uzyxwp6WVlkjongf,jihgfH%c\"Z_XW{[ZYXQuOTSLpPIHGkKJIHG@d>=<A:98\<54X81w/.32+O)o'&J*j(!E}${A@?}_uzyxqpo5srqSi/glkjiha'eGcba`_X|\[ZYX:Pt7SRQPOHGkKJIHG@dD&BA:^87<54X81w543,P0)o'&J$#G!~D$#cy?`_{ts9wponsrk1ihglkjiba'_dcbaZY}@?UZSRvV87SLKo2NGFjDCHG@dD=BA:9]=6;:981U5.R210/.-&J*)"!E%$d"y?}_uzsr8putsl2ponmfN+ihaf_^$E[!_^@?UZSRv98TSLKo2NGFjJIHG@dDC%;:?8\};:921U54321*p(L&+*)"'~D${A!x}|u;yrwpun4rkjong-e+ihaf_^$\a`_XW{UTYXQPtTMLKo2NGFjJIHAF?c=BA@?>=<5Y9276/SR210)o'&J*j('&}C{"y?}_u;yxwvo5srqpohg-e+ihgIed]#[Z_^W{[TYXWVOsMRQPIHGkKJIHG@d>=<;:?8\6|:3W165.-Q10)o'&J*)(!~D$d"y?`vutyxwp6WVlkjongf,jihgfH%c\"Z_X|VUZSRvV87MqQ32NMLEDh+*FE>=<`#">=65Y9270543,P0p(L&+*)"'~D|#"!x>|uts9wpotsrqj0Qglkd*hgfH%cba`_X|VUZSRvuU76LKo2NGFjJIHG@d>=<;:?8\}|49870/.R2+0)('K%*)(!E}|{A@?w|{zyr8vXnsrkjoh.leMihg`&dFEa`_X|\[ZYXQ9UNSRKoO10FKDh+GFE>CB;_?>=6|:3W765.-Q10/(L,l$#(!EfeB"y?}_u;yrqpon4lTjohg-kdcha'edc\"ZB^]\[TxXQ9UNSRKonH0LKJIBAe(DC<;_?8=<;432Vw54-,PO)o'&J*j('&}C{"y?}|ut:rqvo5mlqpohg-ed*)gfed]\"ZY^]\Uy<XWPUTMqK3INMLEihgGFE>=BA@?8\6|:32V6v.-,P*p(L&+*#GF&feBzyx}v<]yxwpun4lTjohg-Njchgf_^$bDZ_^]VzZYRWVUNrqKJOHMFjJI+Ae?>CBA:?87[;{z276543,PON('&%$H('&feB"y~}v{zyr8ponsrqj0hmf,+ihgfH%c\aZ~^]V[ZYRvPUTSRQJn10FKDh+GFE>CB;_?>=6|:3W76v.3,P0).'&J*j('&}C{"!x}|u;s9qvon4rkpingf,Mcba'eGc\[!_X|V[TSXWPOsS54JImMFKJCgA@ED=B;_9>=<5YX8765.R210/.'K+*)"!E%$#z@aw|{tyr8potml2johmled*hgfH%cba`_X|V[TSXWPOsS54JIm0/KJIHAF?cCBA@?>=6;4X876/S321*p(L,+*#i'&}C#"!x>v{t:rwvo5srqSi/mfNdc)gIed]b[`Y}@VUySXWPtsSLQJn10LEJCgA@ED=B;_?>=6;:981Uvu-2+O)o'&%I#G'&feBzb~w|{t:xqpun4lqponmf,+ihgfH%c\"!Y^]\UZSRv9UNSRKo2NGFjJIHG@dD=<A@?8\654981U54321*p(L&+*)"'~D${A!x}v<zs9wvunVl2ponmfN+ihgIed]#[`_^]Vz=YRWVUNMqpPIHGkKDIHG@?cCBA:"8\<;4381UT.32+*)Mnm%$#(!~}C#cy?}|u]s9wvo5mlqpohg-kjibg`_^$\[ZY}@?UZSRvVOTSLpPIHGkKJIHG@d>&<;@9]=}|:9876/.R,+O)o'&J*)('&}Cd"!x>|u]yxwpo5srkjoh.fN+ihaf_^$baZ_^]\Uyx;:VUTSLKo2NGFjDCHG@d>C<;:9]~6;4381UT.32+*)M-,+*#i'&}C#"!x>|{ts9qYun4Ukpi/gle+cba'_^]b[!Y}@VUyYRQVOs6RQJINGk.DCHAFE>C<`@9!=<5Y9216543,P0)o-&J*#(!~D${"yx>|u]s9Zvutsrkpi/mleMihg`&^cb[Z~^@?UZYXWPtTSLQJnH0LKJIBAe(DC<;_^]7<;:981U5.3,P*p.-&JIHG'&feBzyx}v<zyrwvo5srqSi/mleMib(fH%c\aZ~}|\[TYRv9ONSRQJImMFEDIBfed>bBA:^>=6|:3W70/.R210/.'K+$)(!EfeB"y?}v{zyr8vunmlqj0hPfkdc)gfH%]\a`_XW{[ZYX:Pt7SRQPOHGFjJ,HG@d'=BA:?87[;4z8765432+*N.'&+$H('~%${A!~}_u;yxqpo5mrk1Rhmlkjihgfe^]#DZ_^]VUyYRQVUNMqQPIHGkKJIHG@d>=<A:98\<54X87w5.-210/(L,+$j"'~Dedzy?>v{zsr8Yun4Ukpi/glkdc)('_d]#DZ_^]VzZYXWVOsMLKoIHMFEJIBfFE'=<A@?8\6|:32V0T.321*N.'&%*)(!Efe#"y?}|utyr8vonm3kpoh.leMihg`&d]baZY}@VUyYRQVOsS5KJIHl/EJIHAe?D=BA@9]=<;4X870T4321*p(L&%$H('&}C#c!x}|u;yxqYun4UTjih.fNdcha'&dFb[`Y}@?UZSRWPt76LKonNG/EiC+G@EDCB;_9>=<5:32VUT4t,10).'K%$#G'~}${"y?w_{ts9wpun4lTjohmlkjib(fH%c\"`_^]\[TSwQPUNMqQJnH0LKJIBAeEDCBA#"8\6;:9876/.R210/.'K+*)"F~}C#"y?wv{t:rwvunsl2ponPle+ihgfH%cba`_X|VUZYRvP8TSRQJONMLEiCBA@?c=aA:?>7[5{321Uvu-2+ONon&%I#"F&%${"y?w|ut:rqpon4Ukpi/gOejc)gIe^]#[Z~^]\[=<XWPt7SRQPOHGkKJIHG@d>=BA:9]=<54Xy76/St,+O)o'&%I)i!&}${A!~}_u;sxwputml2ponmfN+chg`_^$b[Z~^]V[ZSwWVOTSLp3ONMLEDhH*)ED=a$:^>=6|:3W765.-Q10)o'&J*)('&}C{cy?}_uzsr8vunVrk1onmfN+ihaf_^$\aZYX|VUZSXQVOsS5QPIHGkE-IBAe(>=<`#">=65Y38765.-Q10)o'&%I)('&}C#"y?}v{zyr8vunmlqj0hPfkdc)gfH%]\"ZYX|\UTYXQPUNMq43IHGkE-IBAe(DC<;:9]765Y9870/.R2+*Non&%I#"FED${"y?w|{zyr8vonm3kSinmlkdib(fHdc\"`_^W{[TSRWVOsSRQPOHGFjJIH*)E>b<;@?8=6Z:3y76/St,+Op('K+*j(!~Ded"y~}v<;sr8YXtmrk1oQglkd*Ka`e^]#D`_X|VUZSwWVOs6RQJIHl/KDCHAFE>C<`#"8=6Z:98765.3,P0/.-&J*j(!Efe#z@~}v<]\xwpun4rkSonmlkdihg`&d]\[Z~^@?UZSRvPt7SRQPOHGkKJIHG@d>C<;:9]=6|:3Wx0/43,P0)o'&J$#G'&fe#z@~}v<zyxqpun4Ukpi/gled*bJ`_^$bDZ_^]VUyYX:Pt7SRQPOHGkKJIHG@d>CBA@?8\}|:9876/.R2rq/(L,+$j"'~Ded"y~}v<]sxqp6tml21onmfN+Lhg`_d]#a`Y^]Vz=SXWPONMqQJIHGLEiC+G@EDCB;_?>=6|:3W76v.3,P0)o'&%Ij('&%${Ab~w|u;yxwvunVl2pohmle+chg`&dc\[!BXWV[ZSwWVONSLKonmMFKJCgGF?DCB;_?>=}|:9876/S-2+0)(L,%$#G'&%edz@a}v{zyr8vunml2ponmlkdcba'eGcb[Z~^]\Uy<XWPUTMqK3INMLEihg*)E>b%A@?87[;{921U/u3,+*Non&%*)(!E%|{Ab~w|u;yxqYonmrqj0ngled*bg`&dFEa`_X|\>=YXWPOs65KPIHGFjJIHAe?>bBA@"!7[;{32V65.3,P0)o'&%I)"FEfeBzyx}v<zyrwvo54rkji/mfN+Lhg`_d]#"!_XW\[ZYRv9ONSRQJIm0/KJIHAF?c=<;:?>=<;4X270TSt,+Op('K+*#i'&}C#"!xwv<]yxwpunsl2ponmfN+Lhg`_d]#"Z_X|V[TYRv9ONSRQJImMFEDIBfed'CBA@?>=<5Y9216543,Pqp.-&J$)"!~}C{"yx}v<;sr8vunm3qpong-edcbaf_%$bDZ_^]VzZYRWVUNrRQP2HGFjJIBGFE>b%;@?8\6|:32V05.RQP0/(-&+$H('&feB"y~}v{zyr8potml2pohmf,+ihgfH%c\aZ~^@?UZYRv98TSLKo21GLEiCHGFED=a$:?>7[ZY9876v43,P0)o'&J*j('&}CBA!~}|^]s9Zvutsrkpi/gOejc)(`H^]b[!~^]\UTYRvPUTSLpJINGkK-,BGF?cb%A@?87[5492VUTSt,+O/.n,+*)"F&%${cy?}|{zs[q7utVrkji/mfN+Lbgfe^]#aC_X|\[Z<RWVOTSLpPO10FEJCg*@E>=B;_L
</pre>

Malbolge っていうesolangらしいです。どこら辺をみたら判別付くのかがよくわかんないです。

<br />
オンラインのインタープリターで解読します。<br />
[http://www.malbolge.doleczek.pl/](http://www.malbolge.doleczek.pl/)


<br />
結果：
<pre>
0011 0002 0000 0010 0001 000f 0001 0004 0000 000e 000c 000d 000b 0006 000a 0003 0009 0000 0004 0012 0000 000b 0001 0000 00b1 0500 b62b 0400 b24c 0312 4c02 120e 0000 0002 0002 0032 0000 000a 0002 000d 000c 0009 0005 0000 0001 0006 0000 000b 0001 0000 00b1 0100 b72a 0500 0000 0100 0100 1d00 0000 0a00 0100 0900 0800 0000 0200 0000 0000 0700 0600 2000 5629 3b67 6e69 7274 532f 676e 616c 2f61 7661 6a4c 2815 0001 6e6c 746e 6972 7007 0001 6d61 6572 7453 746e 6972 502f 6f69 2f61 7661 6a13 0001 3b6d 6165 7274 5374 6e69 7250 2f6f 692f 6176 616a 4c15 0001 7475 6f03 0001 6d65 7473 7953 2f67 6e61 6c2f 6176 616a 1000 016e 6f69 7470 6563 7845 2f67 6e61 6c2f 6176 616a 1300 0174 6365 6a62 4f2f 676e 616c 2f61 7661 6a10 0001 6e65 675f 6761 6c66 0800 0121 0020 000c 1f00 071e 001d 000c 1c00 0765 6430 637d 7435 3362 5f35 7431 5f74 405f 3362 4062 5f33 6640 635f 6874 3177 5f33 6741 7567 6e41 6c5f 6331 7233 7430 3565 7b6b 7234 6436 0001 0000 0109 0008 000c 6176 616a 2e6e 6567 5f67 616c 660d 0001 656c 6946 6563 7275 6f53 0a00 011b 0007 736e 6f69 7470 6563 7845 0a00 0156 293b 676e 6972 7453 2f67 6e61 6c2f 6176 616a 4c5b 2816 0001 6e69 616d 0400 0165 6c62 6154 7265 626d 754e 656e 694c 0f00 0165 646f 4304 0001 5629 2803 0001 3e74 696e 693c 0600 011a 0007 1900 0718 0017 000a 1600 1500 0914 0008 1300 0812 0007 000a 2200 3700 0000 beba feca
</pre>


以下のサイトで16進数から文字列に変換。<br />
https://dencode.com/ja/string

印字可能文字以外のものが結構あるし、しかもフラグが反転してるとか、個人的にはちょっとやりすぎな気がします。

```
$ echo "ed0c}t53b_5t1_t@_3b@b_3f@c_ht1w_3gAugnAl_c1r3t05e{kr4d" | rev
d4rk{e50t3r1c_lAnguAg3_w1th_c@f3_b@b3_@t_1t5_b35t}c0de
```

<br />
Flag: `d4rk{e50t3r1c_lAnguAg3_w1th_c@f3_b@b3_@t_1t5_b35t}c0de`





<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

