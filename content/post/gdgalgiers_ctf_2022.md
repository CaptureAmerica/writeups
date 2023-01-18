---
title: "GDG Algiers CTF 2022 Writeup"
date: 2023-01-17T13:00:00+09:00
lastmod: 2023-01-17T13:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fgdgalgiers_ctf_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.gdgalgiers.com/challenges](https://ctf.gdgalgiers.com/challenges)
<br /><br />

2022年の10月頃に参加したCTFです。復習をしたので、一個Writeupを残しておきます。

CTFTimeの履歴によると、70点を獲得し、最終順位は530位だったようです。<br />

<img src="https://captureamerica.github.io/writeups/img/gdgalgiers_ctf_2022_score.png" alt="gdgalgiers_ctf_2022_score.png">


<br /><br />
<br /><br />
## [Misc]: Intruder
- - -
### Challenge
> We heard a rumor that an intruder is in our GDG club and he's providing our enemies sensitive data to knock us down. We got this image through the network. Your task is to figure out the way he transmitted the data.

Attachment:

- AK.png


<br>

### Solution
これはイベント中には解けなかったやつです。

まず、以下がOfficial Writeupです。

[https://github.com/GDGAlgiers/gdg-algiers-ctf-2022-writeups](https://github.com/GDGAlgiers/gdg-algiers-ctf-2022-writeups)

<br /><br />

Official Writeupでは、pngcheckを使ったあとに、IDATデータをなんらかして取り出しているようですが、自分はzstegで取り出しました。

<br />

まずは、zstegを実行したときの結果。

<pre>
$ zsteg AK.png
[?] 413 bytes of extra data after zlib stream
extradata:0         .. file: zlib compressed data
    00000000: 78 9c ed d3 41 6a c4 30  10 44 d1 2b 25 cc fd ef  |x...Aj.0.D.+%...|
    00000010: 96 6d c0 74 f1 d5 32 66  ac fe d4 c6 d8 4a ab 9e  |.m.t..2f.....J..|
    00000020: 32 fa fc 7c 8c 31 c6 18  63 8c 79 2e bf 38 d5 fa  |2..|.1..c.y..8..|
    00000030: eb fb ff 6f f2 5f f5 3a  a8 9b a3 e3 6b 48 4f de  |...o._.:....kHO.|
    00000040: bc d7 41 dd 34 1d d9 bd  5a 5f 4d 5e d5 ed 9c 80  |..A.4...Z_M^....|
    00000050: 3a 75 79 5f a2 de e9 a0  4e dd ea e4 6a c2 93 1d  |:uy_....N...j...|
    00000060: d4 9d a7 e3 bb 57 22 32  33 df bb 9d 9e ea ce d3  |.....W"23.......|
    00000070: 91 64 e9 fe 33 ef a0 6e  82 ae 97 eb 5e bc 6d 35  |.d..3..n....^.m5|
    00000080: e1 7b a2 ee 1b 74 d5 af  9a ff e6 f9 dd cc 2e 72  |.{...t.........r|
    00000090: 3e ea d4 e5 6e 95 28 af  5f 55 e4 53 52 77 b6 8e  |>...n.(._U.SRw..|
    000000a0: 78 c9 09 e4 6e e4 ac c8  4a 75 73 74 55 b7 bc be  |x...n...JustU...|
    000000b0: e7 e2 0d ef ba 71 ea de  ab db 99 9f 5d 57 7b f5  |.....q......]W{.|
    000000c0: f5 de e6 ea ce d0 e5 7b  b7 7a 13 7b 5f 73 78 13  |.......{.z.{_sx.|
    000000d0: 75 27 e9 88 34 ab 79 37  f2 26 ef a2 6e a6 2e cf  |u'..4.y7.&..n...|
    000000e0: c9 fd 89 9d f7 cc 52 75  73 74 b9 43 be 65 3d 1d  |......Rust.C.e=.|
    000000f0: d9 f1 ae 7b a7 ee 8d 3a  22 22 cd 79 9f d5 09 f9  |...{...:"".y....|
meta date:create    .. text: "2013-03-24T23:31:44+00:00"
meta date:modify    .. [same as "meta date:create"]
meta software       .. text: "ImageMagick 6.6.9-7 2012-08-17 Q16 http://www.imagemagick.org"
meta Thumb::Document::Pages..

    00000000: 31                                                |1               |
meta Thumb::Image::height..

    00000000: 38 35 34                                          |854             |
meta Thumb::Image::Width..

    00000000: 32 33 36 35                                       |2365            |
meta Thumb::Mimetype.. text: "image/png"
meta Thumb::MTime   .. text: "1364167904"
meta Thumb::Size    .. text: "8.094MBB"
meta Thumb::URI     .. text: "file:///tmp/localcopy_5f9283566320-1.png"
imagedata           .. text: "YO[Sf$0&"
</pre>


<br />

ここから、extradata:0 を取り出していきます。

<pre>
$ zsteg -E "extradata:0" AK.png > extracted

$ file extracted
extracted: zlib compressed data

$ zlib-flate -uncompress < extracted
3030303030303030303030303030303030303030...(snip)...03030303030313131313131313...(snip)...
</pre>



<br /><br />
30と31の2つの値が見られるので、30を1に、31を0に変換します。これは、テキストエディタ（emacs）でやりました。

イベント中では、ここでstuckして降参しました。。。


<br /><br />

Official Writeupによると、このデータのサイズが12321 (111 x 111) であることから、QRコードであることが予想できる、とのこと。


<br /><br />

Emacsでファイルを保存すると、どうしても最後に改行が入ってしまってサイズが12322になるので、ちょっと気づきにくいかもです。。。

<pre>
$ ls -al aaa.txt | awk '{print $5, $9}'
12322 aaa.txt
</pre>

<br /><br />

ということで、echoを使ってファイルに保存します。

<pre>
$ echo -n "00000000...(snip)..." | wc -c
   12321

$ echo -n "00000000...(snip)..." > aaa.txt

$ ls -al aaa.txt | awk '{print $5, $9}'
12321 aaa.txt

</pre>

<br />

ちなみに、Terminalで出力して、ウィンドウサイズを調整すると、QRコードっぽいのがすでに確認できます。

<img src="https://captureamerica.github.io/writeups/img/gdgalgiers_ctf_2022_terminal.png" alt="gdgalgiers_ctf_2022_terminal.png">


<br /><br />

この手のQRコードのデータは、ツールを使って解読できた記憶があって、自分のブログを遡って見てみたら以下でやってました。

[Jornadas CTF 2021](https://captureamerica.github.io/writeups/post/jornadas_ctf_2021/)

<br />

ツールは以下です。ちなみに、Python2で動くツールです。

[https://github.com/waidotto/strong-qr-decoder](https://github.com/waidotto/strong-qr-decoder)


<br /><br />

まずは正方形にして、ファイルに保存します。

<pre>
$ echo -n "00000000...(snip)..." | fold -111 > aaa.txt
</pre>

<br />

<pre>
$ python ./strong-qr-decoder/sqrd.py aaa.txt
error: 辺の長さがおかしいです
</pre>

<br />

どうやら、111 x 111 という正方形はQRコードとしては無効らしいです。

<br /><br />

余分な箇所を削除して、89 x 89 で試してみましたが、それでもダメでした。。。

<pre>
$ cat aaa.txt | head -n 100 | tail -n 89 > bbb.txt
$ cat bbb.txt | cut -c 12-100 > ccc.txt
$ python ./strong-qr-decoder/sqrd.py ccc.txt
Traceback (most recent call last):
  File "./strong-qr-decoder/sqrd.py", line 1025, in <module>
    num = int(data_bits[:10], 2)
ValueError: invalid literal for int() with base 2: ''
</pre>


<br /><br />

Solutionとしては、やっぱり一度画像にしてからQRコードを読み取った方がいいみたいです。

これは、過去に以下のCTFで似たようなものをやっていたので、そのときのコードを流用しました。

[X-MAS CTF 2019](https://captureamerica.github.io/writeups/post/xmas_htsp_ctf/)

<br /><br />

aaa.txt は予め以下のように作成しておきます。

<pre>
$ echo -n "00000000...(snip)..." | fold -111 > aaa.txt
</pre>

<br />

書いたコード：

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from PIL import Image

MAX=111
f=open("aaa.txt","r").read().split("\n")
a=[]
for line in f:
    for y in range(MAX):
        val = line[y]
        if val=='0':
	    a.append((255,255,255))
        else:
	    a.append((0,0,0))
                 
im = Image.new("RGB",(MAX,MAX))
im.putdata(a)
im.save("QR.png")
```

<br />

<img src="https://captureamerica.github.io/writeups/img/gdgalgiers_ctf_2022_QR.png" alt="gdgalgiers_ctf_2022_QR.png">


<br /><br />

ちなみに、Official Writeup のコードの中で、MAX=25というのは間違いで、MAX=111が正解ですね。

<img src="https://captureamerica.github.io/writeups/img/gdgalgiers_ctf_2022_official_writeup.png" alt="gdgalgiers_ctf_2022_official_writeup.png">


<br /><br />

Flag: `CyberErudites{pN6_ChUnK5_4r3_N1c3_P14C3_70_H1d3}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />