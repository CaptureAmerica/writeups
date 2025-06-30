---
title: "BCACTF 6.0 Writeup"
date: 2025-06-30T07:00:00+09:00
lastmod: 2025-06-30T07:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbcactf_2025%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://play.bcactf.com/](https://play.bcactf.com/)
<br /><br />

BCACTF は、5回目の参加です。

[BCACTF 1.0](https://captureamerica.github.io/writeups/post/bcactf_2019/) (62位)

[BCACTF 2.0](https://captureamerica.github.io/writeups/post/bcactf_2021/) (63位)

[BCACTF 3.0](https://captureamerica.github.io/writeups/post/bcactf_2022/) (54位)

BCACTF 4.0 - 不参加

[BCACTF 5.0](https://captureamerica.github.io/writeups/post/bcactf_2024/) (239位)

BCACTF 6.0 (78位) - 今回

<br />

今回は975点を獲得し、最終順位は78位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_score.png" alt="bcactf_2025_score.png">

<br /><br />


今回は、参加者が少なかったですね。

[CTFTime](https://ctftime.org/) からイベントが消えちゃっていたり、イベント開始までレジストレーションができなかったりしたのが原因なのかな、と思います。



<br><br>

以下はチャレンジのリストです。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_misc.png" alt="bcactf_2025_misc.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_binex.png" alt="bcactf_2025_binex.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_crypto.png" alt="bcactf_2025_crypto.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_rev.png" alt="bcactf_2025_rev.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_webex.png" alt="bcactf_2025_webex.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_algo.png" alt="bcactf_2025_algo.png">

<br />



<br /><br />
<br /><br />
## [Misc]: dessert_time (25 points)
- - -
### Challenge
> I'm a very good planner, but my robotics competition really fried my brain! I remember eating something for the past few weeks but I just can't seem to remember what. Can you please help me? Here's my schedule for reference
<br /><br />
Hint: I remember eating it early in the morning, but sometimes I would snack on it in the evening too.

<br>

### Solution
Google Sheetが与えられます。

フラグの一部である `{` をサーチしてみたらヒットしました。視覚的に見えないのは、フォントの色のせいかな（そこは調べませんでした）。

<br />

正規表現を使って数字以外をサーチしてみました。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_dessert_time.png" alt="bcactf_2025_dessert_time.png">

<br><br>

すべての文字を集めたらフラグになります。

<br />

Flag: `bcactf{Apple_pieS_Are_yuMMy}`



<br /><br />
<br /><br />
## [Misc]: Molasses (25 points)
- - -
### Challenge
> This GIF shows the flag letter-by-letter but it's really slow and I don't want to wait all day. Maybe there's a faster way to get the flag?

Attachment:

- molasses.gif

<br>

### Solution

Animation GIF で、タイマーが長くセットされているんだと思うんですが、Mac PC の Preview で開いたらすべての画像が表示されたので、各画像の文字を並べるだけでした。

想定解は別にあるんでしょうね。たぶん。

<br />

Flag: `bcactf{is_it_gif_or_gif_a51fd7ace416}`



<br /><br />
<br /><br />
## [Misc]: Annoying (50 points)
- - -
### Challenge
> Sometimes things are more annoying than they are difficult.

Attachment:

- DLXENH.zip

### Solution

かなり階層の深いzipファイルです。

Mac PC の Archive Utility で解凍すると、途中で3回ほど停止しましたが、ほぼ全自動で解凍できました。

これも、想定解ではないかな。。

<br />

Flag: `bcactf{w3ll_th4t_w4s_4nn0y1ng_fca743ef043c1d8}`



<br /><br />
<br /><br />
## [Forensics]: Flag Flag (25 points)
- - -
### Challenge
> It's a flag—inside a flag.

Attachment:

- flag-flag.png

<img src="https://captureamerica.github.io/writeups/img/flag-flag.png" alt="flag-flag.png">

<br><br>

### Solution

各色の RGB の値が、文字に変換できます。

rgb(98,99,97)

rgb(99,116,102)

rgb(123,49,106)

rgb(78,55,117)

rgb(89,107,106)

`+7D` は、`}` のことです。

<br />

Flag: `bcactf{1jN7uYkj}`



<br /><br />
<br /><br />
## [Forensics]: Recovery Shop (50 points)
- - -
### Challenge
> Welcome to the recovery shop! For a small price, we can restore you to your best look!
<br><br>
Hint: Have you tried looking at yourself in the mirror? (the next hint is a real one)

Attachment:

- recovers.heic

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_heic.png" alt="bcactf_2025_heic.png">


<br><br>


### (Unsolved)

これは、解けませんでした。

<br>

まず、exiftoolで以下が見つかります。

<pre>
$ exiftool recovers.heic
:
Layer Names                     : orz, 6, 2, 5, 1, 3, bcactf{dOn!t_trUs!_the_T1Tle1}, 4
Layer Unicode Names             : orz, 6, 2, 5, 1, 3, bcactf{dOn!t_trUs!_the_T1Tle1}, 4
</pre>

これが next hint と言われるものですかね？

<br>

.heic ファイルは Mac PC で普通に開けるし、なんなら文字のコピーもできます。

コピペで読み取った文字列がこちら。

<pre>
bcactf{th!ss_
@!e_83Kl
ts_fr
_b!n@na_
f$_|@gOrz}
</pre>

<br>

改行を除くと、`bcactf{th!ss_@!e_83Klts_fr_b!n@na_f$_|@gOrz}` になりますが、これはフラグとして通りませんでした。

結局のところ、大文字と小文字が間違っていたり、`|`が`l`だったりするだけなんですが、画像から文字を読取るだけのチャレンジでは、見間違いやすい文字は使わないで欲しいと思いました。

<br>

ヒントも、意味不明過ぎるし。むしろ無い方がよかった。

Recoveryって何？ Mirrorって何？ "Don't trust the title" って何？？

ただの愚痴です。

<br>

まぁ、学んだこともありました。

{{% admonition tip "Tips" %}}
Mac PCで画像からコピペで読み取った文字列は、実際の文字とは異なることがある。
{{% /admonition %}}




<br /><br />
<br /><br />
## [Forensics]: tune in (50 points)
- - -
### Challenge
> my boss radioed me a message but I can't hear it. he told me to tune in, can you help me out?
<br /><br />
Hint: look for suspicious audio

Attachment:

- transmission.wav

<br>

### (Unsolved)

これは、解けませんでした。調査の過程だけ、残しておきます。

<br>

.wavファイルなので、まず Audacity で開きます。

ステレオのR側に音の大きな雑音、L側にSSTVの問題でよく出てくる音が入っています。

L側は音がかなり小さくて、音量を上げる必要がありました。

<br />

Audacity の Amplify では音量を上げられなかった（おそらくR側の音が大きいから）ので、Normalizeを使いました。

`Normalize stereo channels independently` のオプションは必須です。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_audacity_tune_in_1.png" alt="bcactf_2025_audacity_tune_in_1.png">

<br /><br />

Exportする際も、`Mono`にしないといけません。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_audacity_tune_in_2.png" alt="bcactf_2025_audacity_tune_in_2.png">

<br /><br />

Windows PC にコピーして、RX-SSTVツールを使ってみましたが、まともな画像は取れませんでした。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_audacity_tune_in_3.png" alt="bcactf_2025_audacity_tune_in_3.png">


<br />



<br /><br />
<br /><br />
## [Forensics]: plain-sight (75 points)
- - -
### Challenge
> Can you help find my flag? I tried asking around but all I found was this poem.

Attachment:

- hiding_in_plain_sight.pdf

<br>

### Solution

Libre Office で開いて、オブジェクトを移動したりするとフラグが見つかるやつです。

<br />

Flag: `bcactf{Now_y0U_s3e_mE}`




<br /><br />
<br /><br />
## [Forensics]: Tarred and Feathered (75 points)
- - -
### Challenge
> i recently exfiltrated this archive from a suspect's computer. they illegally stole all of our flags.
<br />
however, not everything is as it seems. the flag is in the archive, but where? i couldn't find it, can you?
<br /><br />
Hint1: Not everything in the archive is what it seems. Look beyond the visible files.
<br />
Hint2: how can you get more information about what files are in the archive?

Attachment:

- challenge.tar.gz

<br>

### Solution

普通に解凍しようとすると、以下のようなエラーになります。解凍には失敗しますが、flag.txtのパスはわかります。

<pre>
$ tar zxvf challenge.tar.gz
x note.txt
x ../.cache/.hidden_logs/.�\200\213flag.txt: Path contains '..': Unknown error: -1
tar: Error exit delayed from previous errors.
</pre>

<br />

以下のように`-O`オプションとパスの指定でフラグの中身が取れました。

<pre>
$ tar zxvf challenge.tar.gz -O ../.cache/.hidden_logs/
x ../.cache/.hidden_logs/.�\200\213flag.txtbcactf{you_tarred_me_apart_1bf1fb1}
</pre>

<br />

Flag: `bcactf{you_tarred_me_apart_1bf1fb1}`



<br /><br />
<br /><br />
## [Web]: People (50 points)
- - -
### Challenge
> Find the administrator's email address. Wrap it in bcactf{}.
<br /><br />
http://challs.bcactf.com:42593

<br>

### (Unsolved)

多くの人が解けているのに、自分は解けませんでした。

<br>

View Source で以下が見つかります。

<pre>
&lt;span>Name: Roscoe Quigley</span>&lt;br />
&lt;!-- username: administrator -->
&lt;img src="https://gravatar.com/avatar/06003dfd18a22e6809e736cf27eaabb6" loading="lazy" />
&lt;hr/>
</pre>

<br />

結局のところ、このHash値を CrackStation でCrackしたらメールアドレスが見つかるんですが、そういう発想は全く浮かばなかったです。

<br />

だって、username: administrator なんだし、あとはドメイン名を見つける問題かな、と思っていろいろと調べたんですが、

何気に Roscoe Quigley っていう人も実在するみたいだし、でもそれだとOSINTチャレンジだしなぁと思いつつ、時間の無駄になりそうだったので諦めました。

<br />

せめて role: administrator みたいにしておいてくれたら、よかったのにと思います。

ただの愚痴です。



<br /><br />
<br /><br />
## [Web]: Temps (75 points)
- - -
### Challenge
> Find out when this web app was compiled. Report your answer as a Unix timestamp in milliseconds wrapped with bcactf{}. (For example, the date "Wed, 01 Jan 2020 05:00:00 GMT" would be reported as bcactf{1577854800000})
<br /><br />
http://challs.bcactf.com:26137

<br>

### Solution

`_app/version.json` を見ればバージョンがわかるようです。

<pre>
http://challs.bcactf.com:26137/_app/version.json
{"version":"1735776187591"}
</pre>

<br />

Flag: `bcactf{1735776187591}`



<br /><br />
<br /><br />
## [Web]: What? (75 points)
- - -
### Challenge
> Surmount the insurmountable.
<br /><br />
http://challs.bcactf.com:47861

<img src="https://captureamerica.github.io/writeups/img/bcactf_2025_web_what.png" alt="bcactf_2025_web_what.png">

Attachment:

- what.php (<-- イベントの途中で追加されました)

<br>

### Solution

添付ファイル（what.php）が提供される前に解けました ^^

<br />

2つのHash値を比較するアプリだとすると、Hash Collisionを元にしたチャレンジなのが予想できます。

参考：

https://github.com/spaze/hashes?tab=readme-ov-file


<br />

以下の2つ使って、フラグが取れました。

<pre>
TEXTCOLLBYfGiJUETHQ4hAcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
TEXTCOLLBYfGiJUETHQ4hEcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
</pre>


<br />

Flag: `bcactf{wh0_kn0ws_4nym0r3_11fab08d769a}`



<br /><br />
<br /><br />
## [OSINT]: Japanese Lyrics OSINT (100 points)
- - -
### Challenge
> Out of the top 25 most streamed songs in Japan according to Wikipedia, we picked a song (with Japanese lyrics) and translated a line from Japanese to English using various large language models. Here were the results (each line is a different translation):
<br><br>
An unending, unending flavor has seeped in.
<br><br>
There's a taste that won't go away, it's stained into me.
<br><br>
The taste that won't disappear, won't disappear, has soaked in deeply.
<br><br>
The taste that doesn't fade, doesn't fade, is deeply ingrained.
<br><br>
What is the name of the song, according to its English Wikipedia page? Wrap the name in bcactf{}. It's case-sensitive, so please follow the case as it is on Wikipedia.
<br><br>
Hint1: The difficulty in this challenge perhaps lies in gathering the lyrics to the top 25 most streamed songs. But maybe there's an easy way to get that done?
<br><br>
Hint2: The same word is translated as "unending", "won't go away", "won't disappear", and "doesn't fade". It may be worth finding out what word that is.
<br><br>
Hint3: What is the Japanese word for flavor/taste?
<br><br>
Hint4: The flag is a romanized Japanese word.

<br>

### Solution

リストは以下のWikiにありました。

https://en.wikipedia.org/wiki/List_of_best-selling_singles_in_Japan

<br />

そもそも曲数が25曲しかなくて、フラグは99回Submitできるので、Brute Forceするだけでした。9回目で当たりました。

<br />

Flag: `bcactf{Kaibutsu}`





<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />