---
title: "peactf 2019 Writeup"
date: 2019-08-01T19:00:00+09:00
lastmod: 2019-09-15T19:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://2019.peactf.com/problems](https://2019.peactf.com/problems)
<br /><br />
前回のcybricsに引き続き、風邪が長引いていたのと、アクセスができない時があったりして、簡単なやつをちょこっと解いて終了。
<br /><br />
いちおう、やった記録として残しておきます。



<br /><br />
# Broken Keyboard
- - -
## Challenge
> Help! My keyboard only types numbers!
<br /><br />
Ciphertext: 112 101 97 67 84 70 123 52 115 99 49 49 105 115 99 48 48 108 125

<br />
## Solution
普通に1個1個変換したら面白くないので、ワンライナーで挑戦。
```python
$ python -c 'print("".join([chr(int(x)) for x in "112 101 97 67 84 70 123 52 115 99 49 49 105 115 99 48 48 108 125".split()]))'
```
flag: `peaCTF{4sc11isc00l}`



<br /><br />
<br /><br />
# Breakfast
- - -
## Challenge
> Mmm I ate some nice <b>bacon</b> and eggs this morning. Find out what else I had for an easy flag. Don’t forget to capitalize CTF!
<br /><br />
Ciphertext: 011100010000000000101001000101{00100001100011010100000000010100101010100010010001}

<br />
## Solution
他の問題を解いていると、フラグのフォーマットが peaCTF{xxxx} なのがわかるので、011100010000000000101001000101の部分がpeaCTFだとして6等分すると、

01110 &nbsp; p<br />
00100 &nbsp; e<br />
00000 &nbsp; a<br />
00010 &nbsp; C<br />
10010 &nbsp; T<br />
00101 &nbsp; F<br />
<br />
なので、こんな感じだと予想。<br />
00000 &nbsp; a<br />
00001 &nbsp; b<br />
00010 &nbsp; c<br />
00011 &nbsp; d<br />
00100 &nbsp; e<br />
00101 &nbsp; f<br />
00110 &nbsp; g<br />
00111 &nbsp; h<br />
01000 &nbsp; i<br />
01001 &nbsp; j<br />
01010 &nbsp; l （kはどこへ行った？）<br />
01011 &nbsp; m<br />
01100 &nbsp; n<br />
01101 &nbsp; o<br />
01110 &nbsp; p （これがpなのは確か）<br />
01111 &nbsp; q<br />
10000 &nbsp; r<br />
10001 &nbsp; s<br />
10010 &nbsp; t （これがTなのは確か）<br />
10011 &nbsp; u<br />
10100 &nbsp; v （結果、なぜかこれはvではなくて、wだった）<br />

<br />
ところどころナゾだけど、{}の中身は、<br />
00100 &nbsp; e<br />
00110 &nbsp; g<br />
00110 &nbsp; g<br />
10100 &nbsp; w<br />
00000 &nbsp; a<br />
00101 &nbsp; f<br />
00101 &nbsp; f<br />
01010 &nbsp; l<br />
00100 &nbsp; e<br />
10001 &nbsp; s<br />

<br />
flag: `peaCTF{eggwaffles}`

<br />
＃問題文の中で、<b>bacon</b>の部分だけ太字になっているのがナゾ。なんかのヒントなの？ スライスするとか言うことかな。。


<br /><br />
(2019/09/15 追記)

復習してたら、これは[Bacon's cipher](https://en.wikipedia.org/wiki/Bacon%27s_cipher)っていうらしいですね。知らなくても解けたけど。



<br /><br />
<br /><br />
# Hide and Seek
- - -
## Challenge
> Try to find to the flag file located somewhere in the folders located in: /problems/hide-and-seek_16_d386ec3561e6180b95a47434c62a78be

<br />
## Solution
これはシェルサーバに入って解く問題。
<pre>
$ find . -name *flag*
$ cat ./926c7fc91293cbdcbb5a99c561782682/80dee25b24cac38a8a7f7214c732974c/d9f58f5e381818ca33525a98ca33dfbe/f17007910d11d07404b23bb62d7a7ee1/9c991628ac6f756dd67cfa3b98c42cf6/47c8055d6bd5102bd363fc752227844a/1d4c5a6d7ce032905f80ccd47b1c08df/f4fd3d96096de5ed863de07324e1bf57/flag.txt
</pre>
flag: `flag{peactf_linux_is_fun_f4ca42f3f34f4674d511eade889d74f7}`



<br /><br />
<br /><br />
# School
- - -
## Challenge
> My regular teacher was out sick so we had a substitute today.
<br /><br />
Alphabet: ​WCGPSUHRAQYKFDLZOJNXMVEBTI<br />
zswGXU{ljwdhsqmags}

<br />
## Solution
単一換字式暗号 (monoalphabetic substitution ciphers)です。
```Perl
#!/usr/bin/perl
use strict;
use warnings;
use utf8;

use open qw/:utf8/;

open(my $F, "<:utf8", 'School.txt') or die;
my $text;
while (my $l = <$F>)
{
    #$l =~ s/[\r\n]+/ /g;
    $text .= $l;
}
close($F);

$text =~ y/wcgpsuhraqykfdlzojnxmvebti/abcdefghijklmnopqrstuvwxyz/;
print $text;
print "\n---------\n";
```

得られた結果：peaGXU{orangejuice} </br></br>
上記のPerlスクリプトは小文字だけ置き換えているので、GXUのところが書き換わってないだけです。

flag: `peaCTF{orangejuice}`



<br /><br />
<br /><br />
# Coffee Time
- - -
## Challenge
> Run this jar executable in a virtual machine and see what happens.

Attachments:

- coffeetime.jar

<br />
## Solution
実行しろというので、とりあえず実行。
<pre>
$ java coffeetime.jar 
Error: Could not find or load main class coffeetime.jar
Caused by: java.lang.ClassNotFoundException: coffeetime.jar
</pre>

<br />
よくわからんので、とりあえずコードでも見てみるかと思って、jadでデコンパイル。
<br /><br />
そしたら、ソースコードの中にフラグがありました。なんじゃこりゃ。
<pre>
System.out.println("peaCTF{nice_cup_of_coffee}");
</pre>
flag: `peaCTF{nice_cup_of_coffee}`



<br /><br />
<br /><br />
# We are E.xtr
- - -
## Challenge
> 特に記載なし（添付ファイルのみ）

Attachments:

- E.xtr

<br />
## Solution
<pre>
$ file E.xtr 
E.xtr: data

$ xxd E.xtr | head -n 1
00000000: 8958 5452 0d0a 1a0a 0000 000d 4948 4452  .XTR........IHDR
</pre>

PNGファイルのファイルヘッダを直す系の問題。
<br /><br />
バイナリエディタで編集する。

<pre>
$ xxd E.xtr | head -n 1
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR

$ file E.xtr 
E.xtr: PNG image data, 1280 x 720, 8-bit colormap, interlaced
</pre>

flag: `peaCTF{read_banned_it}`



<br /><br />
<br /><br />
# The Wonderful Wizard
- - -
## Challenge
> 特に記載なし（添付ファイルのみ）

Attachments:

- TheWonderfulWizard.png

<br />
## Solution
「青い空を見上げればいつもそこに白い猫」で赤色ビット0抽出すると、数字が出てくるので、それをHex Decodeするだけ。<br />
<img src="https://captureamerica.github.io/writeups/img/TheWonderfulWizard_Neko.PNG" alt="TheWonderfulWizard_Neko.PNG">

<pre>
66 6c 61 67 7b 70 65 61 63 74 66
5f 77 68 65 72 65 5f 74 68 65 5f
77 69 6e 64 5f 62 6c 6f 77 73 7d
</pre>

flag: `flag{peactf_where_the_wind_blows}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

