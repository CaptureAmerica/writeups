---
title: "SEC-T CTF 2019 Writeup"
date: 2019-09-22T03:00:00+09:00
lastmod: 2019-09-22T03:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fsect_ctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://sect.ctf.rocks/](https://sect.ctf.rocks/)
<br /><br />
平日に一日やってたやつです。難しそうだったしスルーかなぁと思ってましたが、2つ解けました。
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/sectctf_Score.png" alt="sectctf_Score.png">
<br /><br />


<br /><br />
## [Misc/Forensics]: Diagram
- - -
### Challenge
> Don't trust the word of an android, they might cheat.

Attachment:

- diagram.tar.gz


<br />
### Solution
解凍して出てきたファイルはRTFファイルでした。

<pre>
$ file diagram
diagram: Rich Text Format data, version 1, unknown character set
</pre>

<br />
rtf用のツール（rtfdump.py, rtfobj）を使っていきます。

<pre>
$ rtfobj diagram.rtf -s all
rtfobj 0.53.1 on Python 2.7.15 - http://decalage.info/python/oletools
THIS IS WORK IN PROGRESS - Check updates regularly!
Please report any issue at https://github.com/decalage2/oletools/issues

===============================================================================
File: 'diagram.rtf' - size: 1456998 bytes
---+----------+---------------------------------------------------------------
id |index     |OLE Object                                                     
---+----------+---------------------------------------------------------------
0  |001235CDh |format_id: 2 (Embedded)                                        
   |          |class name: 'Excel.SheetMacroEnabled.12'                       
   |          |data size: 19456                                               
   |          |CLSID: 00020832-0000-0000-C000-000000000046                    
   |          |Microsoft Excel sheet with macro enabled                       
   |          |(Excel.SheetMacroEnabled.12)                                   
---+----------+---------------------------------------------------------------
Saving file embedded in OLE object #0:
  format_id  = 2
  class name = 'Excel.SheetMacroEnabled.12'
  data size  = 19456
  saving to file diagram.rtf_object_001235CD.bin


$ file diagram.rtf_object_001235CD.bin 
diagram.rtf_object_001235CD.bin: Composite Document File V2 Document, Cannot read section info


$ oledump.py diagram_object_001235CD.bin 
  1:       106 '\x01CompObj'
  2:         6 '\x03ObjInfo'
  3:     16871 'Package'
</pre>

<br>

olebrowseを使って、Packageの部分を取り出します。
<pre>
$ olebrowse diagram_object_001235CD.bin
</pre>

<img src="https://captureamerica.github.io/writeups/img/olebrowse1.png" alt="olebrowse1.png">

<img src="https://captureamerica.github.io/writeups/img/olebrowse2.png" alt="olebrowse2.png">

stream.bin としてファイルが取り出せました。

<pre>
$ file stream.bin 
stream.bin: Microsoft Excel 2007+

$ mv stream.bin stream.xls
</pre>

<br>

リネームして、Libre Officeで開きます。

<img src="https://captureamerica.github.io/writeups/img/stream_sheet1.png" alt="stream_sheet1.png">


<br />
画像の下に何か隠れているかな〜、と思ったけど、何もなかったです。

オフィス文書系の問題は、文字が白色で隠れていることが多いので、各シートでフォントの色を変えてみたら、シート3に数字が隠れてました。

<img src="https://captureamerica.github.io/writeups/img/stream_sheet3.png" alt="stream_sheet3.png">

どうやら、このデータをベースにグラフを描いているみたいですね。


<br />
たぶん、これがフラグだろう、ってことで文字に直します。

<pre>
$ python -c 'print("".join([chr(int(x)) for x in "83 69 67 84 123 52 110 100 114 48 105 100 115 95 115 104 48 117 108 100 95 98 51 95 110 49 99 101 125".split()]))'
SECT{4ndr0ids_sh0uld_b3_n1ce}
</pre>

<br>

Flag: `SECT{4ndr0ids_sh0uld_b3_n1ce}`




<br /><br />
<br /><br />
## [Misc/Forensics]: Mycat
- - -
### Challenge
> My cat is planing something, find the hidden msg


Attachment:

- mycat.tar.gz

<br />
### Solution
解凍して出てきたファイルはPDFファイルでした。

<pre>
$ file mycat
mycat: PDF document, version 1.4


$ mv mycat mycat.pdf
</pre>

<br />
pdf用のツールを使っていきます。

<pre>
$ pdf-parser.py mycat.pdf
PDF Comment '%PDF-1.4\n'

PDF Comment '%\xe2\xe3\xcf\xd3\n'

obj 3 0
 Type: /EmbeddeDFile
 Referencing: 1 0 R, 2 0 R
 Contains stream

  <<
    /Type /EmbeddeDFile
    /Filter /FlateDecode
    /Params 1 0 R
    /Length 2 0 R
  >>


obj 2 0
 Type: 
 Referencing: 

:
(長いので省略)
:


$ pdf-parser.py mycat.pdf --object=3 -d dump
obj 3 0
 Type: /EmbeddeDFile
 Referencing: 1 0 R, 2 0 R
 Contains stream

  <<
    /Type /EmbeddeDFile
    /Filter /FlateDecode
    /Params 1 0 R
    /Length 2 0 R
  >>


$ file dump
dump: zlib compressed data


$ zlib-flate -uncompress < dump > aaa


$ file aaa
aaa: PDF document, version 1.4


$ mv aaa aaa.pdf
</pre>

object 3を取り出したらzlibで圧縮されてて、解凍したら別のPDFファイルが出てきました。

<br />
PDFを開くと、ちょっと怖い顔のネコちゃんが出てきました。

<img src="https://captureamerica.github.io/writeups/img/mycat_pdf.png" alt="mycat_pdf.png">


<br />
最初、ステガノかと思っていろいろ画像を調べてたけど、さっぱりわからなくて諦めようかと思ったけど、よくよくPDFを見たら下の方が真っ黒な四角になっているのに気づきました。（黒さが微妙に違う）

確かに、調べてた画像サイズ（上のネコ部分）と、PDFで見えてる画像サイズが違う！


<br />
画像付きのPDFは、Libre Officeで開くパターンでしたね。（令和CTF、その他のCTFでも同様なものがありました。）

画像を移動したら、裏側からフラグが出てきました。

<img src="https://captureamerica.github.io/writeups/img/mycat_pdf2.png" alt="mycat_pdf2.png">

<br>

Flag: `SECT{3mb3dd3d_f1l3s_c0uld_b3_tr1cky}`




<br /><br />
## [Misc/Forensics/Memory]: favorite
- - -
### Challenge
> Androids are attack us, they are planing something big. We don't know where.

Attachment:

- favorite.7z

<br />
### (Unsolved)
これは解けなかったんですけど、ちょっとトライしました。後で、復習したいと思います。

<pre>
root@kali:~/SECTCTF_2019# volatility -f favorite.vmem imageinfo
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win8SP0x64, Win81U1x64, Win2012R2x64_18340, Win2012R2x64, Win2012x64, Win8SP1x64_18340, Win8SP1x64
                     AS Layer1 : SkipDuplicatesAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/root/SECTCTF_2019/favorite.vmem)
                      PAE type : No PAE
                           DTB : 0x1a7000L
                          KDBG : 0xf800be0aea30L
          Number of Processors : 2
     Image Type (Service Pack) : 0
                KPCR for CPU 0 : 0xfffff800be109000L
                KPCR for CPU 1 : 0xffffd00020be7000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-09-12 06:42:46 UTC+0000
     Image local date and time : 2019-09-11 23:42:46 -0700
</pre>

<br />
プロファイルによっては、結果が出ないようです。。。
<pre>
root@kali:~/SECTCTF_2019# volatility -f favorite.vmem --profile Win8SP0x64 filescan
Volatility Foundation Volatility Framework 2.6
Offset(P)            #Ptr   #Hnd Access Name
------------------ ------ ------ ------ ----
root@kali:~/SECTCTF_2019# 
root@kali:~/SECTCTF_2019# 
</pre>

<br />
2番目の`Win81U1x64`をプロファイルに指定しました。

チャレンジ名がfavoriteだし、ここら辺なのは間違いない気がするんですけど。
<pre>
root@kali:~/SECTCTF_2019# volatility -f favorite.vmem --profile Win81U1x64 filescan | grep -i favorite
Volatility Foundation Volatility Framework 2.6
0x000000003c626580     16      0 R--rwd \Device\HarddiskVolume2\Users\cyber\Favorites\desktop.ini
0x000000003d023c80  32768      1 R--rwd \Device\HarddiskVolume2\Users\cyber\Favorites
0x000000003d228cf0  32768      1 R--rwd \Device\HarddiskVolume2\Users\cyber\Favorites
0x000000003d9fa690     16      0 RW---- \Device\HarddiskVolume2\Users\cyber\Favorites\Bing.url


root@kali:~/SECTCTF_2019# volatility -f favorite.vmem --profile Win81U1x64 dumpfiles -Q 0x000000003d9fa690 -D .
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x3d9fa690   None   \Device\HarddiskVolume2\Users\cyber\Favorites\Bing.url


root@kali:~/SECTCTF_2019# cat file.None.0xffffe00001b639e0.dat 
[{000214A0-0000-0000-C000-000000000046}]
Prop3=19,2
[InternetShortcut]
IDList=
URL=http://go.microsoft.com/fwlink/p/?LinkId=255142
IconIndex=0
IconFile=%ProgramFiles%\Internet Explorer\Images\bing.ico
</pre>

他の箇所もいろいろ調べましたけど、わからなくて詰みました。

てかこれ、解けた人ひとりしかいないし。Writeup出てこないかもなぁ。。。


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

