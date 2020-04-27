---
title: "Houseplant CTF 2020 Writeup"
date: 2020-04-27T15:00:00+09:00
lastmod: 2020-04-27T15:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2FhouseplantCTF_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://houseplant.riceteacatpanda.wtf/](https://houseplant.riceteacatpanda.wtf/)
<br /><br />
1554pointsで、389thでした。

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Score.png" alt="houseplantCTF_2020_Score.png"><br />

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_Solved.png" alt="houseplantCTF_2020_Solved.png">

<br />
解いた中で一番ポイントが高かった「Satan's Jigsaw」だけ、Writeupを書いておきます。

解法が気になるチャレンジがいくつかあるので、後日復習しようと思います。


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
こんどこそ！！ ジャーン！

<img src="https://captureamerica.github.io/writeups/img/houseplantCTF_2020_output_ok.png" alt="houseplantCTF_2020_output_ok.png">

<br />
Flag: `rtcp{d1d-you_d0_7his_by_h4nd?}`





<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

