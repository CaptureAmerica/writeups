---
title: "Deadface CTF 2024 Writeup"
date: 2024-10-29T14:00:00+09:00
lastmod: 2024-10-29T14:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdeadface_ctf_2024%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://deadface.ctfd.io/challenges](https://deadface.ctfd.io/challenges)
<br /><br />

Deadface CTF は、3回目の参加です。

[Deadface CTF 2021](https://captureamerica.github.io/writeups/post/deadface_ctf_2021/) (304位)

[Deadface CTF 2023](https://captureamerica.github.io/writeups/post/deadface_ctf_2023/) (387位)

<br><br>

今回は778点を獲得し、320位でした。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_Score1.png" alt="deadface_CTF_2024_Score1.png">

<br><br>

チャレンジのリストです。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall1.png" alt="deadface_CTF_2024_chall1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall2.png" alt="deadface_CTF_2024_chall2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall3.png" alt="deadface_CTF_2024_chall3.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall4.png" alt="deadface_CTF_2024_chall4.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall5.png" alt="deadface_CTF_2024_chall5.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall6.png" alt="deadface_CTF_2024_chall6.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall7.png" alt="deadface_CTF_2024_chall7.png">

<br><br>

以下、解いたチャレンジの一覧です。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_chall8.png" alt="deadface_CTF_2024_chall8.png">

<br><br>

最近、忙しくて、今回のWriteupも手抜きです。。


<br /><br />
## [Traffic Analysis]: Data Breach (25 points)
- - -
### Challenge
> We suspect an internal employee is leaking sensitive information, but the source remains unclear. We've captured network traffic for analysis. Your mission is to investigate the data and locate the hidden flag.
<br /><br />
Can we count on your expertise to track it down?

Attachment:

- databreach.pcapng

<br />

### Solution

grepで解けます。

<pre>
$ grep -a flag databreach.pcapng
Not-Suppose-To-Be-Here: flag{Information_disclosure_in_the_head}
:
</pre>


<br />

Flag: `flag{Information_disclosure_in_the_head}`


<br /><br />
<br /><br />
## [Traffic Analysis]: Wild Wild West (50 points)
- - -
### Challenge
> Woah, our network has been lighting up like the fourth of a July on steroids, I tell you HWHAT. Now, given that we had a user call in complaining about weird stuff happening, I think it is about time you cowboy up and figure out what is happening in our here network.

Attachment:

- wildwildwest.pcapng

<br />

### Solution

grepで解けます。

<pre>
$ grep -a flag wildwildwest.pcapng
:
�0���0����)0'�� 0flag{kerbrute_finding_users}
</pre>

<br>

Flag: `flag{kerbrute_finding_users}`



<br /><br />
<br /><br />
## [Traffic Analysis]: Unusual Exfiltration (250 points)
- - -
### Challenge
> Network logs from a machine DEADFACE compromised are being analyzed. It seems something was exfiltrated to their proxy machine on the local network before being sent back to their C2 server. Can you help figure out what data was stolen?

Attachment:

- unusualexfil.pcapng

<br />

### Solution

grepでは解けません。:)

<br>

tsharkでデータ部分を取り出します。余計なデータを取り除くために、"0a"でgrepしました。

<pre>
$ tshark -r unusualexfil.pcap -T fields -e data | grep "0a"
525252525252525252525252525252525252525252525252525252525252520a
525151515151515152525251515152525151525152515251515151515151520a
525152525252525152525252525152525252525152525251525252525251520a
525152515151525152515251525252515251525151525251525151515251520a
525152515151525152515152515252525251515252515251525151515251520a
525152515151525152515152525151515152525151515251525151515251520a
525152525252525152515252515151515151525152525251525252525251520a
525151515151515152515251525152515251525152515251515151515151520a
525252525252525252515252515151525152525252515252525252525252520a
525152515151515152525251525252515152515252515251515151515252520a
525152525251515251525252515152525151525251515151525151525151520a
525251515252515152525252525252525252525252525251515151515252520a
525252525151515251515252515152515251525152515252515151525252520a
525251515252525152515152515152525251515251525152525251515152520a
525152525152525252525151515151515152515151515251515152515151520a
525152515152515151525251515251515151525152515252525151515252520a
525252525251525252525151515151525152525252525152515151525152520a
525152525251525151515252525152515152515251525152525251525251520a
525152515252515251525252525252525151515151515151515152525151520a
525152515152515151525152515152525252515252525251525151525252520a
525152525152525251525252515152515251525251515252525151525151520a
525152525252515151525151525152525251515252515151515151525252520a
525252525252525252515251515251515152515152515252525152525151520a
525151515151515152525251525151515151515251515251525152515252520a
525152525252525152515151525151525152525151515252525152525252520a
525152515151525152515152525152515152515252515151515152515251520a
525152515151525152515151525251525151515252525252515251515252520a
525152515151525152515252515151515252525252515151525151525152520a
525152525252525152525152525152515251525251515252515251525152520a
525151515151515152515252515152525252525151525252525152515252520a
525252525252525252525252525252525252525252525252525252525252520a
</pre>

<br>

2値のデータ('R'と'Q')なんですが、"5"と"0a"を取り除いて、"1"と"2"のデータにします。

<pre>
$ tshark -r unusualexfil.pcap -T fields -e data | grep "0a" | tr -d "50a"
2222222222222222222222222222222
2111111122211122112121211111112
2122222122222122222122212222212
2121112121212221212112212111212
2121112121121222211221212111212
2121112121122111122111212111212
2122222121221111112122212222212
2111111121212121212121211111112
2222222221221112122221222222222
2121111122212221121221211111222
2122211212221122112211112112112
2211221122222222222222211111222
2222111211221121212121221112222
2211222121121122211212122211122
2122122222111111121111211121112
2121121112211211112121222111222
2222212222111112122222121112122
2122212111222121121212122212212
2121221212222222111111111122112
2121121112121122221222212112222
2122122212221121212211222112112
2122221112112122211221111112222
2222222221211211121121222122112
2111111122212111111211212121222
2122222121112112122111222122222
2121112121122121121221111121212
2121112121112212111222221211222
2121112121221111222221112112122
2122222122122121212211221212122
2111111121221122222112222121222
2222222222222222222222222222222
</pre>

<br>

なんとなく正方形なので、QRコードで間違いないです。<br>
（周りを囲っている"2"は取り除く必要がありました。cutコマンドを使いました。）

<br>

以下のツールを使って解きました。Python2で動くツールです。<br>
[https://github.com/waidotto/strong-qr-decoder](https://github.com/waidotto/strong-qr-decoder)

<pre>
$ python ./sqrd.py aaa.txt
flag{QR-c0de-4-exfiltr@ation}
</pre>

<br>

Flag: `flag{QR-c0de-4-exfiltr@ation}`



<br /><br />
<br /><br />
## [Steganography]: Price Check (100 points)
- - -
### Challenge
> DEADFACE has been hacking random people all over town and no one can seem to figure out what they are doing. All we know at this time is that the most recent site that was hit was Otto’s Grocery Store. Oddly, only the customers are being affected and not the company’s network. What is happening? They hired our firm to sniff out the attack and we successfully captured a strange file being sent across the guest WiFi. We believe this is the primary attack vector, and we’ve heard some victims mention that they had to “scan” something - maybe it is in the wrong format? Can you figure out how this file is being used in the attack to reveal the flag?

Attachment:

- STEG05.csv

<br />

### Solution

以下のようなデータの入ったCSVファイルが渡されます。

<pre>
0,255,255,255,255,255,255,0,255,255,255,255,0,255,0,0,255,255,0,0,255,0,255,255,255,0,0,255,255
255,0,0,0,0,0,255,0,0,0,255,255,0,0,255,0,255,255,0,0,255,255,255,255,0,255,0,255,255
255,0,255,255,255,0,255,0,0,0,255,0,0,0,0,255,0,0,0,255,0,255,0,0,0,255,0,255,255
0,0,255,255,255,0,255,0,0,255,0,255,0,0,255,255,255,0,0,0,255,255,0,255,255,255,255,255,255
255,0,255,255,255,0,255,0,255,255,255,255,0,255,0,255,255,255,0,255,255,255,255,255,255,255,0,255,255
0,0,0,0,0,0,255,0,0,255,255,255,0,255,0,255,0,255,255,255,255,0,0,0,255,255,0,0,255
255,255,255,255,255,255,255,0,255,0,255,0,0,255,255,255,0,255,0,0,255,0,255,0,255,0,0,0,255
255,0,0,0,0,0,0,0,255,255,0,0,255,0,0,0,0,0,255,0,255,0,0,0,255,255,0,0,0
0,255,0,255,255,255,255,0,0,0,255,255,0,255,255,255,0,255,0,0,255,255,255,255,255,255,0,255,255
0,0,255,0,255,255,0,0,0,0,0,0,255,0,255,0,0,255,0,255,255,0,0,0,255,255,0,255,0
255,0,0,255,0,255,255,0,0,255,255,0,255,0,0,0,255,255,255,0,255,0,0,0,255,0,0,0,0
255,255,255,255,0,255,0,0,0,255,255,255,255,255,255,0,255,255,255,0,0,255,0,255,255,0,255,0,255
255,0,0,255,0,0,255,255,255,0,0,255,0,255,0,0,0,255,0,255,255,255,0,255,0,0,0,255,0
255,255,0,255,255,255,0,0,0,0,0,255,0,255,0,255,255,255,0,255,255,255,255,0,0,255,0,255,255
255,0,0,255,255,0,255,255,0,255,255,0,0,255,255,255,0,255,255,0,0,0,0,0,255,255,255,0,0
255,0,0,255,255,255,0,255,0,0,0,255,255,0,0,0,0,255,255,0,0,0,255,255,255,255,0,0,255
0,0,255,255,255,0,255,0,255,0,255,255,255,255,255,255,255,255,0,0,255,255,0,0,0,0,0,0,255
255,0,255,0,255,255,0,255,0,255,255,255,0,0,255,0,0,255,255,0,0,255,255,255,255,255,0,0,0
255,0,0,0,255,0,255,0,255,255,255,255,255,0,0,0,255,0,255,0,255,0,0,0,0,255,255,0,0
255,0,255,255,0,0,0,255,255,0,0,255,0,255,255,0,255,255,255,0,0,0,255,255,255,0,255,0,255
255,0,0,0,255,0,255,255,255,255,255,0,0,255,0,0,0,255,0,255,0,255,255,255,255,255,0,0,255
0,0,0,0,0,0,0,0,255,0,0,0,0,255,0,255,255,255,0,255,255,0,0,0,0,0,0,0,0
255,255,255,255,255,255,255,0,255,0,255,0,255,0,255,0,255,0,255,0,255,0,255,255,255,255,255,255,255
255,0,0,0,0,0,255,0,255,255,0,255,255,0,0,0,255,0,0,255,255,0,255,0,0,0,0,0,255
255,0,255,255,255,0,255,0,255,0,255,0,0,0,0,0,0,0,255,0,0,0,255,0,255,255,255,0,255
255,0,255,255,255,0,255,0,255,0,255,255,255,0,0,0,0,0,255,0,0,0,255,0,255,255,255,0,255
255,0,255,255,255,0,255,0,0,0,255,0,255,0,255,0,0,255,255,0,255,0,255,0,255,255,255,0,255
255,0,0,0,0,0,255,0,0,255,255,0,255,255,255,255,0,255,0,255,0,0,255,0,0,0,0,0,255
255,255,255,255,255,255,255,0,255,0,0,0,255,255,255,0,255,255,0,255,0,0,255,255,255,255,255,255,255
</pre>

<br><br>

これもQRコードですね。

<br>

こちらは、上記のツール（sqrd.py）では解けませんでした。

<br>

ということで、実際に画像に変換してカメラで読み取ることにしました。

過去に何回かやってます。

- [GDG Algiers CTF 2022](https://captureamerica.github.io/writeups/post/gdgalgiers_ctf_2022/) <br>

- [X-MAS CTF 2019](https://captureamerica.github.io/writeups/post/xmas_htsp_ctf/)


<br>

書いたコード

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from PIL import Image

MAX=29
f=open("STEG05.csv","r").read().split("\n")
a=[]
for line in f:
    for y in range(MAX):
        val = line[y]
        if val=='0':
            a.append((255,255,255))
        else:
            a.append((0,0,0))

im = Image.new("RGB",(MAX,MAX))
im.putdata(a)
im.save("QR.png")
```

<br>

Flag: `flag{that_will_be_five_dollars}`



<br /><br />
<br /><br />
## [OSINT]: Cup of Compromised Joe (20 points)
- - -
### Challenge
> After cyber stalking mirveal’s online activity for several weeks, we have finally found a post on GhostTown that should allow us to zero in on his location. By examining the photo in this post, can you determine the city and zip code of where he enjoys getting his coffee?

Attachment:

- Joe.jpeg

<br />

### Solution

Google Lensを使って解きました。

"Find image source" をクリックしたら、見つかりました。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_Joe1.png" alt="deadface_CTF_2024_Joe1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2024_Joe2.png" alt="deadface_CTF_2024_Joe2.png">

<br><br>

Flag: `flag{Fresno-93704}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
