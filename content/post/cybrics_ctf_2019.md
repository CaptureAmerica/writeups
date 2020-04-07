---
title: "CyBRICS CTF 2019 Writeup"
date: 2019-07-25T12:00:00+09:00
lastmod: 2019-09-16T14:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/cybrics_ctf_2019/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fcybrics_ctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

URL: [https://cybrics.net/](https://cybrics.net/)
<br /><br />
結構難しかったのと、風邪をひいてしまったのもあって、2問だけです。（うち、1問は解けてないし。。）

<br /><br />
## [Network]: Sender
- - -
### Challenge
> We've intercepted this text off the wire of some conspirator, but we have no idea what to do with that.
<br /><br />
Get us their secret documents

Attachments:

- intercepted_text.txt

以下、intercepted_text.txtの中身。
<pre>
220 ugm.cybrics.net ESMTP Postfix (Ubuntu)
EHLO localhost
250-ugm.cybrics.net
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-AUTH PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
AUTH LOGIN
334 VXNlcm5hbWU6
ZmF3a2Vz
334 UGFzc3dvcmQ6
Q29tYmluNHQxb25YWFk=
235 2.7.0 Authentication successful
MAIL FROM: \<fawkes@ugm.cybrics.net>
250 2.1.0 Ok
RCPT TO: \<area51@af.mil>
250 2.1.5 Ok
DATA
354 End data with \<CR>\<LF>.\<CR>\<LF>
From: fawkes \<fawkes@ugm.cybrics.net>
To: Area51 \<area51@af.mil>
Subject: add - archive pw
Content-Type: text/plain; charset="iso-8859-1"
Content-Transfer-Encoding: quoted-printable
MIME-Version: 1.0

=62=74=77=2E=0A=0A=70=61=73=73=77=6F=72=64 =66=6F=72 =74=68=65 =61=72=63=
=68=69=76=65 =77=69=74=68 =66=6C=61=67=3A =63=72=61=63=6B=30=57=65=73=74=
=6F=6E=38=38=76=65=72=74=65=62=72=61=0A=0A=63=68=65=65=72=73=21=0A
.
250 2.0.0 Ok: queued as C4D593E8B6
QUIT
221 2.0.0 Bye
</pre>


<br />
### Solution
メールの本文はquoted-printableでエンコードされています。一旦別ファイルに落とした後、nkfでデコードします。

```
$ cat printable.txt
=62=74=77=2E=0A=0A=70=61=73=73=77=6F=72=64 =66=6F=72 =74=68=65 =61=72=63=
=68=69=76=65 =77=69=74=68 =66=6C=61=67=3A =63=72=61=63=6B=30=57=65=73=74=
=6F=6E=38=38=76=65=72=74=65=62=72=61=0A=0A=63=68=65=65=72=73=21=0A

$ cat printable.txt | nkf -WmQ
btw.

password for the archive with flag: crack0Weston88vertebra

cheers!
```

パスワード（crack0Weston88vertebra）がわかりました。


<br /><br />
上記に出てくるSMTPサーバでは、POP3も動いていて、メールの受信も可能です。ユーザ名とパスワードは以下の通り。

<pre>
$ echo ZmF3a2Vz | base64 -D ; echo
fawkes

$ echo Q29tYmluNHQxb25YWFk= | base64 -D ; echo
Combin4t1onXXY
</pre>

<br /><br />
メールの添付ファイルとして、secret_flag.zipが取れます。
あとは、上記のパスワードで解凍するだけですが、MacやWindowsの標準で付いているZip解凍だとなんかうまくいかなかったです。
<br /><br />
Explzhで解凍しました。
<br /><br />
Flag:
`cybrics{crack0Weston88vertebra}`


<br /><br />
<br /><br />
## [Forensic]: Tone
- - -
### Challenge
> Ha! Looks like this guy forgot to turn off his video stream and entered his password on his phone!
<br /><br />
[youtu.be/11k0n7TOYeM](youtu.be/11k0n7TOYeM)

<br />
### Solution
なんか変なURLですけど、アクセスするとYouTubeに飛びます。
<br /><br />
音がちっちゃいですが、よく聞くと電話のトーンの音（プッシュ音）が聞こえます。
<br /><br />
日本語で言うとトグル入力をしているようです。（英語では、Multi-tapと言うようです。）
<br /><br />
まずは、音がちっちゃいので、動画をWaveとしてダウンロードしてきて、AudacityでAmplifyしました。
<br /><br />
次にDTMF（Dual-Tone Multi-Frequency）信号を判別できるツールを使って数値にします。以下のツールを使いました。<br />
[https://www.vector.co.jp/soft/win95/net/se123346.html](https://www.vector.co.jp/soft/win95/net/se123346.html)
<br /><br />
このツール、音の検知はちゃんとできてたっぽいんですが、回数の検知がいまいちだったので、そこは手動でやりました。（「2」を三回押しているのに、一回しか検知しないとか）
<br /><br />
取れた値が以下です。<br />
222999227774442227777777733222777338866666255533355524
<br /><br />
以下のサイトでデコードしました。<br />
[https://www.dcode.fr/multitap-abc-cipher](https://www.dcode.fr/multitap-abc-cipher)
<br /><br />
それっぽいのが取れました。<br />
CYBRICSSECREUONALFLAG
<br />
英単語的に、CYBRICSSECRETIONALFLAG が正解かもです。
<br /><br />
ただし、いろんなバリエーションをFlagとしてSubmitしてみましたが、ことごとくダメでした。
<br /><br />
まぁ、それなりにできたので満足して終了。


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
