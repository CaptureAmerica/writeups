---
title: "RACTF 2021 Writeup"
date: 2021-10-11T20:00:00+09:00
lastmod: 2021-10-11T20:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fractf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://digitaloverdose.ractf.co.uk/](https://digitaloverdose.ractf.co.uk/)
<br /><br />
たぶん、RACTFって同じ名前のCTF (https://2021.ractf.co.uk/) が他にもありますね。

<br />

2900点を獲得し、チームとしては68位、個人では38位でした。

なお、参加チームは719、参加プレーヤーは1118人だったようです。

<br><br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_Score1.png" alt="ractf_2021_Score1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_Score2.png" alt="ractf_2021_Score2.png">


<br><br>
チャレンジのリストです。

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall1.png" alt="ractf_2021_chall1.png">

<br>

普段はOSINT系のチャレンジはあんまりやらないんですが、日本の場所を当てるやつだったので頑張って全部解きました。

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall2.png" alt="ractf_2021_chall2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall3.png" alt="ractf_2021_chall3.png">

<br>

これは、Rainbow Tableを参照したり、John the ripper や Hashcat を使って全部解けました。

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall4.png" alt="ractf_2021_chall4.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall5.png" alt="ractf_2021_chall5.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall6.png" alt="ractf_2021_chall6.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall6.png" alt="ractf_2021_chall6.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall7.png" alt="ractf_2021_chall7.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall8.png" alt="ractf_2021_chall8.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall9.png" alt="ractf_2021_chall9.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall10.png" alt="ractf_2021_chall10.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/ractf_2021_chall11.png" alt="ractf_2021_chall11.png">

<br>



<br /><br />
## [Crypto]: Constant Primes (75 points)
- - -
### Challenge
> RSA is the hardest and most non-repetitive asymmetric key encryption. 
<br /><br />
This is your first time seeing one, right? Well be prepared, you will not find the flag. 
<br /><br />
I have given you this key (id_rsa file), it’s this random base64 string, no way you'll know what to do with it.
<br /><br />
The ciphertext is 0x2085f3d3573cd709fad84bed9fe8dde419fb7c8e96aa95ec4651a3bc07b5552f321e03404943744d931a4a51a817cf190880a5efbf94aa828c45da5b31dcdefc

Attachment:

- id_rsa (RSA PRIVATE KEY)

<br />
### Solution
まず、ssh-keygen を実行してみたのですが、エラーになりました。。

<pre>
$ ssh-keygen -f id_rsa -e -m pem | openssl asn1parse
Load key "id_rsa": Invalid key length
Error: offset out of range

</pre>

<br />

以下のコードで、`n` と `e` は取れました。

```Python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from Crypto.PublicKey import RSA

key = RSA.importKey(open('id_rsa', 'r').read())
print(key.n)
print(key.e)
```

<br>

実行結果：

<pre>
$ ./read_key.py
6682669787535606635993287896641983501298577520322405381880667699429293248249880135523414618303313461640928583929883384586229757283439591741435143918813267
65537
</pre>

<br>

n, e, c が揃えば、あとは RsaCtfTool.py で解けます。

反転したものが出てくるので、rev してフラグが得られます。

<pre>
$ ~/Python3/RsaCtfTool/RsaCtfTool.py -n 6682669787535606635993287896641983501298577520322405381880667699429293248249880135523414618303313461640928583929883384586229757283439591741435143918813267 -e 65537 --uncipher 1703380908157528283172041137132390349496157838445725605013427432689517712852707895688494322667218403379009536599334770578485119117275052717689607980637948
[+] Clear text : b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00}drah_taht_t0n_s1_ASR{OD'

$ echo }drah_taht_t0n_s1_ASR{OD | rev
DO{RSA_1s_n0t_that_hard}

</pre>

<br />

Flag: `DO{RSA_1s_n0t_that_hard}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 1 - Dare enter the mirror world (50 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_1.jfif" alt="Challenge_1.jfif">

<br />
### Solution
Google画像検索をしたか、Google Lensを使ったか、覚えてないです。

これは、表参道にある東急プラザの入り口ですね。

<br />

Flag: `DO{Tokyu_Plaza}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 2 - The land of culture (75 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_2.jfif" alt="Challenge_2.jfif">

<br />
### Solution
拡大してみると、秋葉原 COMIC ZIN があるのが見えるので、これをヒントにGoogle Mapを使いました。

対面から写真を撮っているので、Mapをベースに対面を確認すると、そこにはビックカメラがあります。

<br />

Flag: `DO{Bic_Camera_Akiba}`


<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 3 - A childhood favourite (100 points)
- - -
### Challenge
> Where is this Pokémon?

<img src="https://captureamerica.github.io/writeups/img/Challenge_3.jfif" alt="Challenge_3.jfif">

<br />
### Solution
`ミジュマル` というポケモンなのはすぐわかったんですが、場所を見つけるのに苦労しました。

Google Lensでサーチしたら、`池袋` の `サンシャインシティ` にある `ポケモンセンターメガ東京` という場所なのがわかりました。

トライ・アンド・エラーした結果、`サンシャインシティ` がフラグでした。

<br />

Flag: `DO{Sunshine_City}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 4 - One of the classics (100 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_4.jpeg" alt="Challenge_4.jpeg">

<br />
### Solution
ストリートファイターのリュウの置物があるお店のようです。

`ryu hadouken otaku shop` でGoogleサーチしたら、渋谷のパルコにあるお店のようですね。

<br />

Flag: `DO{Shibuya_Parco}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 5.a - The Central Hub (75 points)
- - -
### Challenge
> What building is in this picture?

<img src="https://captureamerica.github.io/writeups/img/Challenge_5.jfif" alt="Challenge_5.jfif">

<br />
### Solution
これは調べるまでもなく、東京駅です。

<br />

Flag: `DO{Tokyo_Station}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 5.b - This way, Emperor Naruhito (75 points)
- - -
### Challenge
> Where was this photo taken from?

（写真は 5.a と同じです。）

<br />
### Solution
これは相当悩みました。。

<s>DO{Tokyo_Marunouchi_Minamiguchi}</s><br>
<s>DO{Marunouchi_Minamiguchi}</s><br>
<s>DO{Marunouchi_Minamiguchi}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Building}</s><br>
<s>DO{Tokyo_Station_Marunouchi}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Minamiguchi}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Building_Minamiguchi}</s><br>
<s>DO{Marunouchi_Ekisha_Minamiguchi}</s><br>
<s>DO{Marunouchi_Building_Minamiguchi}</s><br>
<s>DO{Marunouchi_Building}</s><br>
<s>DO{Tokyo_Station_Hotel}</s><br>
<s>DO{Tokyo_Station_City}</s><br>
<s>DO{Tokyo_Station}</s><br>
<s>DO{Tokyo_Chiyoda_City}</s><br>
<s>DO{Tokyo_Chiyoda}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Station_Building}</s><br>
<s>DO{Marunouchi_Station_Building}</s><br>
<s>DO{Tokyo_Marunouchi_Station_Building}</s><br>
<s>DO{Station_Building}</s><br>
<s>DO{Tokyo_Station_Building}</s><br>
<s>DO{Marunouchi_Ekisha}</s><br>
<s>DO{Tokyo_Marunouchi_Ekisha}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Ekisha}</s><br>
<s>DO{Tokyo_Marunouchi_Ekisha_Minamiguchi}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Ekisha_Minamiguchi}</s><br>
<s>DO{Tokyo_Station_Marunouchi_station_building}</s><br>
<s>DO{Marunouchi}</s><br>
<s>DO{Tokyo_Marunouchi}</s><br>
<s>DO{Marunouchi_Ekimae_Hiroba}</s><br>
<s>DO{Ekimae_Hiroba}</s><br>
<s>DO{Tokyo_Marunouchi_Ekimae_Hiroba}</s><br>
<s>DO{Tokyo_Eki_Marunouchi_Ekimae_Hiroba}</s><br>
<s>DO{Miyuki_Dori}</s><br>
<s>DO{Miyuki_Dori_Street}</s><br>
<s>DO{Tokyo_Station_Ekimae_Hiroba}</s><br>
<s>DO{Marunouchi_Daini_Hiroba}</s><br>
<s>DO{Tokyo_Station_Marunouchi_Ekimae_Hiroba}</s><br>
<s>DO{Tokyo_Marunouchi_Ekisha}</s><br>
<s>DO{Central_Stop}</s><br>
<s>DO{Tokyo_Marunouchi_Central_Stop}</s><br>
<s>DO{Tokyo_Station_Central_Stop}</s><br>
<s>DO{Marunouchi_Guchi}</s><br>
<s>DO{Marunouchiguchi}</s><br>
<s>DO{Miyuki_Street}</s><br>
<s>DO{Marunouchi_Plaza}</s><br>
<s>DO{Gyoko-Dori_Ave}</s><br>

たぶん、こんなに間違ったのは自分だけかもしれません。。

<br />

<b>今日の教訓：トライ・アンド・エラーばかりではなく、ちゃんと写真を解析して解くことが大事！！</b>

<br />
写真をよくみると、撮影位置と駅の間にガードレールがあり、道が通っています。

また、撮影位置のところには、タイルがきれいに四角く色分けされてます。

Google Street Viewでクリックして移動していったら、該当箇所が見つかりました。

<img src="https://captureamerica.github.io/writeups/img/Challenge_5.png" alt="Challenge_5.png">

<br />

Flag: `DO{Gyoko-Dori}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 6 - Along the waterways we go (75 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_6.jfif" alt="Challenge_6.jfif">

<br />
### Solution
「こんなのわかるかよ！」と思いつつ、Google Lensでサーチしたら一発で `小名木川` が出てきました。

Google Lens、凄い！

<br />

Flag: `DO{Onagi_River}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 7 - A box of mystery (100 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_7.jpg" alt="Challenge_7.jpg">

<br />
### Solution
Google Lensでサーチしたら、以下のPDF（国土技術政策総合研究所 研究資料）が見つかりました。

[http://www.nilim.go.jp/lab/bcg/siryou/tnn/tnn0572pdf/ks057208.pdf](http://www.nilim.go.jp/lab/bcg/siryou/tnn/tnn0572pdf/ks057208.pdf)

逆に、これ以外の情報は全く見つからなかったです。

千鳥ヶ淵にある換気所のようです。

この資料にはマップも載っていて、この換気塔の対面にあるのは `イギリス大使館` なので、それが答えです。

<br />

Flag: `DO{British_Embassy}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 8 - A staple drink of Japan (50 points)
- - -
### Challenge
> What is this drink called?

<img src="https://captureamerica.github.io/writeups/img/Challenge_8.jfif" alt="Challenge_8.jfif">

<br />
### Solution
これも結構悩んだんですが、テキトーに `メロンソーダ` を入れたら当たりました。

これって、日本の飲み物なのかな。確かに、外国には `メロンソーダ` っていうのは無いかも。

<br />

Flag: `DO{Melon_Soda}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 9 - A concrete jungle's forest heart (50 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_9.jfif" alt="Challenge_9.jfif">

<br />
### Solution
Google Lensで以下のアメブロが見つかりました。

[https://ameblo.jp/gentle-spwj/entry-12423563923.html](https://ameblo.jp/gentle-spwj/entry-12423563923.html)

`目黒の八芳園`です。確か、前に行ったことあります。

<br />

Flag: `DO{Happo-en}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 10 - An answer in plain sight (250 points)
- - -
### Challenge
> Where sector was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_10.jfif" alt="Challenge_10.jfif">

<br />
### Solution
250点問題だけあって、なかなか手強かったです。

`荒川西日三` という文字が見えるので、荒川区 西日暮里 3丁目 なのはすぐわかります。でも、それは答えじゃありません。

右上にはホテルの看板のようなものがあり、`荒川 西日暮里 3丁目 おんぼろホテル` でサーチしたら `谷中 富士見ホテル` であることもわかりました。

ただ、その下の `←30M` の文字を見逃していたので、富士見ホテルのところで写真を撮ったのと勘違いしてフラグが全然通りませんでした。

つまり写真を撮った場所は `富士見ホテルから30M離れた場所` でした。

Google Mapで富士見ホテルの半径30Mのエリアを探し、Street Viewで確認すると、ありました！

<img src="https://captureamerica.github.io/writeups/img/Challenge_10.png" alt="Challenge_10.png">

`谷中 大島酒店` です。でも、これはまだ答えではありません。

<br />

わたしは知らなかったんですが、この場所、`夕焼けだんだん` という有名な場所らしいですね。

<br />

なにかヒントがあるかと思って `夕焼けだん団` の動画（[https://www.youtube.com/watch?v=kPK75vVniNg](https://www.youtube.com/watch?v=kPK75vVniNg)）を見てたら、「谷中」の読み方は「タニナカ」じゃなくて「ヤナカ」であることがわかりました。

結構、早い段階で「谷中」に辿り着いていたのに、漢字が読めてなかっただけとは。。。

<br />

Flag: `DO{Yanaka}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 11 - An island of mascots (150 points)
- - -
### Challenge
> Where in Japan does this mascot represent?

<img src="https://captureamerica.github.io/writeups/img/Challenge_11.jfif" alt="Challenge_11.jfif">

<br />
### Solution
`あまみ市` って書いてあるし。

<br />

Flag: `DO{Amami}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 12 - A story of the ages (75 points)
- - -
### Challenge
> What mythological creature is this?

<img src="https://captureamerica.github.io/writeups/img/Challenge_12.jfif" alt="Challenge_12.jfif">

<br />
### Solution
ぶんぶく茶釜ですね。

<s>DO{Raccoon}</s><br>
<s>DO{Bunbuku_Chagama}</s><br>
<s>DO{Fox}</s><br>
<s>DO{Raccoon_Dog}</s><br>

普通に `タヌキ` が正解でした。

<br />

Flag: `DO{Tanuki}`



<br /><br />
<br /><br />
## [OSINT: Tour de Japan]: 13 - The river of Sakura (75 points)
- - -
### Challenge
> Where was this photo taken from?

<img src="https://captureamerica.github.io/writeups/img/Challenge_13.jfif" alt="Challenge_13.jfif">

<br />
### Solution
これは、調べるまでもなく、目黒川です。

<br />

Flag: `DO{Meguro_River}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
