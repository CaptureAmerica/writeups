---
title: "Security Fest 2019 Writeup"
date: 2019-05-23T00:00:00+08:00
lastmod: 2019-05-25T14:05:00+08:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---

# Signal
- - -
## Challenge
> There is no signal, everything is silent. Nothing is impossible.

Attachment:

- signal.pdf

<br />
開くと、こんな感じ。（以下は、スクリーンショットです。）
<img src="https://captureamerica.github.io/writeups/img/signal.png" alt="signal.png">


<br />
## Solution
回答だけじゃなくて、一応やったことも記録に残しておきます。

- pdf系の問題は、とりあえず、テキストエディタで開いてさらっと中身をチェックします。script使っているかどうかとか。
<br /><br />
- その後、何も考えず、いろいろツールにかけていきました。
 - pdf-parser.py
 - pdfid.py
 - pdfinfo
 - PdfAnalyzer.exe
 - PDFStreamDumper.exe

<br />
特に怪しい箇所がなんにもないので、残るはアスキーテキストに何かあると考えるしかないな、と思いました。

<br />
以下のコマンドの出力結果は抜粋です。
<pre>
$ pdftotext signal.pdf > signal.txt ; cat signal.txt
：
_-'... - .-. .- -. --. . .-.-.- -.- . -.-- -... ---`-_
_-' .- .-. -.. .-.-.- ... . -.-. ..-. .-.-.- -- .---- .`-_
_-'-.. .---- - ....- .-. -.-- ..--.- --. .-. ....- -.. ...-- -_
_-'..--.- ...-- -. -.-. .-. -.-- .--. - .---- ----- -. .-.-.- .-- -_
_-'.- .. - .-.-.- -.-. --- ..- .-.. -.. .-.-.- .-. .- -. -.. --- -`-_
:-------------------------------------------------------------------------:
`---._.-------------------------------------------------------------._.---'
</pre>

これを見た瞬間、なぜか「SOS」のモールス信号の音がアタマに流れました。<br />
あぁ、問題のタイトルSignal（信号）だしね。ビンゴの確信。

<br /><br />
あとは、オンラインで、`morse decode` をキーワードに検索してデコードサイトを見つけてデコードするだけなんですが、これがサイトによってクオリティが違ってて、エラーが出たり出なかったり、変換文字が違ったりするんです。

<br />
使った中で一番よかったのは、以下のサイトでした。
<br />
[https://www.dcode.fr/morse-code](https://www.dcode.fr/morse-code)

<br />
コツは、1行1行別々にデコードすることです。全部を1行にまとめてからデコードしようとすると、もともと改行があった場所でエラーが出たりします。

<br />
でもって、以下の文字列が取れるんですが、
STRANGE.KEYBOARD.SECF.M1L1T4RY_GR4D3T_3NCRYPT10N.WTAIT.COULD.RANDOM
<br /><br />
これそのままだと、incorrect flagで受け付けられなくて、いろんなパターンやってもことごとくダメだったんで何気に諦めかけたけど、結局以下の部分が最終フラグとして通りました。

<br />
**Flag:** m1l1t4ry_gr4d3_3ncrypt10n

<br />
＃まぁ、確かにアンダースコアで繋がっているし、その部分なんだろうけどさ。。。結構悩んだ

- - -
<br /><br />
<br /><br />