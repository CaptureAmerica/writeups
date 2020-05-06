---
title: "NeverLAN CTF 2020 Writeup"
date: 2020-02-12T09:00:00+09:00
lastmod: 2020-02-23T14:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fneverlan_ctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2020/02/23 - 復習しました)

URL: [https://ctf.neverlanctf.com/challenges](https://ctf.neverlanctf.com/challenges)
<br /><br />
久々にがっつりCTFやりました。（学生さん用CTFでした）

最終順位は、127位でした。

しかし、サーバが結構不安定でしたね。最終日は日中にサーバがダウンしてて、夜中になって追加チャレンジと共に復活してたので、真夜中に少しやりました。。

<br /><br />
<img src="https://captureamerica.github.io/writeups/img/neverlan_Score1.png" alt="neverlan_Score1.png">
<br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/neverlan_Score2.png" alt="neverlan_Score2.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/neverlan_Score3.png" alt="neverlan_Score3.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/neverlan_Score4.png" alt="neverlan_Score4.png">
<br />




<br /><br />
## [Reverse Engineering]: Adobe Payroll (100)
- - -
### Challenge
> We've forgotten the password to our payroll machine. Can you extract it?

Attachment:

- Adobe_Payroll.7z (Adobe_Employee_Payroll.exe)

### Solution
PEファイルが入ってます。

<pre>
$ file Adobe_Employee_Payroll.exe 
Adobe_Employee_Payroll.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows
</pre>

dnSpyで開くと、フラグっぽいのが見つかります。

<img src="https://captureamerica.github.io/writeups/img/neverlan_Adobe.png" alt="neverlan_Adobe.png">

<br>
<pre>
$ python -c 'print("".join([chr(int(x)) for x in "102 108 97 103 123 46 110 101 116 95 105 115 95 112 114 101 116 116 121 95 101 97 115 121 95 116 111 95 100 101 99 111 109 112 105 108 101 125".split()]))'
flag{.net_is_pretty_easy_to_decompile}
</pre>

<br>

Flag: `flag{.net_is_pretty_easy_to_decompile}`



<br /><br />
<br /><br />
## [Reverse Engineering]: Reverse Engineer (300)
- - -
### Challenge
> This program seems to get stuck while running... Can you get it to continue past the broken function?

Attachment:

- revseng (ELF 64-bit)

### Solution
300 pointの割に、一瞬で解けるチャレンジでした。

Ghidraで見ると、フラグを表示するためのprint()という関数があるのが見つかります。

```C
void print(void)

{
  undefined *puVar1;
  char *flg;
  char rb;
  char lb;
  char g;
  char a;
  char l;
  char f;
  
  puVar1 = (undefined *)malloc(0x15);
  *puVar1 = 0x77;
  puVar1[1] = 0x33;
  puVar1[2] = 99;
  puVar1[3] = 0x6f;
  puVar1[4] = 0x6e;
  puVar1[5] = 0x37;
  puVar1[6] = 0x72;
  puVar1[7] = 0x30;
  puVar1[8] = 0x6c;
  puVar1[9] = 0x74;
  puVar1[10] = 0x68;
  puVar1[0xb] = 0x33;
  puVar1[0xc] = 0x62;
  puVar1[0xd] = 0x31;
  puVar1[0xe] = 0x6e;
  puVar1[0xf] = 0x61;
  puVar1[0x10] = 0x72;
  puVar1[0x11] = 0x69;
  puVar1[0x12] = 0x33;
  puVar1[0x13] = 0x73;
  puVar1[0x14] = 0;
  printf("%c%c%c%c%c%s%c\n",0x66,0x6c,0x61,0x67,0x7b,puVar1,0x7d);
  return;
}
```

<br>
どこからも呼ばれていないので、gdbでjumpします。

<pre>
gef➤  b main
gef➤  r
gef➤  x print
0x5555555551a9 <print>:	0xe5894855
gef➤  jump *0x5555555551a9
Continuing at 0x5555555551a9.
flag{w3con7r0lth3b1nari3s}
</pre>

Flag: `flag{w3con7r0lth3b1nari3s}`


<br /><br />
<br /><br />
## [Crypto]: Dont Take All Knight (75)
- - -
### Challenge
> （添付ファイルのみ）

Attachment:

- DontTakeAllKnight.png

<img src="https://captureamerica.github.io/writeups/img/DontTakeAllKnight.png" alt="DontTakeAllKnight.png">

### Solution
これは、もうひとつのチャレンジ（Pigsfly）で見てたWiki (https://en.wikipedia.org/wiki/Pigpen_cipher) にVariantsとして解説が載ってました。

Flag: `evenknightsneedcrypto`


<br /><br />
<br /><br />
## [Crypto]: The Invisibles (75)
- - -
### Challenge
> （添付ファイルのみ）

Attachment:

- The_invisibles.png

<img src="https://captureamerica.github.io/writeups/img/The_invisibles.png" alt="The_invisibles.png">

### Solution
適当に1個画像をピックアップして、Googleで画像サーチすると、以下のサイトが見つかります。

https://www.dcode.fr/arthur-invisibles-cipher

これで解析すると、FLAGISYOUCANSEETHEM が取れます。

<br>

Flag: `youcanseethem`


<br /><br />
<br /><br />
## [Crypto]: My own encoding (200)
- - -
### Challenge
> Here's an encoding challenge. This doesnt really test your technical skills, but focuses on your critical thinking.
<br /><br />
I wrote my own encoding scheme. Can you decode it?"
<br /><br />
    -BashNinja

Attachment:

- secretmessage.jpg

<img src="https://captureamerica.github.io/writeups/img/secretmessage.jpg" alt="secretmessage.jpg">

### Solution
25マスあるので、アルファベット（26文字）がちょうど当てはまりそうです。そのうちの1文字はBlank（何もない状態）なのが予想できて、AかZかどちらかですが、実際に問題に出てきているのでAの方を選びました。

つまり、表にはアルファベットが以下のように割り当てられます。

何もない状態：A
<br>
<table>
<tr><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr>
<tr><td>G</td><td>H</td><td>I</td><td>J</td><td>K</td></tr>
<tr><td>L</td><td>M</td><td>N</td><td>O</td><td>P</td></tr>
<tr><td>Q</td><td>R</td><td>S</td><td>T</td><td>U</td></tr>
<tr><td>V</td><td>W</td><td>X</td><td>Y</td><td>Z</td></tr>
</table>

Flag: `nicejobyouhacker`


<br /><br />
<br /><br />
## [Crypto]: BabyRSA (250)
- - -
### Challenge
> We've intercepted this RSA encrypted message 2193 1745 2164 970 1466 2495 1438 1412 1745 1745 2302 1163 2181 1613 1438 884 2495 2302 2164 2181 884 2302 1703 1924 2302 1801 1412 2495 53 1337 2217 we know it was encrypted with the following public key e: 569 n: 2533

### Solution
何気に少しハマりました。

というのも、encrypted messageは1つにまとめて処理すると思っていて、RsaCtfTool.pyがうまく動かなかったのです。

<pre>
$ RsaCtfTool.py -p 149 -q 17 -e 569 --uncipher 21931745216497014662495143814121745174523021163218116131438884249523022164218188423021703192423021801141224955313372217
Traceback (most recent call last):
  File "~/Python3/RsaCtfTool-master/RsaCtfTool.py", line 108, in decrypt
    stderr=DN)
  File "~/.pyenv/versions/3.7.3/lib/python3.7/subprocess.py", line 395, in check_output
    **kwargs).stdout
  File "~/.pyenv/versions/3.7.3/lib/python3.7/subprocess.py", line 487, in run
    output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command '['openssl', 'rsautl', '-raw', '-decrypt', '-in', '/var/folders/87/sclh7gdd7k1dlkyqkdmwqjfd0z5kf0/T/tmprmtfqw00', '-inkey', '/var/folders/87/sclh7gdd7k1dlkyqkdmwqjfd0z5kf0/T/tmp8r3uasek']' returned non-zero exit status 1.
</pre>

過去のRSA用のPythonスクリプトもことごとく動かず、降参しようかと思っていたところ、最初の1個（2193）だけで RsaCtfTool.py をかけたところ 'f' になったので、以下のように解きました。

ちなみに、n = 2533は 17 * 149 なので、p = 17, q = 149 を使ってやった方が計算が速いです。

```sh
#!/bin/sh
while read line
do
   ~/Python3/RsaCtfTool-master/RsaCtfTool.py -p 17 -q 149 -e 569 --uncipher $line
done < cipher.txt
```

<br>
実行結果：

<pre>
$ echo "2193 1745 2164 970 1466 2495 1438 1412 1745 1745 2302 1163 2181 1613 1438 884 2495 2302 2164 2181 884 2302 1703 1924 2302 1801 1412 2495 53 1337 2217" | tr " " "\n" > cipher.txt

$ ./NeverLAN_BasyRSA.sh
[+] Clear text : b'\x00f'
[+] Clear text : b'\x00l'
[+] Clear text : b'\x00a'
[+] Clear text : b'\x00g'
[+] Clear text : b'\x00{'
[+] Clear text : b'\x00s'
[+] Clear text : b'\x00m'
[+] Clear text : b'\x004'
[+] Clear text : b'\x00l'
[+] Clear text : b'\x00l'
[+] Clear text : b'\x00_'
[+] Clear text : b'\x00p'
[+] Clear text : b'\x00r'
[+] Clear text : b'\x001'
[+] Clear text : b'\x00m'
[+] Clear text : b'\x003'
[+] Clear text : b'\x00s'
[+] Clear text : b'\x00_'
[+] Clear text : b'\x00a'
[+] Clear text : b'\x00r'
[+] Clear text : b'\x003'
[+] Clear text : b'\x00_'
[+] Clear text : b'\x00t'
[+] Clear text : b'\x000'
[+] Clear text : b'\x00_'
[+] Clear text : b'\x00e'
[+] Clear text : b'\x004'
[+] Clear text : b'\x00s'
[+] Clear text : b'\x00y'
[+] Clear text : b'\x00}'
[+] Clear text : b'\x00\n'
</pre>

<br>

Flag: `flag{sm4ll_pr1m3s_ar3_t0_e4sy}`


<br /><br />
<br /><br />
## [Forensics]: Listen to this (125)
- - -
### Challenge
> You hear that?
<br /><br />
*Your flag will be in the normal flag{flagGoesHere] syntax
<br /><br />
-ps This guy might be important
<br /><br />
-ZestyFE

Attachment:

- HiddenAudio.mp3

### Solution
とりあえずmp3ファイルを再生してみると、誰かが何かしゃべってます。

さっぱりわからなかったので、`5 point` 使って Hint を Unlock しました。。。

`Hint: Morse what now?`

んん？ モールス信号なんか入ってたっけ？

もう一度再生。 よく聞いたら、確かに聞こえました。。。

{{% admonition tip "Tips" %}}
音声ファイルにVoiceが入っている場合は、まずVoiceを取り除く。
{{% /admonition %}}

<br>
Audacityでファイルを開き、Effect -> Vocal Remover... を選択します。オプションは、Remove vocals + Simple です。

この時点では、音声はほぼ完全に取り除けてますが、音楽がちょっと残っています。

<br>
ここで、モールス信号を解析できるツールを探しました。ググる際のキーワードは、"morse audio analyzer" てな感じです。

以下のサイトにファイルをアップロードしてやってみましたが、かなり精度が悪く、全くフラグが取れません。

https://morsecode.world/international/decoder/audio-decoder-adaptive.html

ただ、助かったのは、どうやらモールス信号の周波数が563Hzくらいだということです。（たぶん）


<br>
Audacityで以下をやりました。

- Vocal Removerのオプションで、Retain freqnency bandを選び、560 ~ 570 辺りのみ取り出し。
- ファイルの後ろ側に、ボリュームの大きい関係ない音が入っているので削除。（これをしないとAmplifyに制限がかかっちゃうため）
- Amplifyして、ボリュームアップ。
- View -> Zoom in

<img src="https://captureamerica.github.io/writeups/img/NeverLAN_HiddenAudio.png" alt="NeverLAN_HiddenAudio.png">

<br>
ちなみに、このファイルも上記のサイトで試しましたが、全然ダメでした。

ということで、目視で解析。この作業自体は思ったより難しくはなかったです。

一覧表はWikiにあります。(https://en.wikipedia.org/wiki/Morse_code)


<br>

Flag: `flag{ditsanddahsforlife}`



<br /><br />
<br /><br />
## [Programing]: DasPrime (100)
- - -
### Challenge
> My assignments due and I still don't have the answer! Can you help me fix my Python script... and also give me the answer? I need to make a prime number generator and find the 10,497th prime number. I've already written a python script that kinda works... can you either fix it or write your own and tell me the prime number?
<br /><br />
-ZestyFE

Attachment:

```Python
import math
def main():
    primes = []
    count = 2
    index = 0
    while True:
        isprime = False
        for x in range(2, int(math.sqrt(count) + 1)):
            if count % x == 0: 
                isprime = True
                continue
        if isprime:
            primes.append(count)
            print(index, primes[index])
            index += 1
        count += 1
if __name__ == "__main__":
    main()
```

### Solution
全然想定解じゃないんですけど、ウェブで素数の一覧を探して10497番目の素数を見つけただけです。。（ごめんなさい）

以下のデータにお世話になりました。<br>
http://www.ysr.net.it-chiba.ac.jp/~yashiro/sosu/

<pre>
$ cat prime.txt | tr " " "\n" | nl | grep 10497
</pre>
<br>

Flag: `110573`



<br /><br />
<br /><br />
## [Recon]: Front Page of the Internet (50)
- - -
### Challenge
> Whoops... I leaked a flag on a public website
<br /><br />
-ZestyFE


### Solution
とりあえず、`Front Page of the Internet` を Google でサーチします。

Reddit のことなんですね。Reddit自体は毎日見てるけど、そういう呼び名があるのは知らなかったです。

<br>
次に、Reddit内で `ZestyFE` をサーチします。そうこうしていると、以下のページに辿り着き、そこにフラグが見つかります。

https://www.reddit.com/r/Hacking_Tutorials/comments/dz2ws8/public_ctf_cite_for_middlehigh_school_classes/fgvew6c/?context=3

Flag: `flag{l3arningFr0mStr4ng3rs}`




<br /><br />
<br /><br />
## [Recon]: Thats just Phreaky (200)
- - -
### Challenge
> The first of many stories that have been told. 01 September 2017 | 14:01

### (Unsolved)
Harry Potter の 19 years later 辺りの話なのはわかるんですが、フラグがなんなのかがわかりませんでした。

`14:01` の辺りの情報が見つからなかったんですが、そこがキーポイントなのかな。

あんまり労力を費やしたくなかったので、潔く降参！

<br /><br />
(2020/02/23 追記)

他の方のWriteupを参照させてもらったら、全然 Harry Potter じゃなかった件。。は、恥ずかしい〜＞＜




<br /><br />
<br /><br />
## [Web]: Follow Me! (100)
- - -
### Challenge
> Let's start here. https://7aimehagbl.neverlanctf.com

### Solution
ひたすらリダイレクトが走ります。

`--max-redirect`を増やしたらそのうち終わるのかと思いきや、無限にループします。

```C
$ wget -O - https://7aimehagbl.neverlanctf.com/ --max-redirect=200
Location: http://03pjxvv2l6.neverlanctf.com [following]
Location: http://1qntnni112.neverlanctf.com [following]
Location: http://3w299ja5ez.neverlanctf.com [following]
:
200 redirections exceeded.
```

<br>
Wiresharkでキャプチャしたら、フラグ出てました。。。

<br>

Flag: `flag{d0nt_t3ll_m3_wh3r3_t0_g0}`



<br /><br />
<br /><br />
## [Trivia]: AAAAAAAAAAAAAA! I hate CVEs (20)
- - -
### Challenge
> This CVE reminds me of some old school exploits. If `flag` is enabled in sudoers.

### Solution
CVE-2019-18634 なんだろうとすぐ思ったんだけど、フラグはCVE番号だとてっきり思っていて少しハマりました。

Flag: `pwfeedback`



<br /><br />
<br /><br />
## [Trivia]: Rick Rolled by the NSA??? (50)
- - -
### Challenge
> This CVE Proof of concept Shows NSA.gov playing "Never Gonna Give You Up," by 1980s heart-throb Rick Astley.
<br /><br />
Use the CVE ID for the flag. flag{CVE-?????????}

### Solution
`"Never Gonna Give You Up" POC exploit` をキーワードにググると、以下のサイトが見つかりました。

https://arstechnica.com/information-technology/2020/01/researcher-develops-working-exploit-for-critical-windows-10-vulnerability/

<br>

Flag: `flag{CVE-2020-0601}`


<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後（2020/02/23）に行った復習です。他の方のWriteupとか参照してます。

<br />
## [Crypto]: It is like an onion of secrets.
- - -
### Challenge
> Crypto Hard Jake From S7a73farm
<br />
This one has layers like an onion. Just don't let it make you cry..

Attachment:

- Much_Confused.png

(Hintが3つくらいあったみたいです。全然見てなかったです。。)

<br>

### Solution

zstegで長いbase64 encodeされた文字列が取れます。

<pre>
$ zsteg -l 0 Much_Confused.png -v b1,rgb,lsb,xy 
</pre>

<br>
それをデコードすると、`lspv wwat kl rljvzfciggvnclzv` という文字列が取れます。ここで詰まってました。

`neverlanctf` を Key として、Vigenere Cipherで復号したらよかったみたいです。そういうヒントがあったのかな？

復号は、以下のサイトにお世話になりました。

https://www.mygeocachingprofile.com/codebreaker.vigenerecipher.aspx

復号結果："your flag is myfavoritecipher"

<br>

Flag: `flag{myfavoritecipher}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

