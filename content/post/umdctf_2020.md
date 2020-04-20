---
title: "UMDCTF 2020 Writeup"
date: 2020-04-20T08:00:00+09:00
lastmod: 2020-04-20T08:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fumdctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://umdctf.io/challenges](https://umdctf.io/challenges)
<br /><br />
直感的に解けそうと思ったやつだけやりました。

3問だけ解いて750points、147thでした。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2020_Solved.png" alt="umdctf_2020_Solved.png">


<br /><br />
## [Steganography]: Crash Confusion (200 points)
- - -
### Challenge
> There's something weird about this crash screen...

Attachment:

- crash.jpg

<br />
### Solution
「青い空を見上げればいつもそこに白い猫」でテキトーに見てたらBASE64エンコードされた文字列が出てきました。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2020_crash.png" alt="umdctf_2020_crash.png">

<br />

<pre>
$ echo VU1BQ1RGLXt0aHIzc2gwbGRfZXJyMHJ6fQ | base64 -d
UMACTF-{thr3sh0ld_err0rz}
</pre>

<br />
どこか読み間違えているのか、`UMACTF`になってしまったので、`UMDCTF`に直してSubmitしました。

Flag: `UMDCTF-{thr3sh0ld_err0rz}`





<br /><br />
<br /><br />
## [Forensics]: Sensitive (150)
- - -
### Challenge
> Not sure what to make of this...

Attachment:

- sensitive


<br />
### Solution
まず、どんなファイルなのかを調べます。

<pre>
$ file sensitive 
sensitive: data
</pre>

<br />
判別できなかったので、実際に中身を見てみると、元はPDFファイルで1バイト毎に空白文字が入っているようでした。

<pre>
% P D F - 1 . 5
:
</pre>

<br />

以下のコードで余分な空白文字を取り除きました。

（元々のファイルサイズが187836バイトなので、その半分の93918をb2のサイズとしています。）

```Python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

b1 = bytearray(open("sensitive","rb").read())
b2 = bytearray(93918)

i = 0
for x in b1:
    if (i % 2) == 0:
        b2[i//2] = x
    i += 1
    
open("output.pdf", "wb").write(bytearray(b2))
```

<br />

PDFをLibreで開くと、中央に薄っすらQRコードが見えます。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2020_libre_qr.png" alt="umdctf_2020_libre_qr.png">

<br />

この画像をファイルとして保存し、「青い空を見上げればいつもそこに白い猫」でテキトーに見てたら白黒がハッキリできました。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2020_sensitive_qr.png" alt="umdctf_2020_sensitive_qr.png">

<br />

Flag: `UMDCTF-{l0v3-me_s0me_h3x}`




<br /><br />
<br /><br />
## [Forensics]: Nefarious Bits (400)
- - -
### Challenge
> After being exposed to some solar radiation, it looks like some bits have turned bad. It is your job to figure out what they are trying to say.
<br /><br />
Note: do not attempt to communicate with or contact any of the IP addresses mentioned in the challenge. The challenge can and should be solved statically.

Attachment:

- attack.pcap


<br />
### Solution
WireSharkで各パケットを見ていくと、IPヘッダ内のフラグの値が0や1に変化しているのがわかります。

<pre>
$ tshark -r attack.pcap -Tfields -e ip.flags.rb | tr -d "\n" ; echo
01010101010011010100010001000011010101000100011000101101011110110011001101110110011010010110110001011111011000100011000101110100011100110101111101000000011100100011001101011111001101000110110001110111001101000111100101110011010111110011001101110110011010010011000101111101
</pre>

<br>
2進数を文字列に変換すると、フラグが得られます。

Flag: `UMDCTF-{3vil_b1ts_@r3_4lw4ys_3vi1}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

