---
title: "TexSAW CTF 2023 Writeup"
date: 2023-04-17T21:00:00+09:00
lastmod: 2023-04-17T21:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Ftaxsaw_ctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://www.texsaw2023.com/challenges](https://www.texsaw2023.com/challenges)
<br /><br />

2510ポイントで、最終順位は16位でした。(234チーム中)

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_score.png" alt="taxsaw_ctf_2023_score.png">

<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_chall1.png" alt="taxsaw_ctf_2023_chall1.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_chall2.png" alt="taxsaw_ctf_2023_chall2.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_chall3.png" alt="taxsaw_ctf_2023_chall3.png"> <br />

<br /><br />


比較的、解いたチームが少ないチャレンジのWriteupを4つ残しておきます。

## [Forensics]: Image Extraction (150 points)
- - -
### Challenge
> Uncover a jpg file from the given file. The flag is in that image.

Attachments:

- Challenge.pdf

<br />
### Solution

Challenge.pdfは、拡張子は.pdfですが、PDFファイルではありません。

<pre>
$ file Challenge.pdf
Challenge.pdf: data
</pre>

<br />

先頭2バイトは `ffd8` ですが、jpeg のマジック `ffd8 ffe0` ではありません。

xxdで見たときに、別のファイル名 (cute-cat-photos-3593441022.jpg) が先頭の方に出てくるのと、末尾に `PK` が出てくるので、どちらかというとZipファイルっぽいです。

<pre>
$ xxd Challenge.pdf | head -n4
00000000: ffd8 1400 0000 0800 e852 8b56 ce00 2e61  .........R.V...a
00000010: db9c 0100 89ac 0100 1e00 0000 6375 7465  ............cute
00000020: 2d63 6174 2d70 686f 746f 732d 3335 3933  -cat-photos-3593
00000030: 3434 3130 3232 2e6a 7067 9c5a 6958 525f  441022.jpg.ZiXR_

$ xxd Challenge.pdf | tail -n4
00019d60: 670a 0020 0000 0000 0001 0018 006d 24a1  g.. .........m$.
00019d70: 8889 6cd9 0100 0000 0000 0000 0000 0000  ..l.............
00019d80: 0000 0000 0050 4b05 0600 0000 0001 0001  .....PK.........
00019d90: 0070 0000 0017 9d01 0000 00              .p.........
</pre>

<br />
別のチャレンジでダウンロードしてた Zipファイルを見てみると、以下のようになっていました。

結構似ているので、やっぱりZipファイルっぽいです。

<pre>
$ file rock.rock
rock.rock: Zip archive data, at least v2.0 to extract, compression method=deflate

$ xxd rock.rock | head -n4
00000000: 504b 0304 1400 0000 0800 f878 8456 3753  PK.........x.V7S
00000010: 5272 dc96 0c00 00c2 0c00 0900 0000 726f  Rr............ro
00000020: 636b 2e72 6f63 6bec b663 8c2f d0f3 e6d9  ck.rock..c./....
00000030: b671 fbdb b66d dbb6 6ddb b66d f3b6 6deb  .q...m..m..m..m.
</pre>

<br />

ということで、バイナリエディタで `ffd8` の部分を、`504b 0304`で置き換えたら Zipファイルが復元できて、中に入っていた画像が取り出せます。

<br />

そこに、フラグの元となる数値が書かれているので、文字に変換するだけです。

<pre>
$ python -c 'print("".join([chr(int(x,16)) for x in "74 65 78 73 61 77 7B 6F 4E 65 5F 43 75 54 65 5F 43 61 54 21 7D".split()]))'
texsaw{oNe_CuTe_CaT!}
</pre>

<br />

Flag: `texsaw{oNe_CuTe_CaT!}`



<br /><br />
<br /><br />
## [Forensics]: Matryoshka (doll1) (400 points)
- - -
### Challenge
> You have been given a zip file containing an image of a Matryoshka doll. Hidden inside the image is a text file is a link to a zip file containing an image of a smaller Matryoshka doll (along with potentially other files), which contains a text file containing a link to a zip file with an image of an even smaller Matryoshka, and so on. You will need to use some steganography tools to open each doll and extract its contents. However, a password is required to open each doll. Use your security skills and knowledge to open all the dolls!

Attachments:

- doll1.jpg

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll1.jpg" alt="taxsaw_ctf_2023_doll1.jpg"> <br />

<br />
### Solution

繰り返し unzip させられるチャレンジかと思いきや、解凍する度に別のチャレンジが出てきて、結局のところ複数のチャレンジを全て解けた人がポイントを得られる、というものでした。

ということで、4つに分けて記載します。

<br />

まず最初のdoll1.jpgですが、「青い空を見上げればいつもそこに白い猫」のビット抽出のところで、「Steghide適用可能性あり」「password:sneakythief」の情報が得られます。優秀！！

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll1_neko.png" alt="taxsaw_ctf_2023_doll1_neko.png"> <br />

<br />

<pre>
$ steghide extract -p 'sneakythief' -sf doll1.jpg
wrote extracted data to "linktodoll2.txt".
</pre>

次の doll2.zip へのリンクが得られました。

以下へ続く。。。


<br /><br />
<br /><br />
## [Forensics]: Matryoshka (doll2)
- - -
### Challenge

Attachments:

- doll2.jpg

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll2.jpg" alt="taxsaw_ctf_2023_doll2.jpg"> <br />

- md5hash1.txt (dd679302de4ce83d961f95a1facca536)
- md5hash2.txt (5400711cd704e87ed3fd11556cc174ae)


<br />
### Solution

Saltを使ったMD5のcrackです。

以下のようなファイルを用意します。

- aaa.txt
<pre>
dd679302de4ce83d961f95a1facca536:uwu
</pre>

- bbb.txt
<pre>
5400711cd704e87ed3fd11556cc174ae:owo
</pre>

<br />

hashcatを使います。

<pre>
$ hashcat -a 0 -m 20 aaa.txt /usr/share/wordlists/rockyou.txt
hashcat (v6.2.5) starting

OpenCL API (OpenCL 2.0 pocl 1.8  Linux, None+Asserts, RELOC, LLVM 11.1.0, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=====================================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-1030NG7 CPU @ 1.10GHz, 1426/2917 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimim salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

dd679302de4ce83d961f95a1facca536:uwu:pizza
:
(snip)
</pre>

<br />

<pre>
$ hashcat -a 0 -m 10 bbb.txt /usr/share/wordlists/rockyou.txt
hashcat (v6.2.5) starting

OpenCL API (OpenCL 2.0 pocl 1.8  Linux, None+Asserts, RELOC, LLVM 11.1.0, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=====================================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-1030NG7 CPU @ 1.10GHz, 1426/2917 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimim salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

5400711cd704e87ed3fd11556cc174ae:owo:pasta
:
(snip)
</pre>

<br />

ということで、

<pre>
$ steghide extract -p 'pizzapasta' -sf doll2.jpg
wrote extracted data to "linktodoll3.txt".
</pre>

次の doll3.zip へのリンクが得られました。

以下へ続く。。。


<br /><br />
<br /><br />
## [Forensics]: Matryoshka (doll3)
- - -
### Challenge

Attachments:

- doll3.jpg

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll3.jpg" alt="taxsaw_ctf_2023_doll3.jpg"> <br />

- hint1/convertthenshift.txt

- hint2/weirdaudio.wav

- hint3/noize.js


<br />
### Solution

convertthenshift.txt の中身は以下の通り。
<pre>
01100101 01110011 01110000 00100000 01111001 01101100 01111000 01110000 00100000 01111010 01110001 00100000 01100101 01110011 01110000 00100000 01111010 01100011 01110010 01101100 01111001 01110100 01101011 01101100 01100101 01110100 01111010 01111001
</pre>

CyberChefのbinaryレシピを使ったと思います。

"esp ylxp zq esp zcrlytkletzy" が変換後の文字列。

これをrot15すると、"the name of the organization" になります。

<br /><br />

weirdaudio.wav は、よくあるスペクトラムを見るやつです。

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll3_wav.png" alt="taxsaw_ctf_2023_doll3_wav.png"> <br />


<br /><br />

noize.js は、エディタで眺めて、以下を見つけました。

<pre>
        '/defco',
        'n.org/',
</pre>

<br /><br />

ということで、全部まとめると、

What is the name of the organization that issued the web certificate to "defcon.org"?

なんだと思います。

<br /><br />

https://defcon.org/ にアクセスして、サーバー証明書を確認。

"Hellenic Academic and Research Institutions CA" が書かれていますが、先頭から43文字目までを使うということで、末尾の ` CA`は削除。

<pre>
$ echo -n 'Hellenic Academic and Research Institutions CA' | wc -c
46
</pre>

<br />

<pre>
$ steghide extract -p 'Hellenic Academic and Research Institutions' -sf doll3.jpg
wrote extracted data to "linktodoll4.txt".
</pre>

次の doll4.zip へのリンクが得られました。

以下へ続く。。。


<br /><br />
<br /><br />
## [Forensics]: Matryoshka (doll4)
- - -
### Challenge

Attachments:

- doll4.jpg

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll4.jpg" alt="taxsaw_ctf_2023_doll4.jpg"> <br />

- guardian (ELF 64bit)



<br />
### Solution

ghidraでデコンパイルします。

```C

void main(void)

{
  undefined8 local_28;
  undefined8 local_20;
  undefined4 local_18;
  
  setbuf(stdout,(char *)0x0);
  setbuf(stdin,(char *)0x0);
  setbuf(stderr,(char *)0x0);
  local_28 = 0;
  local_20 = 0;
  local_18 = 0;
  genesis(&local_28);
  puts("Here is the password:");
  exitium(&local_28);
  puts((char *)&local_28);
  sleep(2);
  puts(
      "\nUh oh, it looks like the password got severely damaged somehow. I am afraid there is nothin g I can do to fix it."
      );
  return;
}

```

<br />

一回動かしてみて、動作も確認しておきます。

<pre>
$ ./guardian
Here is the password:
0u1u111t1u1u0t05020

Uh oh, it looks like the password got severely damaged somehow. I am afraid there is nothing I can do to fix it.
</pre>

<br />

なんとなく、genesis()の直後では、ちゃんと文字列が取れているけど、exitium()で壊れて、その結果が表示されている感じがしました（直感）。

<br />

ということで、gdbで動かし、genesis()を過ぎた辺りでスタックを眺めていたら、それっぽい文字列が見つかりました。

<pre>
$rax   : 0x00007fffffffe0c3  →  0x0000000000000000
$rbx   : 0x0
$rcx   : 0x12
$rdx   : 0x13
$rsp   : 0x00007fffffffe0a0  →  0x00007fffffffe1e8  →  0x00007fffffffe486
$rbp   : 0x00007fffffffe0d0  →  0x0000000000000001
$rsi   : 0x0
$rdi   : 0x00007fffffffe0b0  →  "queenofthenest85321"
</pre>

<br>

<pre>
$ steghide extract -p "queenofthenest85321" -sf doll4.jpg
wrote extracted data to "linktodoll5.txt".
</pre>

次の doll5.jpg へのリンクが得られました。

そろそろフラグを出してください、と思っていたら、やっとここで出てきました。

<img src="https://captureamerica.github.io/writeups/img/taxsaw_ctf_2023_doll5.jpg" alt="taxsaw_ctf_2023_doll5.jpg"> <br />

<br />

Flag: `texsaw{r3curs3_reCurSe_rECurSe_dOwn_thE_r4Bbi7_H01e}`


<br /><br />
<br /><br />
## [Web]: MIT of The South (150 points)
- - -
### Challenge
> Welcome to UTD! We like to call ourselves the MIT of the South (not really). The flag for this challenge is hidden in one of the classrooms, can you find it?
<br /><br />
[http://18.216.238.24:1004](http://18.216.238.24:1004)

<br />
### Solution

まず、アクセスをしてみると、http://18.216.238.24:1004/webpage/files/dir/index.html 辺りにredirectされます。

<br />

robots.txtがあるかな、と思っていろいろ見ていると、以下がありました。

http://18.216.238.24:1004/webpage/files/dir/robots.txt

<pre>
Robots!?
There are no robots here!
Only Temoc, and his army of tobors!!
</pre>

<br />

で、http://18.216.238.24:1004/webpage/files/dir/tobors.txt にアクセスすると、ディレクトリが大量に書かれたリストが得られます。

<pre>
/ad/
/ad/1.100/
/ad/1.101/
/ad/1.102/
/ad/1.103/
/ad/1.104/
:
(snip)
</pre>

<br />

このうちの、どこかにフラグがあるっぽいです。

URLの全リストを作成し、`wget -i list.txt`で、全ての結果を取ってくるようにしました。

<br>

ある程度、ダウンロードできたところで grep したらフラグは取れてました。

<pre>
$ grep -r texsaw .
./index.html.3989:			texsaw{woo0OOo0oOo00o0OOOo0ooo0o00Osh}
</pre>

<br />

Flag: `texsaw{woo0OOo0oOo00o0OOOo0ooo0o00Osh}`


<br /><br />
<br /><br />
## [Web]: Git er' done (200 points)
- - -
### Challenge
> I've made my first website but I still have a lot of tasks to do. Can you check it out and give me some feedback?
<br /><br />
[http://18.216.238.24:1002/](http://18.216.238.24:1002/)

<br />
### Solution

タイトルから、明らかにgitが関係してるっぽいのはわかります。

<pre>
$ git-dumper http://18.216.238.24:1002/.git/ ./output/
</pre>

<br />

ダウンロードしてきたファイルの中に、すでにflag.txtがありました。

git-dumper 優秀！

<pre>
$ ll
total 12
drwxr-xr-x 1 501  224 Apr 16 20:39 ./
drwxr-xr-x 1 501   96 Apr 16 20:39 ../
-rw-r--r-- 1 501   32 Apr 16 20:39 flag.txt
drwxr-xr-x 1 501  384 Apr 16 20:39 .git/
-rw-r--r-- 1 501  563 Apr 16 20:39 index.html
-rw-r--r-- 1 501 3316 Apr 16 20:39 oneko.gif
-rw-r--r-- 1 501 5073 Apr 16 20:39 oneko.js
</pre>

<br />

Flag: `texsaw{0h_n0_my_g1t_15_3xp053d!}`

<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
