---
title: "watevrCTF Writeup"
date: 2019-12-15T14:00:00+09:00
lastmod: 2019-12-15T14:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/watevrctf_2019/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fwatevrctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

URL: [https://ctf.watevr.xyz/challenges](https://ctf.watevr.xyz/challenges)
<br /><br />
クリスマスシーズンでCTFイベントが並行して開催されている中、こちらのCTFは問題数も多く、よくわからんのが多くてほとんど手つかずです。
<br /><br />
3問しか解いてないです。302位でした。

<img src="https://captureamerica.github.io/writeups/img/watevrCTF_2019_Score.png" alt="watevrCTF_2019_Score.png">




<br /><br />
## [Forensic]: Evil Cuteness
- - -
### Challenge
> Omg, look at that cute kitty! It's so cute I can't take my eyes off it! Wait, where did my flag go?

Attachment:

- kitty.jpg


<br />
### Solution
$ binwalk --dd='.*' kitty.jpg

<br />
Flag: `watevr{7h475_4c7u4lly_r34lly_cu73_7h0u6h}`


<br /><br />
<br /><br />
## [Web]: Cookie Store
- - -
### Challenge
> Welcome to my cookie store!
<br /><br />
http://13.48.71.231:50000


<br />
### Solution
ショップのページで100円で変えるクッキーがあるけど、手持ちは50円でした。

取れたクッキー。
<pre>
$ echo eyJtb25leSI6IDUwLCAiaGlzdG9yeSI6IFtdfQ== | base64 -d
{"money": 50, "history": []}
</pre>

<br />
クッキーを編集してお金を増やして100円クッキーを購入。
<pre>
$ echo "{\"money\": 200, \"history\": []}" | base64
eyJtb25leSI6IDIwMCwgImhpc3RvcnkiOiBbXX0K
</pre>

<br />
Flag: `watevr{b64_15_4_6r347_3ncryp710n_m37h0d}`




<br /><br />
<br /><br />
## [Reverse]: Timeout
- - -
### Challenge
> Stop, wait a minute!

Attachment:

- timeout (ELF 64bit)



<br />
### Solution
Ghidraでソースを確認。generate()という関数の中で、フラグを生成しています。

```C
void generate(void)

{
  long in_FS_OFFSET;
  char local_b8;
  undefined local_b7;
  undefined local_b6;
  undefined local_b5;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  if (can_continue == 0x539) {
    local_78 = 0x23;
    local_77 = 0x3c;
:
(snip)
:
    local_b8 = 'w';
    local_b7 = 0x61;
    local_b6 = 0x74;
    local_b5 = 0x65;
    local_b4 = 0x76;
    local_b3 = 0x72;
    local_b2 = 0x7b;
    local_b1 = 0x33;
    local_b0 = 0x6e;
    local_af = 99;
    local_ae = 0x72;
    local_ad = 0x79;
    local_ac = 0x74;
    local_ab = 0x69;
    local_aa = 0x6f;
    local_a9 = 0x6e;
    local_a8 = 0x5f;
    local_a7 = 0x69;
    local_a6 = 0x73;
    local_a5 = 0x5f;
    local_a4 = 0x6f;
    local_a3 = 0x76;
    local_a2 = 0x65;
    local_a1 = 0x72;
    local_a0 = 0x72;
    local_9f = 0x61;
    local_9e = 0x74;
    local_9d = 0x65;
    local_9c = 100;
    local_9b = 0x5f;
    local_9a = 0x79;
    local_99 = 0x6f;
    local_98 = 0x75;
    local_97 = 0x74;
    local_96 = 0x75;
    local_95 = 0x62;
    local_94 = 0x65;
    local_93 = 0x2e;
    local_92 = 99;
    local_91 = 0x6f;
    local_90 = 0x6d;
    local_8f = 0x2f;
    local_8e = 0x77;
    local_8d = 0x61;
    local_8c = 0x74;
    local_8b = 99;
    local_8a = 0x68;
    local_89 = 0x3f;
    local_88 = 0x76;
    local_87 = 0x3d;
    local_86 = 0x4f;
    local_85 = 0x50;
    local_84 = 0x66;
    local_83 = 0x30;
    local_82 = 0x59;
    local_81 = 0x62;
    local_80 = 0x58;
    local_7f = 0x71;
    local_7e = 0x44;
    local_7d = 0x6d;
    local_7c = 0x30;
    local_7b = 0x7d;
    local_7a = 0;
    puts(&local_b8);
  }
```

長いので途中は省略してます。

local_b8の位置から表示しているので、それより前の値は無視してオッケーです。

local_b8 は 'w' だし、フラグ確定ですね。

'w' とか 10進数を16進数に変えておいて、pythonで文字に変換します。

<pre>
$ python -c 'print("".join([chr(int(x,16)) for x in "0x77,0x61,0x74,0x65,0x76,0x72,0x7b,0x33,0x6e,0x63,0x72,0x79,0x74,0x69,0x6f,0x6e,0x5f,0x69,0x73,0x5f,0x6f,0x76,0x65,0x72,0x72,0x61,0x74,0x65,0x64,0x5f,0x79,0x6f,0x75,0x74,0x75,0x62,0x65,0x2e,0x63,0x6f,0x6d,0x2f,0x77,0x61,0x74,0x63,0x68,0x3f,0x76,0x3d,0x4f,0x50,0x66,0x30,0x59,0x62,0x58,0x71,0x44,0x6d,0x30,0x7d".split(",")]))'
watevr{3ncrytion_is_overrated_youtube.com/watch?v=OPf0YbXqDm0}
</pre>

<br />
Flag: `watevr{3ncrytion_is_overrated_youtube.com/watch?v=OPf0YbXqDm0}`

<br />
（結局、タイマーの辺りは何もしなかったです)


<br /><br /><hr>
ここから以下は、解けてないやつです。。。


<br /><br />
<br /><br />
## [Misc]: Unspaellablle
- - -
### Challenge
> I think the author of this episode's script had a stroke or something... Or maybe it's just me?

Attachment:

- chall.txt

<br />
### (Unsolved)
スペルミスのある単語を見つけるだけなんでしょうけど、途中で飽きてしまいました。

<pre>
cawrds  (w)
thinag  (a)
grtabs  (t)
hies 	(e)
herre	(r)
holid 	(i)
fecmale (c)
laddear (a)
clinmbs (n)
Retirted(t)
Samuesls(s)
ppiece  (p)
teo 	(e)
Hammolnds (l)
sttart  (t)
thie 	(i)
havyen't (y)
:
</pre>

1個ずつ見ていくのが面倒だったので、途中からGoogle Docのスペルチェックを使って一気にスペル修正して、修正前と修正後のDiffを取ったら、うまい具合にフラグにならなかったです。



<br /><br />
<br /><br />
## [Misc]: Ancient Script
- - -
### Challenge
> Hmm, this looks like something my english teacher would give me, doesn't it? Too bad i forgot english... NOTE: the flag for this challenge is not in the standard watevr{} format. Submit it as is right from the challenge without the flag format.

Attachment:

- romeo (Ascii Text file)

<br />
### (Unsolved)
これは、Shakespeareというesolangです。

難しいのは、オンラインのインタープリターがなくて、コンパイラーを使ってコンパイルしないといけないのですが、どれを使ってもまともにコンパイルができないので降参しました。

試したやつ：<br />
http://shakespearelang.sf.net/download/spl-1.2.1.tar.gz (bison, flex 辺りが必要) <br />
https://github.com/zmbc/shakespearelang<br />
https://github.com/drsam94/Spl<br />







<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

