---
title: "HackCon'19 Writeup"
date: 2019-08-25T10:00:00+09:00
lastmod: 2019-08-25T10:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://hackcon.online/](https://hackcon.online/)
<br /><br />
平日に開催されたので、早起き（朝4時）してちょっとやりました。
<br /><br />
結局、あんまり時間も取れなかったし、1問だけ解いて終了〜。
<br /><br />
いくつか気になる問題もあるので、Writeup見て勉強しておきます。


<br /><br />
# Stego: Small icon much wow
- - -
## Challenge
> One of my friends like to hide data in images.Help me to find out the secret in image.

Attachment:

- stego.jpg


<br />
## Solution

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
- - -
<br /><br />
<br /><br />

