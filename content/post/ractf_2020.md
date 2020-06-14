---
title: "Really Awesome CTF 2020 Writeup"
date: 2020-06-14T09:00:00+09:00
lastmod: 2020-06-14T09:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fractf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2020.ractf.co.uk/](https://2020.ractf.co.uk/)

これは順位が見れないやつだったかな。どっちみち、大した順位じゃありません。。

<img src="https://captureamerica.github.io/writeups/img/ractf_Score.png" alt="ractf_Score.png">



<br /><br />
1個だけWriteup残しておきます。

<br />

## [Misc]: Reading Between the Lines (300 points)
- - -
### Challenge
> We found a strange file knocking around, but I've got no idea who wrote it because the indentation's an absolute mess!

Attachment:

- main.c (中身は以下の通りです)

<img src="https://captureamerica.github.io/writeups/img/ractf_mainc.png" alt="ractf_mainc.png">


「This challenge should be a fun one!」というコメントがあって、なんか楽しいやつらしいです。


<br />
### Solution
Whitespace という esolang です。

References:<br />
https://ja.wikipedia.org/wiki/Whitespace<br />
https://en.wikipedia.org/wiki/Whitespace_(programming_language)<br />
https://esolangs.org/wiki/Whitespace<br />

<br />
チャレンジ名は「Reading Between the Lines」ですが、全部の行になんかしら入っているし、厳密には「Between the words」ですね。

とりあえず、目に見える文字をsedを使って消していきます。（ハイフンだけは横着してエディタで消しました。）

<img src="https://captureamerica.github.io/writeups/img/ractf_1.png" alt="ractf_1.png">


<br />
インタープリターはいろいろあるみたいですが、以下にある内のCで書かれたプログラムを使いました。

https://github.com/hostilefork/whitespacers/

<pre>
$ ./whitespace RACTF_2020.ws 
ractf{R34d1ngBetw33nChar4ct3r5}
</pre>

<br />

Flag: `ractf{R34d1ngBetw33nChar4ct3r5}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

