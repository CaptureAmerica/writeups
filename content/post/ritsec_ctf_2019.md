---
title: "RITSEC CTF 2019 Writeup"
date: 2019-11-18T13:00:00+09:00
lastmod: 2019-11-18T13:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://ctf.ritsec.club/challenges](https://ctf.ritsec.club/challenges)
<br /><br />

1012ポイントで、201番でした。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2019_score1.png" alt="ritsec_ctf_2019_score1.png"> <br />
<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2019_score2.png" alt="ritsec_ctf_2019_score2.png">

<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2019_chall_list1.png" alt="ritsec_ctf_2019_chall_list1.png"> <br />
<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2019_chall_list2.png" alt="ritsec_ctf_2019_chall_list2.png">
<br /><br />


結構、興味深いチャレンジがいくつかありました。解けなかったものは、後日復習しておきます。


<br /><br />
# [Pwn]: 999 Bottles
- - -
## Challenge
> Well, this is embarassing... I've accidentally compiled 999 ELF files with my password somewhere along the line, one character at a time.
<br /><br />
Solve these in order, each accepting one ASCII character. Keep going...eventually combining these solutions will match the regular expression RITSEC{.*}
<br /><br />
Good luck, and thanks for the help!

Attachments:

- bottles.zip

<br />
## Solution
bottles.zip の中には、999個（001.c.out ~ 999.c.out）のelf実行ファイルが入ってます。

ひとつGhidraにかけてみます。（001.c.out）

```C
undefined4 main(void)

{
  int in_GS_OFFSET;
  char local_15;
  int local_14;
  undefined *local_c;
  
  local_c = &stack0x00000004;
  local_14 = *(int *)(in_GS_OFFSET + 0x14);
  D = 0x3a;
  K = 0x2d;
  u = 0x4f;
  k = 99;
  N = 0x3c;
  y = 0x61;
  r = 0x58;
  c = 0x4e;
  I = 0x5a;
  J = 0x2b;
  L = 0x53;
  j = 0x73;
  i = 99;
  l = 0x40;
  O = 99;
  d = 0x5d;
  q = 0x2f;
  z = 0x49;
  s = 0x28;
  Z = 0x3e;
  P = 0x68;
  G = 0x2b;
  B = 0x2d;
  S = 0x79;
  b = 0x4a;
  A = 0x6a;
  v = 0x54;
  E = 0x2d;
  p = 'F';
  t = 0x62;
  F = 0x40;
  T = 0x42;
  C = 0x6b;
  Y = 0x29;
  w = 0x52;
  h = 0x3f;
  n = 0x2e;
  R = 0x76;
  m = 0x7b;
  X = 0x4b;
  Q = 0x65;
  x = 0x3b;
  U = 0x47;
  V = 0x75;
  g = 0x3b;
  o = 0x5e;
  W = 0x5f;
  a = 0x57;
  M = 0x4a;
  f = 0x42;
  H = 0x4d;
  e = 0x4c;
  puts("What is my character?");
  __isoc99_scanf(&DAT_08048806,&local_15);
  if (local_15 == p) {
    puts("OK!");
  }
  else {
    puts("Nope!\r");
  }
  if (local_14 != *(int *)(in_GS_OFFSET + 0x14)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

scanfで得た入力値と、変数pの値を比較しています。（65行目）

pは単なる変数名で、実際の値は 'F' なので、注意が必要です。（39行目）



<br />
002.c.out も見てみたところ、こちらは " (ダブルクォーテーション) との比較をしていました。

問題文にあるように、1つのファイルから1つの文字が取れるようです。



<br />
アプローチとしては、<br />
&nbsp;&nbsp;1. 各実行ファイルに対してBrute Forceする<br />
&nbsp;&nbsp;2. アセンブラから値を取得<br />
のどっちかかな、と思いましたが、今回は2番で行くことにしました。


<br />
objdumpの結果から、変数のアドレスとその値が見つかります。
```Asm
 8048612:	c6 05 39 a0 04 08 46 	mov    BYTE PTR ds:0x804a039,0x46  ; <-- !!
 8048619:	c6 05 37 a0 04 08 62 	mov    BYTE PTR ds:0x804a037,0x62
 8048620:	c6 05 3e a0 04 08 74 	mov    BYTE PTR ds:0x804a03e,0x74
:
(snip)
:
 80486f9:	83 ec 0c             	sub    esp,0xc
 80486fc:	68 f0 87 04 08       	push   0x80487f0
 8048701:	e8 7a fc ff ff       	call   8048380 <puts@plt>
 8048706:	83 c4 10             	add    esp,0x10
 8048709:	83 ec 08             	sub    esp,0x8
 804870c:	8d 45 f3             	lea    eax,[ebp-0xd]
 804870f:	50                   	push   eax
 8048710:	68 06 88 04 08       	push   0x8048806
 8048715:	e8 86 fc ff ff       	call   80483a0 <__isoc99_scanf@plt>
 804871a:	83 c4 10             	add    esp,0x10
 804871d:	0f b6 55 f3          	movzx  edx,BYTE PTR [ebp-0xd]
 8048721:	0f b6 05 39 a0 04 08 	movzx  eax,BYTE PTR ds:0x804a039  ; <-- !!
 8048728:	38 c2                	cmp    dl,al
```

<br />
それをベースに書いたスクリプト。
```Bash
#!/bin/bash
for i in `seq -f %03g 1 999`
do
    objdump -D -M intel ${i}.c.out > ${i}.txt
    grep "movzx  eax,BYTE PTR" ${i}.txt | cut -d: -f3 | xargs -IADDR grep ADDR ${i}.txt | grep -v movzx | tail -n1 | cut -d, -f2 | xxd -r -p
done
```

<br />
以下、実行結果です。
```
root@kali:~/Ritsec_CTF_2019/bottles# ./objdump_loop.sh 
F")|/f,:PsUUmKL*z(;N`QPtDZvX@j~=]q)AJ$w#g|Kehk)_$D_k;ESDz@ZK#=ZWfCo[GYG;FQ`W"mhoPlhr#W"N=RUxzjh"}&PJWWE@Jh%vKEIey`h,Xvxnsce/oqb,&*{#o&gMe-:RbSJO*>QIicbo<[sm>rmT@$@MgEwi:t{;U$WU[wRI!]+l[ngTqU>W:W*)$Sb},PmyEdJ~puK^zk!y.]M].vnBl!.OECe=JDiM|n+RihaL"x_p@M^P!f<Pa*j,A#"-,_n+z[?-bsQ.LBc{r<xR$[vuA/vMq%/_f-Yqg#$V}y&}[ReHU,`{^L/?rQlEW:Tv&l|&Ac#=FgrQR@a[Awwh-EEK@L:xf`M@E&}VFOZo:]ObRyiAomKD|,=pErk)wr%ir!J+`.DkN_`k>D}yrZ^@J&,qIRo|dvY+m@o:{cBvSE:G<;lGzV?NwpG*`VMnqSdXjN:r#=`=qq[qsn_kih>|M|WzEfz^|J>GTEc~k=KEbr@xOrP}iQnw#-uO-/]iCmbtBV+N*CmUiWl;STEf@}oB!e*!K#wmg](w.P]jm_o;Qec"AVm}JA#ua=hptzPVH?MSbopjRYUzDL_[[pA[)huW;=mhbIPAibwC[?o!"t.uKy[o~NiG;B=T.Rrn&OrF:&J&Xf`lr^wN${HnW<DtVjk.RITSEC{AuT057v}^W!xT;ImOU;ruPEQJKtRPlL#aGA.[PX,;,e~$t:*^dR?P_daEe,_{#r+iNrE-UVdeQh]GiIO+G;*.m=&+g#x|P!oXA(iFm({ZgTIohA<,.e(&KWw|>~,Wl<XH]<zT|H;gl.I_n"JAJ=n&}KJV{wsIFgsGHv@)+kj&>AQ~%xxK}<D?V+~oD?p=IZ$,Uy:L}$d[*VboIFrUuxp{{U!&e}-MSFDal(dIp^dN]D_`DYi!$VpNB-ZCUYKVxXta$Ur*!kSN`k>#fOzg]"ERlSIE~g)YlRi^oZg*Y,|ODGgbrXoqljJzChJ"c+ZRjy]}f{e
```

Flag `RITSEC{AuT057v}`







<br /><br />
<br /><br />
# [Forensics]: Take it to the Cleaners
- - -
## Challenge
> People hide things in images all the time! See if you can find what the artist forgot to take out in this one!

Attachments:

- ritsec_logo2.png

<br />
## Solution
1. exiftoolの結果
<pre>
ExifTool Version Number         : 11.47
File Name                       : ritsec_logo2.png
Directory                       : .
File Size                       : 4.3 kB
File Modification Date/Time     : 2019:11:16 05:25:12+09:00
File Access Date/Time           : 2019:11:16 05:25:28+09:00
File Inode Change Date/Time     : 2019:11:16 05:25:16+09:00
File Permissions                : rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 328
Image Height                    : 154
Bit Depth                       : 8
Color Type                      : Palette
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Palette                         : (Binary data 129 bytes, use -b option to extract)
Exif Byte Order                 : Big-endian (Motorola, MM)
Image Description               : Hi there! Looks like youre trying to solve the forensic_fails challenge! Good luck!
Resolution Unit                 : inches
Artist                          : Impos73r
Y Cb Cr Positioning             : Centered
Copyright                       : RITSEC 2018
Exif Version                    : 0231
Components Configuration        : Y, Cb, Cr, -
User Comment                    : RVZHRlJQe1NCRVJBRlZQRl9TTlZZRl9KQkFHX1VSWUNfTEJIX1VSRVJ9
Flashpix Version                : 0100
GPS Latitude Ref                : North
GPS Longitude Ref               : West
Image Size                      : 328x154
Megapixels                      : 0.051
</pre>
<br />

2. User Comment（RVZHRlJQe1NCRVJBRlZQRl9TTlZZRl9KQkFHX1VSWUNfTEJIX1VSRVJ9）をBase64デコード<br />
EVGFRP{SBERAFVPF_SNVYF_JBAG_URYC_LBH_URER}<br /><br />


3. rot13 <br/>
RITSEC{FORENSICS_FAILS_WONT_HELP_YOU_HERE}

<br />
Flag: `RITSEC{FORENSICS_FAILS_WONT_HELP_YOU_HERE}`



<br /><br />
<br /><br />
# [Forensics]: Long Gone
- - -
## Challenge
> That data? No it's long gone. It's basically history

Attachments:

- chromebin (232MB)

<br />
## Solution
1. フラグフォーマットの文字列 "RITSEC" をサーチ
<pre>
$ grep -i ritsec -r .
Binary file ./SwReporter/77.224.200/software_reporter_tool.exe matches
Binary file ./SwReporter/77.224.200/em004_64.dll matches
Binary file ./Default/Favicons matches
Binary file ./Default/Cache/data_1 matches
Binary file ./Default/History Provider Cache matches
Binary file ./Default/History matches
</pre>
問題文の中でも、history だと言っているので、Default/Historyを見ることにします。<br/>


2. ファイル タイプのチェック
<pre>
$ file History
History: SQLite 3.x database, last written using SQLite version 3029000
</pre>


3. sqlite3で中身をcsvファイルに保存します。
```
$ sqlite3 History
SQLite version 3.24.0 2018-06-04 14:10:15
Enter ".help" for usage hints.

sqlite> .tables
downloads                meta                     urls                   
downloads_slices         segment_usage            visit_source           
downloads_url_chains     segments                 visits                 
keyword_search_terms     typed_url_sync_metadata

sqlite> .mode csv
sqlite> .output visits.csv
sqlite> select * from visits;
sqlite> .output downloads.csv
sqlite> select * from downloads;
sqlite> .output urls.csv
sqlite> select * from urls;
sqlite> .output meta.csv
sqlite> select * from meta;
sqlite> .output keyword.csv
sqlite> select * from keyword_search_terms;
sqlite> .quit
```


4. keyword.csv とかに、以下のURLが入っていました。<br />
us-central-1.ritsec.club/l/relaxfizzblur


5. そのURLにアクセスすると、フラグが取れます。

<br />
Flag: `RITSEC{SP00KY_BR0WS3R_H1ST0RY}`



<br /><br />
<br /><br />
# [Web]: misdirection
- - -
## Challenge
> Looks like someone gave you the wrong directions!
<br /><br />
http://ctfchallenges.ritsec.club:5000/
<br /><br />
Flag format is RS{ }

<br />
## Solution
リダイレクトが走るので、-Lを付けてcurlを実行します。

<pre>
curl -v -L http://ctfchallenges.ritsec.club:5000/
*   Trying 129.21.228.105...
* TCP_NODELAY set
* Connected to ctfchallenges.ritsec.club (129.21.228.105) port 5000 (#0)
> GET / HTTP/1.1
> Host: ctfchallenges.ritsec.club:5000
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 302 FOUND
< Server: nginx/1.14.0 (Ubuntu)
< Date: Sat, 16 Nov 2019 08:05:41 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 211
< Connection: keep-alive
< Location: http://ctfchallenges.ritsec.club:5000/R
< 
* Ignoring the response-body
* Connection #0 to host ctfchallenges.ritsec.club left intact
* Issue another request to this URL: 'http://ctfchallenges.ritsec.club:5000/R'
* Found bundle for host ctfchallenges.ritsec.club: 0x7fb504c176f0 [can pipeline]
* Re-using existing connection! (#0) with host ctfchallenges.ritsec.club
* Connected to ctfchallenges.ritsec.club (129.21.228.105) port 5000 (#0)
> GET /R HTTP/1.1
> Host: ctfchallenges.ritsec.club:5000
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 302 FOUND
< Server: nginx/1.14.0 (Ubuntu)
< Date: Sat, 16 Nov 2019 08:05:41 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 211
< Connection: keep-alive
< Location: http://ctfchallenges.ritsec.club:5000/S
< 
* Ignoring the response-body
* Connection #0 to host ctfchallenges.ritsec.club left intact
* Issue another request to this URL: 'http://ctfchallenges.ritsec.club:5000/S'
* Found bundle for host ctfchallenges.ritsec.club: 0x7fb504c176f0 [can pipeline]
* Re-using existing connection! (#0) with host ctfchallenges.ritsec.club
* Connected to ctfchallenges.ritsec.club (129.21.228.105) port 5000 (#0)
> GET /S HTTP/1.1
> Host: ctfchallenges.ritsec.club:5000
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 302 FOUND
< Server: nginx/1.14.0 (Ubuntu)
< Date: Sat, 16 Nov 2019 08:05:41 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 211
< Connection: keep-alive
< Location: http://ctfchallenges.ritsec.club:5000/{

:
(snip)
:
* Ignoring the response-body
* Connection #0 to host ctfchallenges.ritsec.club left intact
* Maximum (50) redirects followed
curl: (47) Maximum (50) redirects followed

</pre>

Maxに達しちゃったので、`--max-redirs 100` オプションを付けてやってみたんですが、それでもMax(100)になっちゃったので、どうやら永遠にRedirectするようです。

結果をファイル（curl.log）に落として、
<pre>
$ cat curl.log | grep GET | cut -c 8 | tr -d '\n' ; echo
 RS{4!way5_Ke3p-m0v1ng}RS{4!way5_Ke3p-m0v1ng}RS{4!way5_Ke3p-m0v1ng}RS{4!way5_Ke3p-m0v1ng}RS{4!way5_Ke
</pre>

Flag `RS{4!way5_Ke3p-m0v1ng}`



<br /><br />
<br /><br />
# [Stego]: the_doge
- - -
## Challenge
> Steganography is the practice of concealing messages or information within other nonsecret data and images. The doge holds the information you want, feed the doge a treat to get the hidden message.

Attachments:

- the_doge.jpg


<br />
## Solution
問題文より、「ワンちゃんにtreatをあげて」(feed the doge a treat)とのことなので、

<pre>
# steghide extract -p 'treat' -sf the_doge.jpg 
wrote extracted data to "doge_ctf.txt".

# cat doge_ctf.txt 
RITSEC{hAppY_l1L_doG3}
</pre>

<br />
Flag: `RITSEC{hAppY_l1L_doG3}`



<br /><br />
<br /><br />
# [Misc]: Onion Layer Encoding
- - -
## Challenge
> Encoding is not encryption, but what if I just encode the flag with base16,32,64? If I encode my precious flag for 150 times, surely no one will be able to decode it, right?

Attachments:

- onionlayerencoding.txt


<br />
## Solution
Base16, Base32, Base64を150回かけたもの、ということで以下のようなコードを書きました。

```Python
#!/usr/bin/env python
import base64

def decode(data, i):
    try:
        data += '=' * (-len(data) % 4)
        return base64.b16decode(data)
    except:
        try:
            return base64.b32decode(data)
        except:
            try:
                return base64.b64decode(data)
            except:
                print("{:<30}: {}".format("Decode error", i))
                print data
                exit()

data = open("onionlayerencoding.txt",'r').read()
for i in range(150):
    data = decode(data, i)
    if i == 31:   # Added this to debug
        print(data)
print(data)
```

<br />
どうも途中でエラーになっちゃうので、decode()でiを引き数に取るようにして、どこら辺まで行っているかわかるようにしました。

33回目辺りでコケていたので、32回目、31回目がどうなっているか調べていたら、31回目ですでにフラグが取れていました。

150回じゃないの？？


Flag: `RITSEC{0n1On_L4y3R}`



<br /><br />
<br /><br />
# [Misc]: Crack me If You Can
- - -
## Challenge
> Rev up your GPUs...
<br /><br />
nc ctfchallenges.ritsec.club 8080
<br /><br />
Flag format RS{ }


<br />
## Solution
とりあえず繋いでみるとmd5が出てきたので、rainbowテーブル （https://crackstation.net/） でいけたんですが、shadow passwordは後回し。
```
$ nc ctfchallenges.ritsec.club 8080
Some moron just breached Meme Corp and decided to dump their passwords...  
In the meantime, prepare your GPUs, and get Ready... Set.... and go CRACK!
However... We have a theory that the passwords might come from probable-v2-top12000.txt, 500-worst-passwords.txt or darkweb2017-top10000.txt
29dbb8b29980bcf44e3ae91cc8a29e7a
romeo
Good job.

fc3c75843c684d2ce071347a349d099
jason
Good job.

$6$YdnPzCGiXVuIUZph$dXDWK/J1/8HIzM3vXQ7rKbnuUgPup0qhLeKbZA/ZpAG5v.R4zFYfLnB3669y7Th46j4T5/emWVTA/mTceW3ik/

Oof.
```

<br />
以下のファイルはググったらすぐ見つかりました。

- probable-v2-top12000.txt
- 500-worst-passwords.txt
- darkweb2017-top10000.txt

ちなみに、ncで繋ぐ度に、違うファイル名が出てくるんですが、まぁこの3つがあれば大丈夫でしょう。


<br />
がっちゃんこしておきました。
```
# cat *.txt | sort | uniq > passwd_list.txt
```

<br />
shadow passwordが出てきたら、hash.txtとして保存して、John the ripperでCrackします。
```
# john hash.txt --wordlist=./passwd_list.txt
```

<br />
Crackできないやつもいくつかありましたが、何回かやっているとそのうち解けます。3回続けてCrackできたらフラグゲットです。

<br />

Flag: `RS{H@$HM31FY0UCAN}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
