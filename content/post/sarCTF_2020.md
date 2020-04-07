---
title: "SarCTF by Saratov State University 2020 Writeup"
date: 2020-02-17T00:00:00+09:00
lastmod: 2020-02-23T14:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fsarctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2020/02/23 - 復習しました)

URL: [https://sarctf.tk/challenges](https://sarctf.tk/challenges)
<br /><br />
シャーロック・ホームズをテーマにしたCTFでした。シャーロック・ホームズはNetflixで一通り見ましたよ〜
<br /><br />
WebとCryptoはほぼスルー（Web問題のうち半分はvk.comのアカウントを作らないと解けないとか書かれてたし）、Stegoは今回のは手強くて降参です。
<br /><br />
開始当初、サーバがめっちゃ不安定で、チャレンジも見れないわ、フラグもSubmitできないわで、もうやめようかと思ったんですが、寝て起きた次の日は直っていたのでよかったです。


<img src="https://captureamerica.github.io/writeups/img/sarctf_Score1.png" alt="sarctf_Score1.png">
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/sarctf_Score2.png" alt="sarctf_Score2.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/sarctf_Score3.png" alt="sarctf_Score3.png">
<br />


<br /><br />
## [Misc]: Deep dive
- - -
### Challenge
> Worth digging into these tricks.

Attachment:

- flag.txt


<br />
### Solution
オニオン系（マトリョーシカ系？）の問題で、ひらすらファイルを解凍しつづけるやつです。Tar archiveから始まります。

<pre>
$ file flag.txt 
flag.txt: POSIX tar archive (GNU)
</pre>

<br>
20階層くらいまで手動でやったところで、ファイルサイズも全然小さくならないし手動でやっていたら終わらない気がしたので、Pythonスクリプトを書きました。

この手のチャレンジはよく出てくるので、前からツール化したいとは思ってたんですよね。

コードは載せまんが、アイデアとしては、Pythonからfileコマンドを呼んで、そのファイルタイプによって該当する解凍コマンドを呼ぶ。それだけのことです。

ところどころ、sleep(1)を入れてパッチしないとエラーになったりしました。（システムコールを使って解凍してるのがいけないっぽいです。。）

出てくるファイルがすべてflag.txtだったので、ループはPython内ではやらず、watchでやりました。

<pre>
$ watch -n 1 "./Deep_dive_solve.py flag.txt"
</pre>

時間は計ってないけど、30分くらいかかったような気がします。（ちょっとコードがよくなかったかも）

<br>
Flag: `FLAG{matri0sha256}`


<br /><br />
<br /><br />
## [Misc]: Layouts
- - -
### Challenge
> Sherlock found a huge pile of evidence, but it was difficult for him to analyze them. Help him.

Attachment:

- RWtm7A5f (パスワード付きZip file)


<br />
### Solution
zip2johnを使って、Password Crackしました。

<pre>
$ zip2john RWtm7A5f > Layouts.txt

$ john Layouts.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
RWtm7A5f         (RWtm7A5f/Lz68qMZU)
</pre>


ファイル名がパスワードみたいです。（最初、クラックできなかったのかと思いました）

何回か繰り返しやったところ、そのパターンでいけるようなので、スクリプト化しました。

Layouts_solve.shを以下のように作成。
```bash
#/bin/bash
unzip -P $1 $1
rm $1
```

<br>
同一フォルダ内でzipファイルを見つけて、そのファイル名を上記のスクリプトの引数に渡す、ということを繰り返しやります。
```bash
$ watch -n 1 "find . -exec file {} \; | grep -i zip | cut -d: -f1 | cut -c 3- | xargs -Ixxx ./Layouts_solve.sh xxx"
```

<br>
更に続きがあって、ある程度解凍すると今度はXZファイルが出てきます。

解凍すると、以下のディレクトリとファイルが出てきます。見たところ、ファイルは全部0バイトです。

<pre>
$ find .
.
./203
./117
./117/12
./143
./232
./108
./147
./223
./249
./70
./159
./105
./255
./30
./72
./208
./179
./181
./63
./233
./163
./165
./34
./146
./152
./150
./41
./209
./32
./196
./238
./19
./48
./217
./95
./95/15
./40
./43
./53
./53/17
./101
./101/9
./62
./172
./123
./123/5
./178
./190
./92
./155
./5
./130
./230
./79
./194
./136
./16
./231
./137
./113
./177
./214
./184
./138
./216
./145
./195
./18
./80
./116
./52
./52/14
./52/7
./221
./228
./133
./13
./168
./135
./67
./205
./103
./103/8
./111
./127
./46
./69
./170
./200
./37
./93
./114
./160
./73
./20
./81
./174
./90
./169
./121
./246
./236
./56
./253
./243
./144
./149
./222
./229
./167
./26
./171
./57
./98
./125
./125/21
./182
./15
./91
./24
./244
./14
./50
./2
./183
./3
./204
./148
./77
./83
./83/1
./1
./112
./112/18
./212
./220
./235
./215
./241
./10
./76
./11
./31
./187
./99
./74
./192
./154
./242
./206
./39
./210
./251
./248
./64
./180
./59
./6
./85
./250
./44
./28
./218
./82
./33
./47
./100
./202
./51
./51/10
./247
./54
./219
./87
./21
./225
./122
./122/6
./86
./186
./213
./12
./128
./88
./9
./22
./166
./173
./142
./185
./65
./71
./120
./120/13
./126
./17
./239
./84
./84/4
./140
./175
./68
./198
./75
./224
./119
./124
./161
./227
./191
./129
./78
./78/3
./27
./61
./157
./38
./189
./42
./245
./237
./153
./211
./25
./104
./29
./115
./193
./58
./102
./102/11
./23
./164
./36
./188
./118
./162
./254
./8
./55
./89
./89/2
./7
./197
./35
./240
./131
./226
./141
./132
./97
./110
./110/16
./252
./139
./156
./201
./151
./106
./60
./45
./158
./199
./4
./134
./109
./176
./207
./107
./49
./49/19
./49/20
./94
./234
./96
./66
</pre>


<br>
ディレクトリ名がAscii文字列範囲なのと、ファイル名がバラけているのに気づきました。

取り出して、ソートして、Pythonを使って文字変換します。

<pre>
$ find . | cut -c 3- | grep / | awk -F '/' '{print $2, $1}' | sort -n | cut -d" " -f2 | tr "\n" " " ; echo
83 89 78 84 123 122 52 103 101 51 102 117 120 52 95 110 53 112 49 49 125

$ python -c 'print("".join([chr(int(x)) for x in "83 89 78 84 123 122 52 103 101 51 102 117 120 52 95 110 53 112 49 49 125".split()]))'
SYNT{z4ge3fux4_n5p11}
</pre>

<br>
最後は、ROT13です。

Flag: `FLAG{m4tr3shk4_a5c11}`




<br /><br />
<br /><br />
## [Forensics]: Doc. Holmes
- - -
### Challenge
> Sherlock got it on his super secret channels. You have received a copy of mail. Is everything okay with it?

Attachment:

- some.file


<br />
### Solution

<pre>
$ file some.file 
some.file: Microsoft Word 2007+
</pre>

Zipとして解凍したら、/word/media/image2.jpg がフラグです。

ファイル名 (some.file) も一応ヒントなんでしょうね。

Flag: `flag{prominentplace}`



<br /><br />
<br /><br />
## [Forensics]: Confidential
- - -
### Challenge
> Sherlock was able to intercept the transmission of secret data passed between Moriarty's agents. Examine the traffic and try to find confidential information.

Attachment:

- captured.pcap


<br />
### Solution
Wiresharkを使ってpcapを見たところ、いろいろファイルが転送されているようです。

一際怪しかったのが、database.kdbxというファイルです。keepass2johnでクラックします。

<pre>
$ keepass2john database.kdbx > database.kdbx.hash

$ john --wordlist=/usr/share/wordlists/rockyou.txt database.kdbx.hash 
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64])
Cost 1 (iteration count) is 60000 for all loaded hashes
Cost 2 (version) is 2 for all loaded hashes
Cost 3 (algorithm [0=AES, 1=TwoFish, 2=ChaCha]) is 0 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
blowme!          (database)
1g 0:00:19:51 DONE (2020-02-15 22:06) 0.000838g/s 118.8p/s 118.8c/s 118.8C/s blowme!..bloodlust1
Use the "--show" option to display all of the cracked passwords reliably
Session completed
</pre>

<br>
パスワード `blowme!` が見つかりました。

<br>
KeePass.exeを使って開くと、Andreaさんのパスワードがフラグになってました。

<img src="https://captureamerica.github.io/writeups/img/sarctf_keepass.png" alt="sarctf_keepass.png">

<br>
Flag: `FLAG{bru73_p455w0rd_4ll_n16h7_l0n6}`



<br /><br />
<br /><br />
## [Reverse]: Crossw0rd
- - -
### Challenge
> While the children were playing toys, Sherlock was solving crosswords in large volumes.

Attachment:

- crossw0rd (ELF 64-bit)


<br />
### Solution
Ghidraで開くと、param_1という配列の値をチェックしているのが見つかります。

抜粋すると、こんな感じです。（配列要素を全部10進数に変えてます）
<pre>
param_1[7] == '5'
param_1[17] == 'g'
param_1[2] == 'A'
param_1[15] == 'i'
param_1[9] == 'r'
param_1[1] == 'L'
param_1[10] == '3'
param_1[18] == '}'
param_1[6] == 'a'
param_1[0] == 'F'
param_1[14] == '5'
param_1[16] == 'n'
param_1[3] == 'G'
param_1[11] == 'v'
param_1[5] == '3'
param_1[4] == '{'
param_1[12] == '3'
param_1[8] == 'y'
param_1[13] == 'r'
</pre>

<br>
これをファイル (crossw0rd.txt) に保存して、ソートします。

<pre>
$ cat crossw0rd.txt | cut -d[ -f2 | sort -n | cut -d\' -f2 | tr -d "\n" ; echo
FLAG{3a5yr3v3r5ing}
</pre>

<br>
Flag: `FLAG{3a5yr3v3r5ing}`



<br /><br />
<br /><br />
## [PPC]: Mind palace I
- - -
### Challenge
> It looks like the situation is hopeless, there is no time to think. However, you can use the mind palace and solve all problems instantly.
<br /><br />
nc 212.47.229.1 33001


<br />
### Solution
繋いでみると、pip と piiip というのが表示されます。モールス信号っぽいです。

WireSharkでキャプチャーして、いくつか目視でモールス変換してみると、英単語 here, up, on になったので、モールス信号確定です。

ちょっと長め (10分くらい？) にキャプチャーする必要がありました。

テキストエディタを使って、pipを「.」、piiipを「-」に変換します。

<pre>
.... . .-. .   ..- .--. --- -.   - .... .   .-.. .- .--. . .-..   --- ..-.   -- -.--   -.-.
 --- .- -   -.-- --- ..-   -- .- -.--   ... . .   - .... .   .-. .. -... -... --- -.   --- 
..-.   -- -.--   -.. . -.-. --- .-. .- - .. --- -.   -... ..- -   - .... .   -- . -.. .- .-
..   .. - ... . .-.. ..-.   ..   -.- . . .--.   .. -.   .-   .-.. . .- - .... . .-. -.   .-
-. --- ..- -.-. ....   .- -   .... --- -- .   ..-. .-.. .- --.   ... .... . .-. .-.. --- -.
-. -.-   .-.. .. -.- . ...   -.-- --- ..- .-.   -- --- .-. ... . .... . .-. .   ..- .--. --
- -.   - .... .   .-.. .- .--. . .-..   --- ..-.   -- -.--   -.-. --- .- -   -.-- --- ..-  
 -- .- -.--   ... . .   - .... .   .-. .. -... -... --- -.   --- ..-.   -- -.--   -.. . -.-
. --- .-. .- - .. --- -.   -... ..- -   - .... .   -- . -.. .- .-..   .. - ... . .-.. ..-. 
  ..   -.- . . .--.   .. -.   .-   .-.. . .- - .... . .-. -.   .--. --- ..- -.-. ....   .- 
-   .... --- -- .   ..-. .-.
</pre>

<br>
文字への変換は以下のサイトにお世話になりました。

http://www.unit-conversion.info/texttools/morse-code/

以下が、取れた結果です。

here??upon??the??lapel??of??my???oat??you??may??see??the??ribbon??o???my??decoration??but??the??meda???itself??i??keep??in??a??leathern???ouch??at??home??flag??sherlo?k??likes??your??morsehere??up?n??the??lapel??of??my??coat??you??may??see??the??ribbon??of??my??de?oration??but??the??medal??itself??i??keep??in??a??leathern??pouch??a???home??fr

<br>
ちょっとGuessも入りましたが、フラグが取れました。

Flag: `flag{sherlock_likes_your_morse}`


<br /><br />
<br /><br />
## [PPC]: Mind palace II
- - -
### Challenge
> It's time to strain your brains.
<br /><br />
nc 212.47.229.1 33002


<br />
### Solution
pwntoolを使ってひたすら繰り返しROT13するだけです。

Flag: `FLAG{Y0U_V3RY_F45T3R_CRYPT0GR4PH}`


<br /><br />
<br /><br />
## [PPC]: Mind palace III
- - -
### Challenge
> 100% of brain CPU
<br /><br />
nc 212.47.229.1 33003


<br />
### Solution
pwntoolを使ってひたすら繰り返しXOR, OR, AND演算するだけです。

Flag: `FLAG{0HH_Y0UR3_4_V3RY_5M3RT_M4TH3M4T1C}`



<br /><br />
<br /><br />
## [PPC]: Magic of numbers
- - -
### Challenge
> Do you think Sherlock can beat a computer in math?
<br /><br />
nc 212.47.229.1 33004


<br />
### (Unsolved)
降参です。。。

暗算：

<pre>
$ nc 212.47.229.1 33004
======================================================================================================
Hey, hello! Just send me an answer to 9 simple examples, so I can check if my machine knows math well.
======================================================================================================
[>] 0.1 + 0.2 + 0.3
[>] Result: 0.6
[>] 0.2 + 0.3 + 0.4
[>] Result: 0.9
[>] 0.3 + 0.4 + 0.5
[>] Result: 1.2
[>] 0.4 + 0.5 + 0.6
[>] Result: 1.5
[>] 0.5 + 0.6 + 0.7
[>] Result: 1.8
[>] 0.6 + 0.7 + 0.8
[>] Result: 2.1
[>] 0.7 + 0.8 + 0.9
[>] Result: 2.4
[>] 0.8 + 0.9 + 0.10
[>] Result: 1.8
[>] 0.9 + 0.10 + 0.11
[>] Result: 1.11
[>] 0.10 + 0.11 + 0.12
[>] Result: 0.33
You made a mistake somewhere! Bay, bay!

</pre>

<br>
pythonで計算したのをコピペ：

<pre>
$ nc 212.47.229.1 33004
======================================================================================================
Hey, hello! Just send me an answer to 9 simple examples, so I can check if my machine knows math well.
======================================================================================================
[>] 0.1 + 0.2 + 0.3
[>] Result: 0.6000000000000001
[>] 0.2 + 0.3 + 0.4
[>] Result: 0.9
[>] 0.3 + 0.4 + 0.5
[>] Result: 1.2
[>] 0.4 + 0.5 + 0.6
[>] Result: 1.5
[>] 0.5 + 0.6 + 0.7
[>] Result: 1.8
[>] 0.6 + 0.7 + 0.8
[>] Result: 2.0999999999999996
[>] 0.7 + 0.8 + 0.9
[>] Result: 2.4
[>] 0.8 + 0.9 + 0.10
[>] Result: 1.8000000000000003
[>] 0.9 + 0.10 + 0.11
[>] Result: 1.11
[>] 0.10 + 0.11 + 0.12
[>] Result: 0.33
You made a mistake somewhere! Bay, bay!

</pre>


あと、pwntool使ったりもしたりしたけど、どこが間違っているのかわからないです。

<b>Bay, bay!</b>

<br /><br />
（Writeup見てると、どうやら途中で何かFixされてたっぽいですね。。。）



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後（2020/02/23）に行った復習です。他の方のWriteupとか参照してます。


<br />
## [Forensic]: Blogger
- - -
### Challenge
> Recently, John's keys began to be pressed by themselves when he runs his blog. You need to figure out what's the matter.

Attachment:

- usb_here.pcapng

<br>
### Solution
USB pcapは過去のCTFでも何度かお目になっていて、tsharkでデータを取り出して3バイト目辺りを変換表に基づいて文字に換える、という事はなんとなく知ってたんですが、いっつも後回ししていて今回もスルーしました。

今回、解けた人もだいぶ多かったし、一度スクリプトを用意してしまえば使い回しができるんでしょうね。

<pre>
$ tshark -r usb_here.pcapng -T fields -e usb.capdata | tr -s "\n" > usb.capdata 
</pre>

{{% admonition tip "Tips" %}}
tr -s "\n" は空行を削除
{{% /admonition %}}

<br>
<pre>
$ ./usb.py usb.capdata 
sherlock,john,andhenrythenvisitthehollowinthehopeoffindingthehound.ontheway,johnnoticeswhatseemstobeflag[like-a-b100dh0und]e
</pre>
（Pythonコードは他の人の書いたコードのほぼパクリなので載せません。）

<br>
Flag: `flag{like-a-b100dh0und}`



<br /><br />
<br /><br />
## [Stego]: Red King
- - -
### Challenge
> Just Moriarty? Really?

Attachment:

- m0r1ar7y.png

<br>
### Solution
"Red King" というタイトルから、赤をなんとかするんだろうと思ってLSBも取ってみたけど、よくわからなかったやつです。

縦方向に読み込む必要があったみたいです。ふーん。

以下は、「青い空を見上げればいつもそこに白い猫」のスクリーンショットです。

<img src="https://captureamerica.github.io/writeups/img/sarctf_m0r1ar7y.png" alt="sarctf_m0r1ar7y.png">

<br>
Flag: `FLAG{who_is_moriarty}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

