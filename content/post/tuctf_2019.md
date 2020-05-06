---
title: "TUCTF 2019 Writeup"
date: 2019-12-03T13:00:00+09:00
lastmod: 2019-12-25T16:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Ftuctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2019/12/25 - 復習しました)

URL: [https://tuctf.com/](https://tuctf.com/)

もうサイトにアクセスできないようなので、最終順位がわかりません。

以下、チャレンジ一覧と解いた問題です。

<img src="https://captureamerica.github.io/writeups/img/tuctf_chal1.png" alt="tuctf_chal1.png">

<img src="https://captureamerica.github.io/writeups/img/tuctf_chal2.png" alt="tuctf_chal2.png">
<br /><br />



<br /><br />
## [Misc]: Onions
- - -
### Challenge
> Ogres are like files -- they have layers!

Attachment:

- shrek.jpg


<br />
### Solution
シュレックのJPEG画像です。著作権は大丈夫なんでしょうか。

foremostではなにも取れなかったので、binwalkから始めます。

binwalk -e でもextractされなかったので、--ddを使いました。

<pre>
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
</pre>

<br />
Flag: `TUCTF{F1L3S4R3L1K30N10NSTH3YH4V3L4Y3RS}`


<br /><br />
<br /><br />
## [Misc]: Super Secret
- - -
### Challenge
> Something's blocking my flag from this file...

Attachment:

- document.odt


<br />
### Solution
<pre>
$ file document.odt
document.odt: OpenDocument Text
</pre>

使っている Windows PC に OpenOffice がインストール済みだったので、マクロを無効な状態で開いてみました。

マクロの中身は以下で確認できました。

OpenOffice > ツール > マクロ > マクロの管理 > OpenOffice.org Basic > 編集

<pre>
REM  *****  BASIC  *****

Sub Main
TUCTF{ST0P_TRUST1NG_M4CR0S_FR0M_4N_UNKN0WN_S0URC3}
End Sub
</pre>

<br />
Flag: `TUCTF{ST0P_TRUST1NG_M4CR0S_FR0M_4N_UNKN0WN_S0URC3}`



<br><br>
<br><br>
## [Misc]: RNGeesus
- - -
### Challenge
> RNGeesus has a secret technique, can you guess it?
<br><br>
nc chal.tuctf.com 30300


<br />
### Solution
まずは繋げてみます。2回繋げて、毎回同じ問題であることを確認しました。

また、適当に答えを入れると、ググれ！というヒントがもらえました。

<pre>
$ nc chal.tuctf.com 30300
Good afternoon, my children...

I've overheard the prayers to RNGeesus, but he's got a terrible secret...

He's just using rand() calls on his new Mac!

If you can guess his next number, I'll even give you a flag.

You ready to guess his next move? Here's his current random number.

RNGesus gives the following number: 520932930


$ nc chal.tuctf.com 30300
Good afternoon, my children...

I've overheard the prayers to RNGeesus, but he's got a terrible secret...

He's just using rand() calls on his new Mac!

If you can guess his next number, I'll even give you a flag.

You ready to guess his next move? Here's his current random number.

RNGesus gives the following number: 520932930

520932931
Easy! Alright, what's his next move?
Hmm, that wasn't right. Try some more searching. There's source code out there somewhere! Better luck next time...
</pre>

"520932930"でググると、いくつかサイトがヒットしました。

シード値(0)でrand()を実行した際の最初の結果が"520932930"で、次は"28925691"のようです。

<br />
Flag: `TUCTF{D0NT_1NS3CUR3LY_S33D_Y0UR_LCGS}`



<br><br>
<br><br>
## [Mega]: Cup of Joe: The Server
- - -
### Challenge
> On the first leg of the journey, I was looking at all the life, there were plants and hills and rocks and things, there was java and mugs and caffeine.
<br><br>
chal.tuctf.com:32000


<br />
### Solution
上記のウェブサイトにアクセスすると、以下にリダイレクトされます。

http://chal.tuctf.com:32000/coffeepot?

<br />
ソースを見てみます。
```html
<html>
        <head>
                <title>April Fools</title>
                <style>
			body {
    				background-color: #805500;
    				font-family: papyrus;
			}
			.center {
  				display: block;
  				margin-left: auto;
  				margin-right: auto;
  				width: 50%;
			}
		</style>
        </head>
        <body>
		<h1 align=center>I'm Craving Some Coffee</h1>
  		<hr>
		<p> You know that feeling when you just want something from a website and you haven't gotten it yet? Well look no further. At TUCTF, we have a problem we request you solve: we want coffee.
		<br><br>
		<img src=https://tinyurl.com/418isFake class=center>		
		<p> But don't worry. Don't get fooled by those crazy tea loving children! Soon enough you will prove to us you can get coffee -- and a flag.
		<!-- Pssst this is the actual admin. We were lying. We do want tea. Can you make us some? I know we have pretty teapots somewhere -->
		<br>
		<form action="/coffeepot" method="BREW">

			<Input type="Submit" value="Give Us Coffee"/>

		</form>
	</body>
</html>
```

これは、エイプリルフールのジョークRFC（RFC 2324 - Hyper Text Coffee Pot Control Protocol）ですね。

RFCの存在は知ってましたけど、実際に使うのは今回が初めてです。

<br /><br />
普通にGETを送ると、カフェインが足りないと言って怒られます。
<pre>
GET /coffeepot? HTTP/1.1
Host: chal.tuctf.com:32000
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
DNT: 1
User-Agent: Mozilla/5.0 (Windows NT 6.2; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: ja,en-US;q=0.9,en;q=0.8

HTCPCP/1.0 404 Not enough caffeine here :(
 Server: JavaServer
 Content-Length: 0
 Content-Type: null
</pre>


<br /><br />
いろいろと試した結果、teapotに対してリクエストを送るのが正解でした。
<pre>
$ curl -X BREW http://chal.tuctf.com:32000/teapot
HTCPCP/1.0 418 I'm a teapot. Go to /broken.zip
 Server: JavaServer
 Content-Length: 0
 Content-Type: Short and stout

$ curl -X BREW http://chal.tuctf.com:32000/broken.zip
HTCPCP/1.0 200 Success!
 Server: JavaServer
 Content-Length: 6965888
 Content-Type: application/octet-stream

Warning: Binary output can mess up your terminal. Use "--output -" to tell
Warning: curl to output it to your terminal anyway, or consider "--output
Warning: <FILE>" to save to a file.

$ curl -X BREW http://chal.tuctf.com:32000/broken.zip --output broken.zip
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 6802k    0 6802k    0     0   771k      0 --:--:--  0:00:08 --:--:-- 1123k

$ file broken.zip
broken.zip: data

$ unzip broken.zip
Archive:  broken.zip
warning [broken.zip]:  110 extra bytes at beginning or within zipfile
  (attempting to process anyway)
   creating: broken/
  inflating: broken/broken.img
  inflating: broken/flag.txt
</pre>

broken.zip は fileコマンドでZipとして認識されませんが、unzipコマンドで普通に解凍できました。

<br />
Flag: `TUCTF{d0_y0u_cr4v3_th3_418}`



<br><br>
<br><br>
## [Mega]: Broken
- - -
### Challenge
> Sometimes things that are broken have been broken on purpose.


<br />
### Solution
添付ファイルがありません。どうやら、先程の問題で取れた broken.img を使うようです。

フラグはstringsで見つかりました。

Flag: `TUCTF{D1S4ST3R_R3C0V3RY}`



<br><br>
<br><br>
## [Crypto]: Something in Common
- - -
### Challenge
> We've managed to gather the details in the given file. We need a professional cryptographer to decipher the message. Are you up to the task?

Attachment:

- rsa_details.txt

以下が中身です。<br />
n = 5196832088920565976847626600109930685983685377698793940303688567224093844213838345196177721067370218315332090523532228920532139397652718602647376176214689

e1 = 15

e2 = 13

c1 = 2042084937526293083328581576825435106672034183860987592520636048680382212041801675344422421233222921527377650749831658168085014081281116990629250092000069

c2 = 199621218068987060560259773620211396108271911964032609729865342591708524675430090445150449567825472793342358513366241310112450278540477486174011171344408


<br />
### Solution
"rsa ctf c1 c2 e1 e2"とか、"Common Modulus Attack" をキーワードにでググるのがコツです。

過去のCTF (Square CTF 2018) でほぼ同じ問題があり、Writeupが見つかります。

他の方が書いたWriteupそのままになっちゃうので、ここにはコードは載せません。

<pre>
$ python rsa_common_solve.py
TUCTF{Y0U_SH0ULDNT_R3US3_TH3_M0DULUS}
</pre>

<br />
Flag: `TUCTF{Y0U_SH0ULDNT_R3US3_TH3_M0DULUS}`



<br><br>
<br><br>
## [Crypto]: Crypto Infinite
- - -
### Challenge
> Crypto has been around forever. Does this challenge go on forever? Let us know in the comments below or find the flag to prove us wrong.
<br><br>
nc chal.tuctf.com 30102


<br />
### Unsolved
これは解けませんでした。。。

<b>無駄に</b>レベルが分かれていて、Level 0 ~ 4 は全く同じコードで解けたので、そこまでを載せておきます。Level 5以降は不明です。


<br /><br />
まずncでアクセスしてみます。
<pre>
$ nc chal.tuctf.com 30102

Welcome to the future.

Can you discover infinity?



Level 0

Give me text:
a
A encrypted is _|

Decrypt .-| |.- [. .-| |.- |- [] > _| |.- .<
Too slow!
</pre>

<br />
次は、AからZまで入れてみました。

<pre>
$ nc chal.tuctf.com 30102

Welcome to the future.

Can you discover infinity?



Level 0

Give me text:

ABCDEFGHIJKLMNOPQRSTUVWXYZ encrypted is _| |_| |_ ] [] [ -| |-| |- ._| |._| |._ .] [.] [. .-| |.-| |.- v > < ^ .v .> .< .^

Decrypt .] |- [.] < v |_ < |._ []
Too slow!
</pre>

<br />
Encryptedの部分をスペース区切りとみなすと26パターンあり、アルファベットの各文字に対応しているようです。


以下、が書いたスクリプトです。（たぶん途中のやつ。この後、更に編集したのが別PCに残っているので、後で差し替えておきます。） 

(2020/05/06 - 差し替えたんだっけ？ もう覚えてないや。。。)

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from pwn import *

char = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
char_list = list(char)

host, port = 'chal.tuctf.com', 30102
s = remote(host, port)
s.recvuntil('Give me text:\n')
s.sendline(char)
s.recvuntil("encrypted is ")
enc = s.recvuntil('\n')[:-1]
enc_list = enc.split(" ")

while True:
    msg = s.recvregex('Decrypt |Congratulations')
    if "Congratulations" in msg:
        s.recvuntil('Give me text:\n')
        s.sendline(char)
        s.recvuntil("encrypted is ")
        enc = s.recvuntil('\n')[:-1]
        enc_list = enc.split(" ")

    cipher = s.recvuntil('\n')[:-1]
    cipher_list = cipher.split(" ")
    print cipher_list

    str = ""
    for c in cipher_list:
        for i in range(len(char_list)):
            if c == enc_list[i]:
                str += char_list[i]
                break
    print(str)
    s.sendline(str)
    print s.recvline()
    print s.recvline()

print s.recvall()
```


<br /><br />
<br /><br />

## [Reversing]: faker
- - -
### Challenge
> One of these things is not like the other. Can you uncover the flag?
<br><br>
Hint: Is there anything hidden inside the binary?

Attachment:

- faker (ELF 64-bit)


<br />
### Solution
Ghidraでコードを確認します。printFlag()という関数と、それを呼んでいるA(), B(), C()関数が見つかります。

```C
void printFlag(char *param_1)

{
  char *__dest;
  size_t sVar1;
  int local_30;
  
  __dest = (char *)malloc(0x40);
  memset(__dest,0,0x40);
  strcpy(__dest,param_1);
  sVar1 = strlen(__dest);
  local_30 = 0;
  while (local_30 < (int)sVar1) {
    __dest[local_30] = (char)((int)((((int)__dest[local_30] ^ 0xfU) - 0x1d) * 8) % 0x5f) + ' ';
    local_30 = local_30 + 1;
  }
  puts(__dest);
  return;
}

void A(void)

{
  printFlag("\\PJ\\fCaq(Lw|)$Tw$Tw@wb@ELwbY@hk");
  return;
}

void B(void)

{
  printFlag("\\PJ\\fCTq00;waq|w)L0LwL$|)L0k");
  return;
}

void C(void)

{
  printFlag("\\PJ\\fChqqZw|0;w2l|wELL(wYqqE$ahk");
  return;
}
```

<br /><br />
main()をくっつけて、コンパイルして実行してみます。

```C
#include <stdio.h>

void printFlag(char *param_1)

{
  char *__dest;
  size_t sVar1;
  int local_30;

  __dest = (char *)malloc(0x40);
  memset(__dest,0,0x40);
  strcpy(__dest,param_1);
  sVar1 = strlen(__dest);
  local_30 = 0;
  while (local_30 < (int)sVar1) {
    __dest[local_30] = (char)((int)((((int)__dest[local_30] ^ 0xfU) - 0x1d) * 8) % 0x5f) + ' ';
    local_30 = local_30 + 1;
  }
  puts(__dest);
  return;
}

void A(void)

{
  printFlag("\\PJ\\fCaq(Lw|)$Tw$Tw@wb@ELwbY@hk");
  return;
}

void B(void)

{
  printFlag("\\PJ\\fCTq00;waq|w)L0LwL$|)L0k");
  return;
}

void C(void)

{
  printFlag("\\PJ\\fChqqZw|0;w2l|wELL(wYqqE$ahk");
  return;
}

int main()
{
    A();
    B();
    C();
}
```

<br /><br />
以下が結果です。どれもFake Flagっぽいです。

<pre>
$ ./faker_solve.o
TUCTF{n0p3_7h15_15_4_f4k3_fl46}
TUCTF{50rry_n07_h3r3_317h3r}
TUCTF{600d_7ry_bu7_k33p_l00k1n6}
</pre>

<br /><br />
GhidraでStringsサーチしたら、似たようなものがもう1個見つかりました。

<img src="https://captureamerica.github.io/writeups/img/faker_strings.PNG" alt="faker_strings.PNG">

同じようにコンパイルして実行するとフラグが得られます。

<br />
Flag: `TUCTF{7h3r35_4lw4y5_m0r3_70_4_b1n4ry_7h4n_m3375_7h3_d3bu663r}`


<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後に行った復習です。（他の方のWriteupとか参照してます。）

<br /><br />
## [Reversing]: object
- - -
### Challenge
> We've recovered this file but can't make much of it.
<br /><br />
Hint: Do you see a procedural way to recover the password?

Attachment:

- run.o (ELF 64-bit)


<br />
### Solution
Ghidraでソースを確認します。

```C
void main(void)

{
  void *__s;
  
  setvbuf(stdout,(char *)0x0,2,0x14);
  setvbuf(stdin,(char *)0x0,2,0x14);
  __s = malloc(0x40);
  memset(__s,0,0x40);
  printf("Enter password:\n> ");
  __isoc99_scanf(&DAT_00100218,__s);
  printf("pass: %s\n",__s);
  checkPassword(__s);
  return;
}


undefined8 checkPassword(char *param_1)

{
  size_t sVar1;
  undefined8 uVar2;
  uint local_10;
  
  sVar1 = strlen(param_1);
  if ((int)sVar1 == passlen) {
    local_10 = 0;
    while ((int)local_10 < passlen) {
      if ((byte)(~(param_1[(int)local_10] << 1) ^ 0xaaU) != password[(int)local_10]) {
        printf("Incorrect password\nError at character: %d\n",(ulong)local_10);
        return 1;
      }
      local_10 = local_10 + 1;
    }
    puts("Correct Password!");
    uVar2 = 0;
  }
  else {
    puts("Close, but no flag");
    uVar2 = 1;
  }
  return uVar2;
}
```

入力したパスワードを1文字ずつ処理しています。（whileのすぐ下のところ）

左シフトしたものを、反転 (`XOR 0xFF`) して、さらに `XOR 0xAA` したものを、比較用データと比較してます。

比較用データもGhidraで見つかりました。

<img src="https://captureamerica.github.io/writeups/img/tuctf_object_password.png" alt="tuctf_object_password.png">

`XOR 0xAA` の `XOR 0xFF` は `XOR 0x55` なので、この比較用データを `XOR 0x55` してから右シフトします。

<pre>
python3 -c 'print(*[chr(((int(x,16))^0x55)>>1) for x in "FD FF D3 FD D9 A3 93 35 89 39 B1 3D 3B BF 8D 3D 3B 37 35 89 3F EB 35 89 EB 91 B1 33 3D 83 37 89 39 EB 3B 85 37 3F EB 99 8D 3D 39 AF".split()])' | tr -d " "
TUCTF{c0n6r47ul4710n5_0n_br34k1n6_7h15_fl46}
</pre>

<br />
Flag: `TUCTF{c0n6r47ul4710n5_0n_br34k1n6_7h15_fl46}`

<br />
{{% admonition tip "Tips" %}}
python3にて、listに*を付けることで、要素のみを空白区切りで出力することができます。
{{% /admonition %}}



<br /><br />
## [Reversing]: core
- - -
### Challenge
> We were able to recover a few files we need analyzed. Think you can get anything outta these?
<br /><br />
Hint: How is the data manipulated before the crash? Do you know any of the characters?

Attachment:

- run.c
- core

run.cの中身：

```C
#include <stdio.h>  // prints
#include <stdlib.h> // malloc
#include <string.h> // strcmp
#include <unistd.h> // read
#include <fcntl.h>  // open
#include <unistd.h> // close
#include <time.h>   // time

#define FLAG_LEN 64
char flag[FLAG_LEN];

void xor(char *str, int len) {
	for (int i = 0; i < len; i++) {
		str[i] = str[i] ^ 1;
	}
}

int main() {
    setvbuf(stdout, NULL, _IONBF, 20);
    setvbuf(stdin, NULL, _IONBF, 20);

	// Read the flag
	memset(flag, 0, FLAG_LEN);
	printf("> ");
	int len = read(0, flag, FLAG_LEN);

	xor(flag, len);

	char buf[32];
	read(0, buf, 128);

    return 0;
}
```


<br />
### Solution
gdbでcore解析しようとして、分からなかったやつです。

run.c をベースに、フラグの一部 `TUCTF` をXOR ^ 1したものを、coreに対してstringsサーチするだけでした。

<br />
Flag: `TUCTF{c0r3_dump?_N3v3r_h34rd_0f_y0u}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />