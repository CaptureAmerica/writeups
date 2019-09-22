---
title: "NACTF 2019 Writeup"
date: 2019-09-23T07:00:00+09:00
lastmod: 2019-09-23T07:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://www.nactf.com/challenges](https://www.nactf.com/challenges)
<br /><br />
解いたのはこんな感じ。
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/nactf_Score1.png" alt="nactf_Score1.png">

<img src="https://captureamerica.github.io/writeups/img/nactf_Score2.png" alt="nactf_Score2.png">
<br /><br />

Cryptoはほぼ放置。

General Skillsに出てきたCellularなんじゃも放置。

Reverse EngineeringのKeygenは解きたかった。でも、解けなかった。。＞＜

Pwnも途中でバテました。。。

<br /><br />
いくつかWriteup残しておきます。



<br /><br />
# [General Skills]: Hwang's Hidden Handiwork (100 points)
- - -
## Challenge
> Hwang was trying to hide secret photos from his parents. His mom found a text file with a secret string and an excel chart which she thinks could help you decrypt it. Can you help uncover Hwang's Handiwork?
<br /><br />
Of course, the nobler of you may choose not to do this problem because you respect Hwang's privacy. That's ok, but you won't get the points.
<br /><br />
Hint1: Hwang used the chart to substitute each letter in the plaintext, character by character... can you reverse the process?
<br /><br />
Hint2: Hwang made the image too small to read. Can you make it bigger?

Attachment:

- hwangshandiwork.txt
- substitution.csv

添付ファイルの中身は、こんなです。
```
$ cat hwangshandiwork.txt 
SccLJ0ddkSGy=PP=kM8JMDmPCcMCcymPedh9_r_GwDtt.::/.1TS_Ba:uU9KNpzir:VcNEVK/PPDXCImKlqK8rqtfOAvisA2MIikfjEq1ReFNC/gi_bf5fbrOSxrODf 

$ cat substitution.csv 
Plaintext,a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,1,2,3,4,5,6,7,8,9,0,.,/,-,_,=,:
Ciphertext,T,v,m,9,M,j,=,S,a,i,w,k,e,C,P,L,X,D,J,c,8,h,f,_,.,t,I,B,q,R,Q,Z,U,n,K,u,l,E,-,7,6,g,N,p,/,s,Y,3,:,4,o,A,x,H,G,1,b,F,W,2,z,r,y,d,O,V,5,0
```


<br />
## Solution
このチャレンジはなかなか面白かったです。

substitutionはsedを使いました。
```
cat hwangshandiwork.txt | sed -e y@Tvm9Mj=SaiwkeCPLXDJc8hf_.tIBqRQZUnKulE-76gNp/sY3:4oAxHG1bFW2zrydOV50@abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890./-_=:@
https://lh3.googleusercontent.com/vdx0x3krzzyWWSy4ahxBiWJGdIQR9j0W_tQL_ISoorqnAcIKCIu0Czw-ZbjTZ8eAjlwfLC4Dm6QnSPjx5w=w50-h10-rw
```

<br />
出てきたURLにアクセスすると、めっちゃちっちゃいgoogleflag.webpというファイルが取得できます。

<img src="https://captureamerica.github.io/writeups/img/googleflag.webp.png" alt="googleflag.webp.png">


初めてみるファイルタイプでした。
```
$ file googleflag.webp
googleflag.webp: RIFF (little-endian) data, Web/P image
```

<br />
ググってみると、<br />「出てきたURLの最後の「-rw」を削るとpngやらjpgやら元の形式になりますよ」<br />とのことだったので、末尾の「-rw」を取り除いたら確かにpngとして保存できました。

```
$ file googleflag.png 
googleflag.png: PNG image data, 50 x 5, 8-bit/color RGB, non-interlaced
```

<br />
ここで少し悩んで、ヒントをもう一回見直しました。

> Hint2: Hwang made the image too small to read. Can you make it bigger?

<br />
なるほど、確かにURLの末尾の方に「w50-h10」というのがあって、widthとhightっぽいのがありましたね。

さきほどURLをいじってファイルタイプが変わったように、ここをいじればサイズを大きくできそうです。

10倍「w500-h100」にしてみました。

https://lh3.googleusercontent.com/vdx0x3krzzyWWSy4ahxBiWJGdIQR9j0W_tQL_ISoorqnAcIKCIu0Czw-ZbjTZ8eAjlwfLC4Dm6QnSPjx5w=w500-h100

<img src="https://captureamerica.github.io/writeups/img/googleflag.webp2.png" alt="googleflag.webp2.png">

へぇぇ〜

Flag: `nactf{g00gl3_15nt_s3cur3_3n0ugh}`




<br /><br />
<br /><br />
# [General Skills]: SHCALC (200 points)
- - -
## Challenge
> John's written a handy calculator app - in bash! Too bad it's not that secure...
<br /><br />
Hint: Heard of injection?


<br />
## Solution
以下の通り。
```
> $(ls)
sh: 1: arithmetic expression: expecting EOF: "calc.sh
flag.txt"

> $(cat flag.txt)
sh: 1: arithmetic expression: expecting EOF: "nactf{3v4l_1s_3v1l_dCf80yOo}"
```

Flag: `nactf{3v4l_1s_3v1l_dCf80yOo}`



<br /><br />
<br /><br />
# [Cryptography]: Loony Tunes (50 points)
- - -
## Challenge
> Ruthie is very inhumane. She keeps her precious pigs locked up in a pen. I heard that this secret message is the password to unlocking the gate to her PIGPEN. Unfortunately, Ruthie does not want people unlocking the gate so she encoded the password. Please help decrypt this code so that we can free the pigs!
<br /><br />
P.S. "_" , "{" , and "}" are not part of the cipher and should not be changed
<br /><br />
P.P.S the flag is all lowercase

Attachment:

- pig.jpg

<img src="https://captureamerica.github.io/writeups/img/pig.jpg" alt="pig.jpg">


<br />
## Solution
問題文の中の「PIGPEN」が大文字になっていたのでググってみると、Pigpen Cipherっていうのが見つかりました。

[https://en.wikipedia.org/wiki/Pigpen_cipher](https://en.wikipedia.org/wiki/Pigpen_cipher)

あとは、それにしたがって一文字ずつ解読するだけです。

Flag: `nactf{th_th_th_thats_all_folks}`



<br /><br />
<br /><br />
# [Reverse Engineering]: Keygen (600 points)
- - -
## Challenge
> Can you figure out what the key to this program is?
<br /><br />
Hint: Don't know where to start? Fire up a debugger, or look for cross-references to data you know something about.

Attachment:

- keygen-1 (ELF 32bit)


<br />
## Not Solved
これは見る限りangrで解く問題だと思うんですけど、どうにも答えが得られなかったです。

Keyサイズは8文字。

Keyとして利用できる文字は0~9、a~z、A~Zなんですが、全部対象にすると結構なパターンになるし、Keyは1パターンじゃないだろうから文字を絞って（例えば数字だけとかにして）constraintsに設定してやればできる気がしたんですけどね。

後日、復習しておきます。




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

