---
title: "KNTU CTF Writeup"
date: 2020-03-07T16:30:00+09:00
lastmod: 2020-03-07T16:30:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fkntuctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [http://kntuctf.ir/#/challenges](http://kntuctf.ir/#/challenges)
<br /><br />
Registrationをした後に気づいたんですが、開催が日本時間の朝1時から5時までの4時間とか。。

せっかくなので、参加しましたけど、8問中2問解いただけです。

順位は21番ですが、32番以降のチームはみんな0点なので、参加者はかなり少なかったようですね。

以下、スコアです。それぞれのチャレンジを、どれくらいの時間で解いたのかが表示されてます。
<br /><br />

<img src="https://captureamerica.github.io/writeups/img/knutctf_2020_Score1.png" alt="knutctf_2020_Score1.png">

:

<img src="https://captureamerica.github.io/writeups/img/knutctf_2020_Score2.png" alt="knutctf_2020_Score2.png">


<br /><br />
## [Misc]: 1 | FINDME (35)
- - -
### Challenge
> easy warm up, search for the flag !

Attachment:

- 大量のファイルが入ったZipファイル


<br />
### Solution
<pre>
captureamerica@kali:~/Downloads$ grep -r CTF .
./files/file15/file15/file5/flag.txt:CTF{thisIsNotTheFlag]
./files/file15/file15/file10/flag.txt:CTF{thisIsNotTheFlag]
./files/file15/file15/file15/flag.txt:CTF{thisIsNotTheFlag]

captureamerica@kali:~/Downloads$ grep -r CTF . | grep -v thisIsNot
./files/file12/file7/file14/flag.txt:CTF{this1sN0tTheFlag}
</pre>

一瞬、Fakeフラグかと思いましたけど、Submitしたら通りました。

<br />
Flag: `CTF{this1sN0tTheFlag}`


<br /><br />
<br /><br />
## [Web]: 2 | Dracula (40)
- - -
### Challenge
> Look What I've found, 90s website Made By a Noob Dracula :)

- http://kntuctf.ir/qqualss/Dracula/

<br />
### Solution
html sourceを見ると、st_234g()関数を呼んでいる箇所が見つかります。

st_234g()関数は index.js の中で定義されていて、Developer tool で beautify すると、以下のような感じです。難読化されています。

<img src="https://captureamerica.github.io/writeups/img/knutctf_2020_indexjs.png" alt="knutctf_2020_indexjs.png">

<br />
ファイルをローカルに保存し、以下のように html 本体からalert()を直に呼ぶようにしました。（真ん中辺りのやつ）

3つ目のalertでフラグが表示されます。

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="./index.css" />
    <title>Dracula</title>
  </head>
  <body>
    <header><p>made by a noob :(</p></header>
    <script src="./index.js"></script>

    ここを追加。
    <script>
    alert(_0x5cc5[0x9]);
    alert(_0x5cc5[0xa]);
    alert(_234g);
    </script>
    
    <div class="head">
      <p>RIP de_cbble</p>
      <div class="inside">
        <input id="CTFinput" type="text" placeholder="Password" />
        <button type="submit" onclick="st_234g">CTFcode</button>
      </div>
    </div>
    <div class="bottom">
      <button type="submit" onclick="__s32f()">Password</button>
      <img src="./assets/90369.png" alt="" />
      <div id="pass"></div>
    </div>
  </body>
</html>
```


<br />
Flag: `CTF{EZPZlemonSQZ}`


<br /><br />
<br /><br />
さて、ここから下の残りの6個は、解けなくて諦めて寝たやつです。

## [Reverse]: 3 | GPanel (50)
- - -
### Challenge
> GPannel has been hacked and the hacker made it inaccessible, the flag is still there...

Attachment:

- gpanel (ELF 64bit)


<br />
### (Unsolved)
Ghidraでソースを確認します。

```C
undefined8 main(int param_1,undefined8 *param_2)

{
  int iVar1;
  undefined8 uVar2;
  size_t sVar3;
  char *__dest;
  long in_FS_OFFSET;
  char local_98 [136];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  text_animation(
                "/===========================================================================\\\n|                             W3lc0m3 t0 GP@n3l                            |\n|                              H@ked By _($&&@                            |\n+===========================================================================+\n"
                );
  if (param_1 == 3) {
    text_animation(" ~> Verifying.");
    verify_animation(3);
    iVar1 = strcmp((char *)param_2[1],"BaD_gUy");
    if (iVar1 == 0) {
      sVar3 = strlen((char *)param_2[2]);
      __dest = (char *)malloc(sVar3 + 1);
      strcpy(__dest,(char *)param_2[2]);
      verify_animation(3);
      iVar1 = strcmp(__dest,"evvxhonx0fuc");
      if (iVar1 == 0) {
        text_animation("Correct!\n");
        text_animation("Welcome back!\n");
        snprintf(local_98,0x80,"CTF{%s}\n",param_2[2]);
        text_animation(local_98);
        text_animation("ENCRYPT3D WITH LOVE <3 \n");
      }
      else {
        text_animation(&DAT_00102598);
        text_animation("ACCESS DENIED\n");
        text_animation(" ~> Incorrect password\n");
      }
      uVar2 = 0;
    }
    else {
      text_animation("\nTitle: \nA Discovrse of Fire and Salt (A Discourse of Fire and Salt)");
      text_animation(&DAT_00102220);
      putchar(10);
      text_animation("ACCESS DENIED\n");
      text_animation(" ~> Incorrect username\n");
      uVar2 = 1;
    }
  }
  else {
    puts("[ERROR] Login information missing");
    printf("Usage: %s <username> <password>\n",*param_2);
    uVar2 = 1;
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return uVar2;
}
```

<br />
```
captureamerica@kali:~/Downloads$ ./gpanel BaD_gUy evvxhonx0fuc
/===========================================================================\
|                              W3lc0m3 t0 GP@n3l                            |
|                               H@ked By _($&&@                             |
+===========================================================================+
 ~> Verifying.......Correct!
Welcome back!
CTF{evvxhonx0fuc}
ENCRYPT3D WITH LOVE <3 
```


<br />
なんかまだencryptされていてるらしく、`CTF{evvxhonx0fuc}`は通りませんでした。。



<br /><br />
<br /><br />
## [Web?]: 4 | Mississippi Has Four Ss (55)
- - -
### Challenge
> I've always hated the word MISSISSIPI from Dictation to Discrete Mathematics ! Why four Ss ?!

http://kntuctf.ir/qqualss/MissisippiHasFourSs/


<br />
### (Unsolved)
ちょっとググったら、Permutationとかの数学の話が出てきて眠くなりました。


<br /><br />
<br /><br />
## [Stego]: 5 | Girl With Pearl Earring (60)
- - -
### Challenge
> What is this ^_^ , 1010110.png looks suspicious... Dive in and take a look?

Attachment:

- ^_^（中身は^が一文字入っているだけ）
- 1010110.jpg


<br />
### (Unsolved)
「青い空を見上げればいつもそこに白い猫」で開いて、いろいろ見てると以下のように0と1が浮かんできます。

<img src="https://captureamerica.github.io/writeups/img/knutctf_2020_1010110.png" alt="knutctf_2020_1010110.png">

これをOCRみたいので読み取るんだろうか？ でも、いまいちはっきり見えないし、"^"と言っているのでXORをして解かないといけないんでしょうね。


<br /><br />
<br /><br />
## [Crypto]: 6 | Caesar Salad with Balsamic Vinegar (65)
- - -
### Challenge
> How to make perfect Caesar Salad with Balsamic Vinegar ? first mix all of the ingredients...
<br /><br />
Hint: ADD ALL OF THE INGREDIENTS
<br />
Hint: DON'T FORGET TO USE PEPPER

Attachment:

- 1.txt

<pre>
'3',
'c',
'9',
'a',
'6',
'17',
'17',
'15',
'4',
'4',
'18',
'9',
'11',
'e',
'3',
'c',
'12',
'4',
'e',
'7',
'f',
'9',
'13',
'1',
'1e',
'9' 
</pre>

- 2.txt

<pre>
'c',
'18',
'1a',
'1',
'13',
'6',
'7',
'f',
'9',
'b',
'e',
'c',
'1b',
'1e',
'5',
'1c',
'11',
'4',
'8',
'1d',
'11',
'10',
'5',
'16',
'4',
'2' 
</pre>

- 3.txt

<pre>
'3c',
'34',
'2a',
'36',
'3f',
'25',
'2b',
'27',
'3f',
'35',
'20',
'3c',
'21',
'1f',
'45',
'32',
'1f',
'4e',
'3b',
'24',
'2d',
'28',
'40',
'3f',
'27',
'36'
</pre>

<br />
### (Unsolved)
ヒントに従って、数字をそれぞれ足して、`PEPPER`をKeyに Vigenèreデコードするだけなのかなって思ったけど、ダメでした。



<br /><br />
<br /><br />
## [Pwn?]: 7 | CSafe (70)
- - -
### Challenge
> The Flag has been Saved On CSafe. I wanted to give it to you, but you know, I forgot my username and password T_T.

Attachment:

- gate.c
- vault.o
- vault.h


<br />
### (Unsolved)
これは、見てないです。



<br /><br />
<br /><br />
## [Web]: 8 | FFSdoJS (75)
- - -
### Challenge
> Guess the Flag, and use our CTF Flag Checker...

http://kntuctf.ir/qqualss/FFSdoJS/


<br />
### (Unsolved)
これも、見てないです。




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

