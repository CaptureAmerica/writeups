---
title: "X-MAS CTF Writeup"
date: 2019-12-22T19:00:00+09:00
lastmod: 2019-12-22T19:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://xmas.htsp.ro/home](https://xmas.htsp.ro/home)
<br /><br />
書いたブログが消えてしまい、半泣きしながら書き直してます。

<img src="https://captureamerica.github.io/writeups/img/xmas_Score.png" alt="xmas_Score.png">

<img src="https://captureamerica.github.io/writeups/img/xmas_Solved.png" alt="xmas_Solved.png">
<br /><br />


<br /><br />
## [Misc]: FUNction Plotter
- - -
### Challenge
> One of Santa's elves found this weird service on the internet. He doesn't like maths, so he asked you to check if there's anything hidden behind it.
<br /><br />
Remote Server: nc challs.xmas.htsp.ro 13005

<br />
### Solution
まずは、ncで繋いでみます。

<pre>
$ nc challs.xmas.htsp.ro 13005
Welcome to my guessing service!
Can you guess all 961 values?


f(30, 21)=1
Pretty close, but wrong!

f(24, 23)=1
Pretty close, but wrong!

f(13, 16)=1
Pretty close, but wrong!

f(20, 11)=1
Pretty close, but wrong!

f(14, 26)=1
Good!

f(20, 10)=1
Pretty close, but wrong!

f(26, 14)=1
Good!

f(8, 22)=1
Pretty close, but wrong!

f(12, 26)=1
Good!

:
:
</pre>

適当に1を入れたら、たまたまいくつかは正解しました。0を入れたときも同様でした。

2と3も試しましたが全部wrongになったので、どうやら0か1かのどちらかになるみたいです。

あと、f(x, y)のxとyに入る値はどちらも30くらいまでっぽかった（パターンは限られている）ので、繰り返しトライして961問題分のデータを集めたらよさそう、ということで以下のスクリプトを書きました。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import re
from pwn import *
host, port = 'challs.xmas.htsp.ro', 13005

if 1:
    answer_list = open('correct_answer.txt','r+').read().split("\n")
    bad_list = open('bad_answer.txt','r+').read().split("\n") 
    s = remote(host, port)
    for i in range(961):
        msg = s.recvuntil('=')
        q = re.search(r'f.*=', msg)
        cont_flag = 0
        for answer in answer_list:
            if q.group() in answer:
                s.sendline(answer[len(q.group()):])
                cont_flag = 1
                break
        if cont_flag == 1:
            continue

        for answer in bad_list:
            if q.group() in answer:
                s.sendline(str(int(answer[len(q.group()):])+1))
                cont_flag = 1
                break
        if cont_flag == 1:
            continue

        s.sendline('0')

    answer_list.close()
    bad_list.close()
    s.close()
```

<br />
ちょっと解説：<br />
<br />
\- correct_answer.txtには、正解したときの問題と答えを入れてます。<br />
<pre>
$ grep -i good guessing_service.txt -B1 | grep f > correct_answer.txt
</pre>
\- bad_answer.txtには、間違ったときの問題と答えを入れてます。<br />
<pre>
$ grep -i wrong guessing_service.txt -B1 | grep f > bad_answer.txt
</pre>
\- correct_answer.txtの中身を`f(x,y)=`でサーチして、見つかったらその後ろにある値を返してます。

\- bad_answer.txtの中身を`f(x,y)=`でサーチして、見つかったらその後ろにある値`+1`を返してます。（値の範囲がわからないときに書いたコードなので+1してますが、このチャレンジでは結果的に常に1になります。）

\- correct_answer.txtにも、bad_answer.txtにもマッチしないやつは、とりあえず0を返してます。

{{% admonition tip "Tips" %}}
全部プログラムでやらずに、コマンドで済ませられるところはコマンドで済ますと、プログラムも少し簡潔にできる
{{% /admonition %}}

<br />
4回くらいトライしたところで、全問正解し「Now what?」と言われました。

<pre>
:

f(14, 29)=0
Good!

f(14, 2)=1
Good!

Great! You did it! Now what?
</pre>

<br />
チャレンジ名がPlotだし、f(x,y)の座標に0か1かをセットしたらよさそう、かつデータの数が961 (31 x 31)で正方形になるので、QRコードになりそうな予感ですね。

HackCon'19 で学習したやつです。

https://captureamerica.github.io/writeups/post/hackcon_2019/#stego-gimp-it
{{% admonition tip "Tips" %}}
ファイルサイズが平方根とれる場合、正方形の画像にできる可能性あり！
{{% /admonition %}}
<br />

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from PIL import Image

f=open("great.txt","r").read().split("\n")
# The dimensions of QR is sqrt(961) i.e. 31
a=[]
for x in range(31):
    for y in range(31):
        s = "f(" + str(x) + ", " + str(y) + ")="
        print(s)
        val = ''
        for line in f:
            if s in line:
                val = line[len(s):]
                if val=='0':
	            a.append((255,255,255))
                else:
	            a.append((0,0,0))
                break
        # if val == '':
            # print("Something is wrong")
                 
im = Image.new("RGB",(31,31))
im.putdata(a)
im.save("QR.png")
```

<br />
<img src="https://captureamerica.github.io/writeups/img/xmas_QR.png" alt="xmas_QR.png">

<br />
Flag: `X-MAS{Th@t's_4_w31rD_fUnCt10n!!!_8082838205}`



<br /><br />
<br /><br />
## [Reversing]: Santa's crackme
- - -
### Challenge
> I bet you can't crack this!

Attachment:

- main (ELF 64bit)


<br />
### Solution
Ghidraでコードを確認します。

```C
undefined8 main(void)

{
  uint uVar1;
  byte local_7a;
  undefined local_79;
  byte local_78 [104];
  int local_10;
  uint local_c;
  
  printf("Enter your license key: ");
  __isoc99_scanf("%100s",local_78);
  local_c = 0;
  local_10 = 0;
  while (local_78[local_10] != 0) {
    local_7a = local_78[local_10] ^ 3;
    local_79 = 0;
    uVar1 = strcmp((char *)&local_7a,flag_matrix + (long)local_10 * 2);
    local_c = local_c | uVar1;
    local_10 = local_10 + 1;
  }
  if (local_c == 0) {
    puts("License key is correct");
  }
  else {
    puts("License key is incorrect");
  }
  return 0;
}
```
<br />
`flag_matrix`の辺りにフラグになりそうなデータがあります。

まず、テキストファイルに落としてから、数値だけ取り出します。ここで、"00"を取り除いているので、`(long)local_10 * 2)`の部分は無視してオッケーです。

あとは、Pythonで `XOR 3` しつつ、文字にするだけです。
<pre>
$ cat flag_matrix.txt | awk {'print $2'} | grep -v 00 | tr "\n" " "
5b 2e 4e 42 50 78 36 37 6d 34 37 5c 32 36 5c 61 37 67 5c 37 34 5c 6f 32 60 30 6d 36 30 5c 60 6b 30 60 68 32 6d 35 7e

$ python -c 'print("".join([chr(int(x,16)^3) for x in "5b 2e 4e 42 50 78 36 37 6d 34 37 5c 32 36 5c 61 37 67 5c 37 34 5c 6f 32 60 30 6d 36 30 5c 60 6b 30 60 68 32 6d 35 7e".split()]))'
X-MAS{54n74_15_b4d_47_l1c3n53_ch3ck1n6}
</pre>

<br />
Flag: `X-MAS{54n74_15_b4d_47_l1c3n53_ch3ck1n6}`


<br /><br />
<br /><br />
## [Misc]: Noise
- - -
### Challenge
> A beacon was broadcasting this message. Find out what it means...

Attachment:

- weird.wav

<br />
### Solution
Audacityで開いて再生したら、聞き覚えのあるノイズ(^^)でした。

picoCTF 2019で出てきたm00nwalk、<br />
Rooters CTF 2019で出てきたLuna-3<br />
https://captureamerica.github.io/writeups/post/rootersctf_2019/#forensics-luna--3<br />
と同じですね。

<br />
ちなみに、ステレオで右と左で別々のデータが入っていて、左側は英語のスロー再生です。<br />
右側だけ再生するようにしてます（以下、オレンジ色の四角のところ）<br />
<img src="https://captureamerica.github.io/writeups/img/Noise_Audacity.PNG" alt="Noise_Audacity.PNG">

キャプチャー結果<br />
<img src="https://captureamerica.github.io/writeups/img/Noise_RX-SSTV.PNG" alt="Noise_RX-SSTV.PNG">

少し見にくいので、ペイントで横に伸ばしました。<br />
<img src="https://captureamerica.github.io/writeups/img/Noise_Paint.PNG" alt="Noise_Paint.PNG">

<br />
Flag: `X-MAS{Wh1T3N0Is3}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
