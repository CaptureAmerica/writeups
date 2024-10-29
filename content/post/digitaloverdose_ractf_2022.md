---
title: "Digital Overdose 2022 Autumn CTF Writeup"
date: 2022-11-21T22:00:00+09:00
lastmod: 2022-11-21T22:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdigitaloverdose_ractf_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://digitaloverdose.ractf.cloud/campaign](https://digitaloverdose.ractf.cloud/campaign)
<br /><br />

200点を獲得し、チームとしては244位、個人では314位でした。

<br><br>

チーム順位：

<img src="https://captureamerica.github.io/writeups/img/ractf_2022_Score1.png" alt="ractf_2022_Score1.png">

<br><br>

個人順位：

<img src="https://captureamerica.github.io/writeups/img/ractf_2022_Score2.png" alt="ractf_2022_Score2.png">


<br />
とりあえず、記念に一個だけ、writeupを残しておきます。


<br /><br />
## [Stego]: It's hidden (100 points)
- - -
### Challenge
> It is indeed hidden.
<br /><br />
(Yes, there are no files)


<br />

### Solution
このチャレンジ文の中にはデータが付いていませんが、カテゴリトップのところにあるデータが関係しているのは明らかです。

<img src="https://captureamerica.github.io/writeups/img/ractf_2022_itshidden.png" alt="ractf_2022_itshidden.png">


<br />

1. これの白い部分だけをコピペしてテキストファイルとして保存します。

<br />

2. 次に、xxd コマンドで3バイト毎に区切ります。
<pre>
$ xxd -ps -c 3 aaa.txt
e2808b
e2808b
e2808b
e2808b
:
(snip)
:
</pre>

<br />

3. uniq コマンドで出現回数を数えます。

<pre>
$ xxd -ps -c 3 aaa.txt | uniq -c
  68 e2808b
   1 e29688
  79 e2808b
   1 e29688
  67 e2808b
   1 e29688
  84 e2808b
   1 e29688
:
(snip)
:
</pre>

<br />

4. e2808b の出現回数の数だけ取り出します。

<pre>
$ xxd -ps -c 3 aaa.txt | uniq -c | grep e2808b | cut -c -5 | tr -d " " | tr "\n" " " ; echo
68 79 67 84 70 123 85 78 51 88 80 51 67 84 51 68 95 69 77 80 84 121 78 51 83 36 125
</pre>

<br />

5. 文字へ変換。
<pre>
$ python -c 'print("".join([chr(int(x)) for x in "68 79 67 84 70 123 85 78 51 88 80 51 67 84 51 68 95 69 77 80 84 121 78 51 83 36 125".split()]))'
DOCTF{UN3XP3CT3D_EMPTyN3S$}
</pre>

<br /><br />


Flag: `DOCTF{UN3XP3CT3D_EMPTyN3S$}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
