---
title: "Houseplant CTF 2020 Writeup"
date: 2020-04-27T15:00:00+09:00
lastmod: 2020-04-27T19:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fhouseplantctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2020/04/27 - 復習しました)

URL: [https://houseplant.riceteacatpanda.wtf/](https://houseplant.riceteacatpanda.wtf/)
<br /><br />
1554pointsで、389thでした。

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Score.png" alt="houseplantCTF_2020_Score.png"><br />

<br />
以下が解けたチャレンジです。

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Solved.png" alt="houseplantCTF_2020_Solved.png">

<br />
解いた中で一番ポイントが高かった「Satan's Jigsaw」だけ、Writeupを書いておきます。



<br /><br />
## [Misc]: Satan's Jigsaw (704 points)
- - -
### Challenge
> Oh no! I dropped my pixels on the floor and they're all muddled up! It's going to take me years to sort all 90,000 of these again :(
<br /><br />
Hint: long_to_bytes

Attachment:

- chall.7z

<br />
### Solution
とりあえずchall.7zを解凍しようとしたらなかなか解凍が終わらないのでよく見てみたらファイルが90,000個も入ってた！（問題文よく読んでなかった。。）

もしかして解凍しなくても解けるチャレンジなのでは？と思い、解凍処理を一旦キャンセル。

おそらく、300 x 300 のQRコードだと予想しました。

<br />
まず、ファイルリストを取ります。（ちなみに、"chall.7z" というファイル名は他のチャレンジでも使われていたので、リネームしてます。）

<pre>
$ 7z l Satans_Jigsaw_chall.7z > Satans_Jigsaw_chall_list.txt

$ grep jpg Satans_Jigsaw_chall_list.txt | awk '{print $4}' | sort | uniq
629
630
631
632
633
</pre>

サイズに注目した際に、5パターン (629 ~ 633) しかなかったので、色も5パターンなのかなと思い、以下のようなコードを書いてみました。

あと、座標についてはヒントの通りで、ファイル名からlong_to_bytes()を使って取れました。

```Python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import re
from Crypto.Util.number import long_to_bytes
from PIL import Image

img = Image.new("RGB", (300,300))

f = open("Satans_Jigsaw_chall_list.txt","r")
lines = f.readlines()
for line in lines:
    if "jpg" in line:
        fname = re.findall(r".*chall/(.*)\.jpg\n", line)[0]
        pos = (long_to_bytes(int(fname)))
        x = int(pos.split()[0])
        y = int(pos.split()[1])
        if " 629 " in line:
            img.putpixel((x, y), (150,150,150,0))        
        if " 630 " in line:
            img.putpixel((x, y), (0,0,0,0))        
        if " 631 " in line:
            img.putpixel((x, y), (50,50,50,0))        
        if " 632 " in line:
            img.putpixel((x, y), (100,100,100,0))
        if " 633 " in line:
            img.putpixel((x, y), (200,200,200,0))
f.close()
img.save("output.png")
```

<br />
はい、取れた画像がこれです！

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_output_ng.png" alt="houseplantCTF_2020_output_ng.png">

ガックシ。。。orz

どうやら90,000個のファイルを全部読み込まないとダメっぽいことがわかりました。


<br /><br />

7zファイルを解凍している間に、Pythonで同一ディレクトリ内のファイル一覧を取る方法とか調べたりして、最終的に以下のコードとなりました。

```Python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os
from Crypto.Util.number import long_to_bytes
from PIL import Image

path = '/home/captureamerica/Downloads/Satans_Jigsaw_chall/'
files = []
for filename in os.listdir(path):
    if os.path.isfile(os.path.join(path, filename)):
        files.append(filename)

img = Image.new("RGB", (300,300))

for f in files:
    if "jpg" in f:
        fname = f.split(".")[0]
        pos = (long_to_bytes(int(fname)))
        x = int(pos.split()[0])
        y = int(pos.split()[1])

        img2 = Image.open(f)
        img.putpixel((x, y), img2.getpixel((0,0)))

img.save("output.png")
img.show()
```


<br />
こんどこそ！！ イエーイ！

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_output_ok.png" alt="houseplantCTF_2020_output_ok.png">

<br />
Flag: `rtcp{d1d-you_d0_7his_by_h4nd?}`


<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。


<br /><br />
<br /><br />
## [Beginner]: Beginner 9
- - -
### Challenge
>Hope you've been paying attention! :D
<br /><br />
Remember to wrap the flag with rtcp{}
<br /><br />
Hint! we stan cyberchef in this household

Attachment:

- Beginner 10.txt

中身です。

<pre>
MmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMGEgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmQgMmQgMmQgMmQgMmQgMjAgMmUgMmQgMmQgMmQgMmQ=
</pre>


<br />
### Solution
問題文にある `Hope you've been paying attention!` からBeginner 1〜8のフラグがヒントになっているのかと思って読み返したりはしてたんですが、Solutionの組み合わせだとは思わなかった〜。

Cyberchefで一気にまとめてやると簡単みたいですね。

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Cyberchef.png" alt="houseplantCTF_2020_Cyberchef.png"><br />


<br />
ちなみに、ローカルで使っていたCyberchefが2年前のもので、A1Z26 Cipher Decodeが無かったので、これを機に最新版をダウンロードしました。

というか、Beginner 1〜8も全部Cyberchefで解けばよかったんですね。Atbash cipher (Beginner 7)はCyberchefを使ったんだけど。


<br />
Flag: `rtcp{nineornone}` （たぶん）

nine or none。。。だから、ファイル名が "Beginner 9.txt" じゃなくて "Beginner 10.txt" なのかな。




<br /><br />
<br /><br />
## [Misc]: Music Lab
- - -
### Challenge
>Do you like my song? ♪

Attachment:

- masterpiece.mid

<br />
### Solution
Audacityで開こうとしてドラッグ＆ドロップしたところ、以下のエラーのウィンドウが出て、エラーの内容を読まずに諦めたやつでした ^^;

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Midi1.png" alt="houseplantCTF_2020_Midi1.png">

というか、そんなエラーを出すくらいだったら、自動でImportしてよ！

<br />
言われた通りImportすると、以下のような結果が得られます。

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Midi2.png" alt="houseplantCTF_2020_Midi2.png">


<br />
Flag: `rtcp{M024rt_Would_b3_proud}` (たぶん)

<br />
凄いと思ったのは、それなりにホラー映画で使われそうな音楽になっているんですよね。








<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

