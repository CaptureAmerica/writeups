---
title: "TUCTF 2019 Writeup"
date: 2019-12-03T13:00:00+09:00
lastmod: 2019-12-03T13:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://tuctf.com/](https://tuctf.com/)

もうサイトにアクセスできないようなので、最終順位がわかりません。

以下、チャレンジ一覧と解いた問題です。

<img src="https://captureamerica.github.io/writeups/img/tuctf_chal1.png" alt="tuctf_chal1.png">

<img src="https://captureamerica.github.io/writeups/img/tuctf_chal2.png" alt="tuctf_chal2.png">
<br /><br />



<br /><br />
# [Misc]: Onions
- - -
## Challenge
> Ogres are like files -- they have layers!

Attachment:

- shrek.jpg


<br />
## Solution
シュレックのJPEG画像です。著作権は大丈夫なんでしょうか。

foremostではなにも取れなかったので、binwalkから始めます。

binwalk -e でもextractされなかったので、--ddを使いました。

```Python
$ binwalk -e shrek.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
275566        0x4346E         7-zip archive data, version 0.4


$ binwalk --dd='.*' shrek.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
275566        0x4346E         7-zip archive data, version 0.4


$ cd _shrek.jpg.extracted/

$ ll
total 284
drwxrwxr-x 2 pan pan   4096 Dec  1 17:14 ./
drwxrwxr-x 3 pan pan   4096 Dec  1 17:14 ../
-rw-rw-r-- 1 pan pan 276104 Dec  1 17:14 0
-rw-rw-r-- 1 pan pan    538 Dec  1 17:14 4346E

$ file 0
0: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, comment: "CREATOR: gd-jpeg v1.0 (using IJG JPEG v62), default quality", baseline, precision 8, 2048x1234, frames 3

$ file 4346E
4346E: 7-zip archive data, version 0.4

$ 7z x 4346E
7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,1 CPU Intel(R) Core(TM) i5-3230M CPU @ 2.60GHz (306A9),ASM,AES-NI)

Scanning the drive for archives:
1 file, 538 bytes (1 KiB)

Extracting archive: 4346E
--
Path = 4346E
Type = 7z
Physical Size = 538
Headers Size = 130
Method = LZMA2:12
Solid = -
Blocks = 1

Everything is Ok

Size:       428
Compressed: 538

$ tar zxvf flag.tar.gz
flag.cpio

$ cpio -idv < flag.cpio
flag.lzma
1 block

$ unlzma flag.lzma

$ file flag
flag: current ar archive

$ ar -x flag

$ file flag1.txt
flag1.txt: bzip2 compressed data, block size = 900k

$ bunzip2 -d flag1.txt
bunzip2: Can't guess original name for flag1.txt -- using flag1.txt.out

$ file flag1.txt.out
flag1.txt.out: XZ compressed data

$ xz -dv flag1.txt.out
flag1.txt.out (1/1)
xz: flag1.txt.out: Filename has an unknown suffix, skipping

$ mv flag1.txt.out flag1.xz

$ xz -dv flag1.xz
flag1.xz (1/1)
  100 %                 96 B / 40 B = 2.400

$ file flag1
flag1: ASCII text

$ cat flag1
TUCTF{F1L3S4R3L1K30N10NSTH3YH4V3L4Y3RS}
```


Flag: `TUCTF{F1L3S4R3L1K30N10NSTH3YH4V3L4Y3RS}`


<br><br>
<br><br>
# [Misc]: Super Secret
- - -
## Challenge
> Something's blocking my flag from this file...

Attachment:

- document.odt


<br />
## Solution
```Python
$ file document.odt
document.odt: OpenDocument Text
```



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

