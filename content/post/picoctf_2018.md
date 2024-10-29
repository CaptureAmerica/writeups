---
title: "picoCTF 2018 Writeup"
date: 2019-08-11T23:00:00+09:00
lastmod: 2019-09-05T22:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2018%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2018game.picoctf.com/](https://2018game.picoctf.com/)
<br /><br />
picoCTFのことを知ったのがちょっと前で、すでにイベントは2018年時点で終了してるんですが、続けて勉強ができるようになっていたので他のCTFイベントがない平日とかにちょこちょこやりました。

<br /><br />
進み具合はこんな感じ。

<img src="https://captureamerica.github.io/writeups/img/pico_Score.png" alt="pico_Score.png">

もう、これくらいやったら十分かなぁ。気が向いたら、もう少しやるかもです。

<br />
いくつかピックアップしてWriteupを書いておきます。




<br /><br />
## [Forensics]: Desrouleaux (150 points)
- - -
### Challenge
> Our network administrator is having some trouble handling the tickets for all of of our incidents. Can you help him out by answering all the questions? Connect with nc 2018shell.picoctf.com 54782. 
<br /><br />
Hint: If you need to code, python has some good libraries for it.

Attachment:

- incidents.json
<pre>
{
    "tickets": [
        {
            "ticket_id": 0,
            "timestamp": "2016/02/08 08:45:56",
            "file_hash": "33d81fec987b8a8c",
            "src_ip": "246.69.53.233",
            "dst_ip": "48.173.183.246"
        },
        {
            "ticket_id": 1,
            "timestamp": "2017/03/05 06:01:41",
            "file_hash": "f2d2c758fe8853b5",
            "src_ip": "246.69.53.233",
            "dst_ip": "164.217.208.88"
        },
        {
            "ticket_id": 2,
            "timestamp": "2015/02/28 12:24:15",
            "file_hash": "33d81fec987b8a8c",
            "src_ip": "251.165.34.242",
            "dst_ip": "125.130.154.106"
        },
        {
            "ticket_id": 3,
            "timestamp": "2015/06/24 12:14:19",
            "file_hash": "581dc312cc8c16c6",
            "src_ip": "246.69.53.233",
            "dst_ip": "48.173.183.246"
        },
        {
            "ticket_id": 4,
            "timestamp": "2015/11/15 22:34:03",
            "file_hash": "8aa16b403ac7de3e",
            "src_ip": "215.239.98.18",
            "dst_ip": "231.12.49.241"
        },
        {
            "ticket_id": 5,
            "timestamp": "2015/02/06 15:42:17",
            "file_hash": "8aa16b403ac7de3e",
            "src_ip": "246.69.53.233",
            "dst_ip": "168.138.219.19"
        },
        {
            "ticket_id": 6,
            "timestamp": "2017/12/29 05:54:41",
            "file_hash": "8aa16b403ac7de3e",
            "src_ip": "251.165.34.242",
            "dst_ip": "168.138.219.19"
        },
        {
            "ticket_id": 7,
            "timestamp": "2017/10/10 13:11:20",
            "file_hash": "6629fde09ed2dab7",
            "src_ip": "251.165.34.242",
            "dst_ip": "187.229.175.225"
        },
        {
            "ticket_id": 8,
            "timestamp": "2015/01/26 00:01:08",
            "file_hash": "581dc312cc8c16c6",
            "src_ip": "231.205.245.44",
            "dst_ip": "48.173.183.246"
        },
        {
            "ticket_id": 9,
            "timestamp": "2017/05/07 09:51:22",
            "file_hash": "8aa16b403ac7de3e",
            "src_ip": "251.165.34.242",
            "dst_ip": "168.138.219.19"
        }
    ]
}
</pre>

<br />

### Solution
ncで繋ぐと、そこで問題が出てきます。

ヒントを無視してLinuxコマンドで全部解こうかと思ったけど、勉強のために最後だけはPythonで解きました。

> What is the most common source IP address? If there is more than one IP address that is the most common, you may give any of the most common ones.

<pre>
$ grep src incidents.json | cut -d: -f2 | sort | uniq -c
   1  "215.239.98.18",
   1  "231.205.245.44",
   4  "246.69.53.233",
   4  "251.165.34.242",
</pre>
答え：251.165.34.242


<br />

> How many unique destination IP addresses were targeted by the source IP address 246.69.53.233?

<pre>
$ grep 251.165.34.242 incidents.json -A 2 | grep dst | sort | uniq | wc -l
       3
</pre>
答え：3

<br />

> What is the number of unique destination ips a file is sent, on average? Needs to be correct to 2 decimal places.

```Python
#!/usr/bin/env python
# coding: UTF-8

import json

f = open('incidents.json', 'r')
json_dict = json.load(f)

# uniqにするために一旦リストにする。
hash_list = []
for key in json_dict['tickets']:
    hash_list.append(key['file_hash'])  # hashだけ取り出して、リストにappend

uniq_hash = list(set(hash_list))

uniq_dst = 0
for hash in uniq_hash:
    dstip_list = []
    for key in json_dict['tickets']:
        if key['file_hash'] == hash:
            dstip_list.append(key['dst_ip'])
    uniq_dst = uniq_dst + len(list(set(dstip_list)))

print("{:.2f}".format(uniq_dst / len(uniq_hash)))
```
答え：1.40



<br /><br />
<br /><br />
## [General Skills]: you can't see me (200 points)
- - -
### Challenge
'...reading transmission... Y.O.U. .C.A.N.'.T. .S.E.E. .M.E. ...transmission ended...' Maybe something lies in /problems/you-can-t-see-me_3_1a39ec6c80b3f3a18610074f68acfe69.

<br />

### Solution
SSHでアクセスした後、emacsのdiredからファイルを開いたらすぐフラグ見つかりました。
<img src="https://captureamerica.github.io/writeups/img/emacs_dired.png" alt="emacs_dired.png">


<br />
逆にemacsが使えなかったらどうするのか気になって、別解も調べてみました。

ファイル名に空白文字が含まれているようなので、trでアンダースコアに置き換えて可視化しました。

<pre>
captureamerica@pico-2018-shell:/problems/you-can-t-see-me_3_1a39ec6c80b3f3a18610074f68acfe69$ ll
total 60
drwxr-xr-x   2 root       root        4096 Mar 25 19:57 ./
-rw-rw-r--   1 hacksports hacksports    57 Mar 25 19:57 .  
drwxr-x--x 556 root       root       53248 Mar 25 19:58 ../

captureamerica@pico-2018-shell:/problems/you-can-t-see-me_3_1a39ec6c80b3f3a18610074f68acfe69$ ls -la | tr " " "_"
total_60
drwxr-xr-x___2_root_______root________4096_Mar_25_19:57_.
-rw-rw-r--___1_hacksports_hacksports____57_Mar_25_19:57_.__
drwxr-x--x_556_root_______root_______53248_Mar_25_19:58_..

captureamerica@pico-2018-shell:/problems/you-can-t-see-me_3_1a39ec6c80b3f3a18610074f68acfe69$ cat ". "
cat: '. ': No such file or directory

captureamerica@pico-2018-shell:/problems/you-can-t-see-me_3_1a39ec6c80b3f3a18610074f68acfe69$ cat ".  "
picoCTF{j0hn_c3na_paparapaaaaaaa_paparapaaaaaa_cf5156ef}
</pre>






<br /><br />
<br /><br />
## [Cryptography]: Safe RSA (250 points)
- - -
### Challenge
> Now that you know about RSA can you help us decrypt this ciphertext? We don't have the decryption key but something about those values looks funky..
<br /><br />
Hint1: RSA [tutorial](https://en.wikipedia.org/wiki/RSA_(cryptosystem))<br />
Hint2: Hmmm that e value looks kinda small right?<br />
Hint3: These are some really big numbers.. Make sure you're using functions that don't lose any precision!<br />


Attachment:

- ciphertext（以下が中身）

\-\-\-\-\-\-\-\-<br />
N: 374159235470172130988938196520880526947952521620932362050308663243595788308583992120881359365258949723819911758198013202644666489247987314025169670926273213367237020188587742716017314320191350666762541039238241984934473188656610615918474673963331992408750047451253205158436452814354564283003696666945950908549197175404580533132142111356931324330631843602412540295482841975783884766801266552337129105407869020730226041538750535628619717708838029286366761470986056335230171148734027536820544543251801093230809186222940806718221638845816521738601843083746103374974120575519418797642878012234163709518203946599836959811

e: 3

ciphertext \(c\): 2205316413931134031046440767620541984801091216351222789180535786851451917462804449135087209259828503848304180574549372616172217553002988241140344023060716738565104171296716554122734607654513009667720334889869007276287692856645210293194853 
<br />\-\-\-\-\-\-\-\-

<br />

### Solution
RsaCtfTool \( [https://github.com/Ganapati/RsaCtfTool](https://github.com/Ganapati/RsaCtfTool) \) を使ったら解けました。

\-\-\-\-\-\-\-\-<br />
$ python RsaCtfTool.py -n 374159235470172130988938196520880526947952521620932362050308663243595788308583992120881359365258949723819911758198013202644666489247987314025169670926273213367237020188587742716017314320191350666762541039238241984934473188656610615918474673963331992408750047451253205158436452814354564283003696666945950908549197175404580533132142111356931324330631843602412540295482841975783884766801266552337129105407869020730226041538750535628619717708838029286366761470986056335230171148734027536820544543251801093230809186222940806718221638845816521738601843083746103374974120575519418797642878012234163709518203946599836959811 -e 3 --uncipher 2205316413931134031046440767620541984801091216351222789180535786851451917462804449135087209259828503848304180574549372616172217553002988241140344023060716738565104171296716554122734607654513009667720334889869007276287692856645210293194853

[+] Clear text : b'picoCTF{e_w4y_t00_sm411_34096259}'
<br />\-\-\-\-\-\-\-\-
<br /><br />
Flag: `picoCTF{e_w4y_t00_sm411_34096259}`


<br /><br />
<br /><br />
## [Reversing]: be-quick-or-be-dead-1 (200 points)
- - -
### Challenge
> You find [this](https://www.youtube.com/watch?v=CTt1vk9nM9c) when searching for some music, which leads you to be-quick-or-be-dead-1. Can you run it fast enough? You can also find the executable in /problems/be-quick-or-be-dead-1_3_aeb48854203a88fb1da963f41ae06a1c.
<br /><br />
Hint: What will the key finally be?

Attachment:

- be-quick-or-be-dead-1  (ELF 64-bit)

<br />

### Solution
これは、いろんな解き方があると思います。<br />

alert(1)でタイマーを起動していて、flagをprintする前にタイムアウトしちゃうので、gdbでalert(1)をコールしているところで止めて、引数1を0xffとかに書き換えたらオッケー。



<br /><br />
<br /><br />
## [Reversing]: keygen-me-1 (400 points)
- - -
### Challenge
> Can you generate a valid product key for the validation program in /problems/keygen-me-1_2_74297f5e012cf93ee059a2be15d77734

Attachment:

- activate (ELF 32-bit)

<br />

### Solution
Ghidra使っていきます。

```C
undefined4 main(int param_1,int param_2)

{
  char cVar1;
  undefined4 uVar2;
  undefined4 *puVar3;
  
  puVar3 = &param_1;
  setvbuf(stdout,(char *)0x0,2,0);
  if (param_1 < 2) {
    puts("Usage: ./activate <PRODUCT_KEY>");
    uVar2 = 0xffffffff;
  }
  else {
    cVar1 = check_valid_key(*(undefined4 *)(param_2 + 4));
    if (cVar1 == 0) {
      puts("Please Provide a VALID 16 byte Product Key."); // <--- 16バイト
      uVar2 = 0xffffffff;
    }
    else {
      cVar1 = validate_key(*(undefined4 *)(param_2 + 4)); // 以下を参照
      if (cVar1 == 0) {
        puts("INVALID Product Key.");
        uVar2 = 0xffffffff;
      }
      else {
        printf("Product Activated Successfully: ");
        print_flag(puVar3);
        uVar2 = 0;
      }
    }
  }
  return uVar2;
}
```

```C
undefined4 validate_key(char *param_1)

{
  char cVar1;
  size_t sVar2;
  uint local_18;
  int local_14;
  
  sVar2 = strlen(param_1);   // <-- 16文字
  local_18 = 0;
  local_14 = 0;
  while (local_14 < (int)(sVar2 - 1)) {   // 16-1で、15回ループ
    cVar1 = ord((int)param_1[local_14]);
    local_18 = local_18 + (local_14 + 1) * ((int)cVar1 + 1);
    local_14 = local_14 + 1;
  }
  cVar1 = ord((int)param_1[sVar2 - 1]);   // param_1の最後の文字。
  return CONCAT31((undefined3)(cVar1 >> 7),local_18 % 0x24 == (int)cVar1);
}
```
処理を完全に把握しなくても、16文字のうちの先頭からの15文字をなんか処理して、最後の1文字となんかしているのがわかれば十分でした。

以下のチェック関数があって、文字の範囲は決まっているので、Brute Forceで行けます。
```C
undefined4 check_valid_char(char param_1)

{
  undefined4 uVar1;

  // 0~9とA~Zのみ
  if (((param_1 < '0') || ('9' < param_1)) && ((param_1 < 'A' || ('Z' < param_1)))) {
    uVar1 = 0;
  }
  else {
    uVar1 = 1;
  }
  return uVar1;
}
```

<br />
ここからが答えです。

以下のようにシェルスクリプトで解きました。適当な15文字と、最後の1文字をBrute Forceしてます。なお、ローカルで実行する際には、flag.txtを自前で適当に作っておきます。
```Bash
# cat activate_solve.sh 
#!/bin/bash
ARRAY=(A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 0 1 2 3 4 5 6 7 8 9)

for item in ${ARRAY[@]}; do
    echo "AAAAAAAAAAAAAAA"$item
    ./activate "AAAAAAAAAAAAAAA"$item
done

#  ./activate_solve.sh 
AAAAAAAAAAAAAAAA
INVALID Product Key.
AAAAAAAAAAAAAAAB
INVALID Product Key.
AAAAAAAAAAAAAAAC
INVALID Product Key.
AAAAAAAAAAAAAAAD
INVALID Product Key.
AAAAAAAAAAAAAAAE
INVALID Product Key.
AAAAAAAAAAAAAAAF
INVALID Product Key.
AAAAAAAAAAAAAAAG
INVALID Product Key.
AAAAAAAAAAAAAAAH
INVALID Product Key.
AAAAAAAAAAAAAAAI
INVALID Product Key.
AAAAAAAAAAAAAAAJ
INVALID Product Key.
AAAAAAAAAAAAAAAK
INVALID Product Key.
AAAAAAAAAAAAAAAL
INVALID Product Key.
AAAAAAAAAAAAAAAM
INVALID Product Key.
AAAAAAAAAAAAAAAN
INVALID Product Key.
AAAAAAAAAAAAAAAO
Product Activated Successfully: xxx{abcdefg}
AAAAAAAAAAAAAAAP
INVALID Product Key.
AAAAAAAAAAAAAAAQ
INVALID Product Key.
AAAAAAAAAAAAAAAR
INVALID Product Key.
AAAAAAAAAAAAAAAS
INVALID Product Key.
AAAAAAAAAAAAAAAT
INVALID Product Key.
AAAAAAAAAAAAAAAU
INVALID Product Key.
AAAAAAAAAAAAAAAV
INVALID Product Key.
AAAAAAAAAAAAAAAW
INVALID Product Key.
AAAAAAAAAAAAAAAX
INVALID Product Key.
AAAAAAAAAAAAAAAY
INVALID Product Key.
AAAAAAAAAAAAAAAZ
INVALID Product Key.
AAAAAAAAAAAAAAA0
INVALID Product Key.
AAAAAAAAAAAAAAA1
INVALID Product Key.
AAAAAAAAAAAAAAA2
INVALID Product Key.
AAAAAAAAAAAAAAA3
INVALID Product Key.
AAAAAAAAAAAAAAA4
INVALID Product Key.
AAAAAAAAAAAAAAA5
INVALID Product Key.
AAAAAAAAAAAAAAA6
INVALID Product Key.
AAAAAAAAAAAAAAA7
INVALID Product Key.
AAAAAAAAAAAAAAA8
INVALID Product Key.
AAAAAAAAAAAAAAA9
INVALID Product Key.
```
flag: `AAAAAAAAAAAAAAAO`


<br /><br />
<br /><br />
## [General Skills]: script me (500 points)
- - -
### Challenge
> Can you understand the language and answer the questions to retrieve the flag? Connect to the service with nc 2018shell.picoctf.com 1542
Maybe try writing a python script?

<pre>
$ nc 2018shell.picoctf.com 1542
Rules:
() + () = ()()                                      => [combine]
((())) + () = ((())())                              => [absorb-right]
() + ((())) = (()(()))                              => [absorb-left]
(())(()) + () = (())(()())                          => [combined-absorb-right]
() + (())(()) = (()())(())                          => [combined-absorb-left]
(())(()) + ((())) = ((())(())(()))                  => [absorb-combined-right]
((())) + (())(()) = ((())(())(()))                  => [absorb-combined-left]
() + (()) + ((())) = (()()) + ((())) = ((()())(())) => [left-associative]

Example: 
(()) + () = () + (()) = (()())
</pre>

<br />

### Solution
なんかカッコが多くて、複雑そうに見えるけど、()を別の文字に置き換えるとわかりやすいと思います｡
<pre>
x + x = xx
((x)) + x = ((x)x)
x + ((x)) = (x(x))
(x)(x) + x = (x)(xx)
x + (x)(x) = (xx)(x)
(x)(x) + ((x)) = ((x)(x)(x))
((x)) + (x)(x) = ((x)(x)(x))
x + (x) + ((x)) = (xx) + ((x)) = ((xx)(x))
</pre>

要は、カッコが何重になっているかによって処理が異なっていて、以下の通り。

- 同じ深さの場合は、連結
- 左の方が深かければ、左のカッコの中の最後に連結
- 右の方が深かければ、右のカッコの中の先頭に連結

<br>
書いたコード。

```Python
#!/usr/bin/env python
# coding: UTF-8
import re
from pwn import *

host = "2018shell.picoctf.com"
port = 1542
s = remote( host, port )

while 1:
    msg = s.recvregex("\?\?\?|picoCTF{.*}")
    print(msg)
    if re.search("picoCTF", msg):
        exit(0)
        
    lines = msg.split("\n")
    sample = lines[len(lines)-1]
    msg = s.recvuntil("> ")
        
    # まず+で分ける。
    list = sample.split("+")

    for i in range(0,len(list)-1):
        # 2つ取り出して、余分な文字の削除。
        if i == 0:
            a = list[i].strip(' ')
        else:
            a = c
        b = list[i+1].replace(' ', '')
        b = b.replace('=', '')
        b = b.replace('?', '')

        # ()をxに変えておく。
        a = a.replace("()", 'x')
        b = b.replace("()", 'x')
            
        depth = 0
        a_max_depth = 0
        for ch in a:
            if ch == '(':
                depth += 1
                if depth > a_max_depth:
                    a_max_depth = depth
            if ch == ')':
                depth -= 1

        depth = 0
        b_max_depth = 0
        for ch in b:
            if ch == '(':
                depth += 1
                if depth > b_max_depth:
                    b_max_depth = depth
            if ch == ')':
                depth -= 1

        if a_max_depth == b_max_depth:
            c = a + b
        elif a_max_depth > b_max_depth:
            c = a[:-1] + b + ')'
        else:
            c = '(' + a + b[1:]

        c = c.replace('x', "()")

    s.sendline(c)
    print "> %s" % c
```

<br>
[コード解説]

- コードの中でも、わかりやすくするために()をxで置き換えてます。
- `+` で分けて、2つずつ処理。1個目をa、2個目をbにしてます。
- aとbそれぞれ、余計な文字を削除してます。
- あとは、それぞれカッコの深さを調べて、規則にのっとった処理をするだけ。

<br><br>
以下が、実行結果です。

<pre>
~/Python2$ ./pico_script_me.py
[+] Opening connection to 2018shell.picoctf.com on port 1542: Done
Rules:
() + () = ()()                                      => [combine]
((())) + () = ((())())                              => [absorb-right]
() + ((())) = (()(()))                              => [absorb-left]
(())(()) + () = (())(()())                          => [combined-absorb-right]
() + (())(()) = (()())(())                          => [combined-absorb-left]
(())(()) + ((())) = ((())(())(()))                  => [absorb-combined-right]
((())) + (())(()) = ((())(())(()))                  => [absorb-combined-left]
() + (()) + ((())) = (()()) + ((())) = ((()())(())) => [left-associative]

Example:
(()) + () = () + (()) = (()())

Let's start with a warmup.
((())()) + (()) = ???
> ((())()(()))
Correct!

Okay, now we're cookin!
() + ((())()) + (()()()) = ???
> (()(())()(()()()))
Correct!

Alright see if you can get this one.
(()(())) + (()(())) + (()) + (()()()()()()()()) + (()()()(())()()) = ???
> (()(()))(()(())(())(()()()()()()()()))(()()()(())()())
Correct!

This one's a little bigger!
((())()) + ()() + ()() + (()()) + (()(())((()))(((())))((((()))))) + (((()())()())()) + ()() + (()()) + (()(())((()))(((())))((((()))))) + (((()(())((()))(((())))()()))()) = ???
> ((((())()()()()()(()()))()(())((()))(((())))((((()))))(((()())()())())()()(()()))(()(())((()))(((())))((((())))))((()(())((()))(((())))()()))())
Correct!

Ha. No more messin around. Final Round.
(()(())()()())(()()()(())()()) + ((()(())(()()()()()()()()))()(((()()())()())()())) + (((())()()())()(())((()))(((())))((((()))))) + (((()())()(((()()())()())()()))((((()))))(((())))((()))(())()) + (()()(()))(()()()(())()()) + (((()())()(())(()))((((()))))(((())))((()))(())()) + (()()()())(()()()()()()()()) + (((()(()))((()())()())())((((()))))(((())))((()))(())()) + ((()()(((()()())()())()()))()(())((()))(((())))((((()))))) + ((()(())())((()(())((()))(((())))()()))()(((((()))))(((())))((()))(())())) + ((()())(()()()()()()()())((()())()())()) + ((()()()()()())()()()(())()()) + ((()()()())((((()))))(((())))((()))(())()) + ((()(())()()()())()(((()()())()())()())) + (()()((((()))))(((())))((()))(())()) + ((()())()(())) + (((()()()()()()()()())((()())()())())((()(())((()))(((())))()()))()) + ((()(())()(()))()(((()()())()())()())) + (()(()))((())()()()) + (()()()()()(())((()))(((())))((((())))))(((((()))))(((())))((()))(())()) + (()()()()())(()()()) + (((()())(())()()())((()(())((()))(((())))()()))()) + ((()())((()(())((()))(((())))()()))()) + (((()()())()(()))((()(())((()))(((())))()()))()) + (()()()(((()()())()())()())) + ((()()()((()())()())())()(((()()())()())()())) + (()()(())()()()) + ((()(())()()()())()(((()()())()())()())) + ((()()())((()(())((()))(((())))()()))()(((((()))))(((())))((()))(())())) + ((())()()()()) + ((()()()()(((()()())()())()()))((()(())((()))(((())))()()))()) + (()()()()()()) + (((()())()(()))((())())((()(())((()))(((())))()()))()) + (((()()()())()()()(())()())((()(())((()))(((())))()()))()) + (()()())(()()()) + (()(())()()()(())) + (((()()()(())()())((()())()())())((((()))))(((())))((()))(())()) + (()()()()()())(()()()()()()()()) + (()()(())()()())(()()()(())()()) + (()(())()(())) + ((()()())(()()())()()()(())()()) + ((()())((()(())((()))(((())))()()))()(()(())((()))(((())))((((())))))) + ((()(())()(()()()))((()())()())()) + ((()(())()()()())()(((()()())()())()())) + ((()()())()(())(())) + ((()(((()()())()())()()))()(())((()))(((())))((((()))))) + ((()())(()()()()()()()())()(((()()())()())()())) + (((()())()(()))(()()()(())()())((()(())((()))(((())))()()))()) + (((()()())()(((()()())()())()()))()(())((()))(((())))((((()))))) + (()()(()))((())())(()()()(())()()) = ???
> ((((()(())()()())(()()()(())()())(()(())(()()()()()()()()))()(((()()())()())()()))((())()()())()(())((()))(((())))((((())))))(((()())()(((()()())()())()()))((((()))))(((())))((()))(())()(()()(()))(()()()(())()()))(((()())()(())(()))((((()))))(((())))((()))(())()(()()()())(()()()()()()()()))(((()(()))((()())()())())((((()))))(((())))((()))(())())((()()(((()()())()())()()))()(())((()))(((())))((((())))))(()(())())((()(())((()))(((())))()()))()(((((()))))(((())))((()))(())())((()())(()()()()()()()())((()())()())())((()()()()()())()()()(())()())((()()()())((((()))))(((())))((()))(())())((()(())()()()())()(((()()())()())()()))(()()((((()))))(((())))((()))(())())((()())()(())))(((()()()()()()()()())((()())()())())((()(())((()))(((())))()()))()((()(())()(()))()(((()()())()())()()))(()(()))((())()()())(()()()()()(())((()))(((())))((((())))))(((((()))))(((())))((()))(())())(()()()()())(()()()))(((()())(())()()())((()(())((()))(((())))()()))())((()())((()(())((()))(((())))()()))())(((()()())()(()))((()(())((()))(((())))()()))()(()()()(((()()())()())()()))((()()()((()())()())())()(((()()())()())()()))(()()(())()()())((()(())()()()())()(((()()())()())()())))((()()())((()(())((()))(((())))()()))()(((((()))))(((())))((()))(())())((())()()()()))((()()()()(((()()())()())()()))((()(())((()))(((())))()()))()(()()()()()()))(((()())()(()))((())())((()(())((()))(((())))()()))())(((()()()())()()()(())()())((()(())((()))(((())))()()))()(()()())(()()())(()(())()()()(()))(((()()()(())()())((()())()())())((((()))))(((())))((()))(())())(()()()()()())(()()()()()()()())(()()(())()()())(()()()(())()())(()(())()(()))((()()())(()()())()()()(())()()))((()())((()(())((()))(((())))()()))()(()(())((()))(((())))((((())))))((()(())()(()()()))((()())()())())((()(())()()()())()(((()()())()())()()))((()()())()(())(()))((()(((()()())()())()()))()(())((()))(((())))((((())))))((()())(()()()()()()()())()(((()()())()())()())))(((()())()(()))(()()()(())()())((()(())((()))(((())))()()))()(((()()())()(((()()())()())()()))()(())((()))(((())))((((())))))(()()(()))((())())(()()()(())()()))
Correct!

Congratulations, here's your flag: picoCTF{5cr1pt1nG_l1k3_4_pRo_0466cdd7}
[*] Closed connection to 2018shell.picoctf.com port 1542

</pre>
最後はめっちゃ長いのが来た！

flag `picoCTF{5cr1pt1nG_l1k3_4_pRo_0466cdd7}`





<br /><br />
<br /><br />
## [Forensics]: LoadSomeBits (550 points)
- - -
### Challenge
> Can you find the flag encoded inside this image?
<br /><br />
Hint1: Look through the Least Significant Bits for the image<br />
Hint2: If you interpret a binary sequence (seq) as ascii and then try interpreting the same binary sequence from an offset of 1 (seq[1:]) as ascii do you get something similar or completely different?

Attachment:

- pico2018-special-logo.bmp


<br />

### Solution
BMPファイルだったので、「青い空を見上げればいつもそこに白い猫」が使えなかったし、ちょっと意味不明な部分もあったので、他の方のWriteupを少し参照しました。

「青い空を見上げればいつもそこに白い猫」で解いているWriteupもあったので、目からウロコでした。

<br />
他のWriteupを見たところ、C言語で解いているWriteupは無かったので、Cで書いてみました。
```C
#include <stdio.h>

int sub( int offset )
{
	FILE *fp;
	int  c;
	int i;
	char ch;

	if ( ( fp = fopen( "pico2018-special-logo.bmp", "rb" ) ) == NULL ) {
		perror( "Can't fopen" );
		return -1;
        }
	for ( i = 0 ; i < offset ; i++ ) {
		fgetc( fp );
	}

	i = 7;
	ch = 0;
	while ( ( c = fgetc( fp ) ) != EOF ) {
		c = c & 0x01;
		ch = ch | ( c << i);
		i--;
		if ( i < 0 ) {
			if ( ( ch >= 0x20 ) && (ch <= 0x7e ) ) {
				printf( "%c", ch );
			}
			i = 7;
			ch = 0;
		}
	}
	puts("");
        fclose( fp );
	
	return 0;
}

int main( void ) {
	int i;
	for ( i = 0 ; i < 8 ; i++ ) {
		sub( i );
	}
}
```

どこをデータの先頭にするかで結果が違うので、最初main()で書いてたやつを途中でsub()に変えて、offset処理を後付けしました。。

以下が実行結果の一部です。

DpicoCTF{st0r3d_iN_tH3_l345t_s1gn1f1c4nT_b1t5_770554193}~8?p???q???p??????????????????????????????????????????????????pp?????q8q8??8p?888ppqp?????8?8??????8??8???????????p?????????q8?p??????8???

Flag: `picoCTF{st0r3d_iN_tH3_l345t_s1gn1f1c4nT_b1t5_770554193}`





<br /><br />
<br /><br />
## [Reversing]: keygen-me-2 (750 points)
- - -
### Challenge
> The software has been updated. Can you find us a new product key for the program in /problems/keygen-me-2_1_762036cde49fef79146a706d0eda80a3
<br /><br />
Hint: z3

Attachment:

- activate


<br />

### Solution
Ghidra使って解析していきます。keygen-me-1がベースなので、以下の辺りは同じです。

- 16バイトのキーを入れる。
- 正しいキーを入れたら、flag.txtが表示される。
- valid charは、[0-9A-Z]

今回ポイントとなる箇所は、ここ。
```C
undefined4 validate_key(char *param_1)

{
  char cVar1;
  size_t sVar2;
  
  sVar2 = strlen(param_1);
  cVar1 = key_constraint_01(param_1,sVar2);
  if (((((cVar1 != 0) && (cVar1 = key_constraint_02(param_1,sVar2), cVar1 != 0)) &&
       (cVar1 = key_constraint_03(param_1,sVar2), cVar1 != 0)) &&
      ((((cVar1 = key_constraint_04(param_1,sVar2), cVar1 != 0 &&
         (cVar1 = key_constraint_05(param_1,sVar2), cVar1 != 0)) &&
        ((cVar1 = key_constraint_06(param_1,sVar2), cVar1 != 0 &&
         ((cVar1 = key_constraint_07(param_1,sVar2), cVar1 != 0 &&
          (cVar1 = key_constraint_08(param_1,sVar2), cVar1 != 0)))))) &&
       (cVar1 = key_constraint_09(param_1,sVar2), cVar1 != 0)))) &&
     (((cVar1 = key_constraint_10(param_1,sVar2), cVar1 != 0 &&
       (cVar1 = key_constraint_11(param_1,sVar2), cVar1 != 0)) &&
      (cVar1 = key_constraint_12(param_1,sVar2), cVar1 != 0)))) {
    return 1;
  }
  return 0;
}
```

<br />
全部載せると多いので、2つくらい載せます。Ghidraのデコンパイルの特徴なのか、returnのところで判定文が出てきます。コードの読み方は、その下に記載しています。

```C
uint key_constraint_01(char *param_1)

{
  char cVar1;
  char cVar2;
  uint uVar3;
  
  cVar1 = ord((int)*param_1);
  cVar2 = ord((int)param_1[1]);
  uVar3 = mod((int)cVar2 + (int)cVar1,0x24);
  return uVar3 & 0xffffff00 | (uint)(uVar3 == 0xe);
}
```
==> (param_1[0] + param_1[1]) mod 0x24 = 0xE


<br />
```C
uint key_constraint_02(int param_1)

{
  char cVar1;
  char cVar2;
  uint uVar3;
  
  cVar1 = ord((int)*(char *)(param_1 + 2));
  cVar2 = ord((int)*(char *)(param_1 + 3));
  uVar3 = mod((int)cVar2 + (int)cVar1,0x24);
  return uVar3 & 0xffffff00 | (uint)(uVar3 == 0x18);
}
```
==> (param_1[2] + param_1[3]) mod 0x24 = 0x18


<br />
まとめ。こんな感じで全部見ていくと、それぞれの判定文は以下の通りです。（"param_1"を省略してます）<br />
==> ([0] + [1]) mod 0x24 = 0xE<br />
==> ([2] + [3]) mod 0x24 = 0x18<br />
==> ([2] - [0]) mod 0x24 = 0x6<br />
==> ([1] + [3] + [5]) mod 0x24 = 0x4<br />
==> ([2] + [4] + [6]) mod 0x24 = 0xd<br />
==> ([3] + [4] + [5]) mod 0x24 = 0x16<br />
==> ([6] + [8] + [10]) mod 0x24 = 0x1f<br />
==> ([1] + [4] + [7]) mod 0x24 = 0x7<br />
==> ([9] + [12] + [15]) mod 0x24 = 0x14<br />
==> ([13] + [14] + [15]) mod 0x24 = 0xc<br />
==> ([8] + [9] + [10]) mod 0x24 = 0x1b<br />
==> ([7] + [12] + [13]) mod 0x24 = 0x17<br />


<br />
ちなみに、上記に出てくるord()関数は、別途独自定義されていて、以下のようになってます。
```C
int ord(byte param_1)

{
  int iVar1;
  
  if (((char)param_1 < '0') || ('9' < (char)param_1)) {
    if (((char)param_1 < 'A') || ('Z' < (char)param_1)) {
      puts("Found Invalid Character!");
                    /* WARNING: Subroutine does not return */
      exit(0);
    }
    iVar1 = (uint)param_1 - 0x37;
  }
  else {
    iVar1 = (uint)param_1 - 0x30;
  }
  return iVar1;
}
```

最初、これを完全に見逃していて、相当悩みました。<br />

gdbで追っかけて行ったときに、ord()を読んだ後に、レジスタの値が予想外に変わる所まで見てたんですが、別途定義されているという発想が全くなかったです。。。凹○ コテッ



<br />
以下は、そのord()も加味した解法です。（pico_ord()の部分を後付しました。）

文字の範囲も限られているので、Brute Forceでやりました。
```C
#include <stdio.h>

#define CHAR_0  48
#define CHAR_9  57
#define CHAR_A  65
#define CHAR_Z  90

void print_flag( int *p ) {
	int i;
	char flag[16];
	for ( i = 0 ; i < 16 ; i++ ) {
		printf( "%c", (char)*( p + i ) );
	}
	puts("");

}

int validate_key( int p )
{
	if ( p > CHAR_9 && p < CHAR_A ) {
		return 1;
	} else {
		return 0;
	}
}

int pico_ord( int p )
{
	if ( p <= CHAR_9 ) {
		return p - 0x30;
	} else {
		return p - 0x37;
	}
}

int main( void )
{
	int p[16];
	p[11] = CHAR_0;

	//==> ([0] + [1]) mod 0x24 = 0xE
	for ( p[0] = CHAR_0 ; p[0] <= CHAR_Z ; p[0]++ ) {
		if ( validate_key( p[0] ) ) {
			continue;
		}
		for ( p[1] = CHAR_0 ; p[1] <= CHAR_Z ; p[1]++ ) {
			if ( validate_key( p[1] ) ) {
				continue;
			}
			if ( ( pico_ord( p[0] ) + pico_ord( p[1] ) ) % 0x24 == 0xE ) {
				break;
			}
		}
		if ( p[1] > CHAR_Z ) {
			continue;
		}

		//==> ([2] + [3]) mod 0x24 = 0x18
		for ( p[2] = CHAR_0 ; p[2] <= CHAR_Z ; p[2]++ ) {
			if ( validate_key( p[2] ) ) {
				continue;
			}
			for ( p[3] = CHAR_0 ; p[3] <= CHAR_Z ; p[3]++ ) {
				if ( validate_key( p[3] ) ) {
					continue;
				}
				if ( ( pico_ord( p[2] ) + pico_ord( p[3] ) ) % 0x24 == 0x18 ) {
					break;
				}
			}
			if ( p[3] > CHAR_Z ) {
				continue;
			}

			//==> ([2] - [0]) mod 0x24 = 0x6
			if ( ( pico_ord( p[2] ) - pico_ord( p[0] ) ) % 0x24 == 0x6 ) {
				
				//==> ([1] + [3] + [5]) mod 0x24 = 0x4
				for ( p[5] = CHAR_0 ; p[5] <= CHAR_Z ; p[5]++ ) {
					if ( validate_key( p[5] ) ) {
						continue;
					}
					if ( ( pico_ord( p[1] ) + pico_ord( p[3] ) + pico_ord( p[5] ) ) % 0x24 == 0x4 ) {
						break;
					}
				}
				if ( p[5] > CHAR_Z ) {
					continue;
				}
				//==> ([2] + [4] + [6]) mod 0x24 = 0xd
				for ( p[4] = CHAR_0 ; p[4] <= CHAR_Z ; p[4]++ ) {
					if ( validate_key( p[4] ) ) {
						continue;
					}
					for ( p[6] = CHAR_0 ; p[6] <= CHAR_Z ; p[6]++ ) {
						if ( validate_key( p[6] ) ) {
							continue;
						}
						if ( ( pico_ord( p[2] ) + pico_ord( p[4] ) + pico_ord( p[6] ) ) % 0x24 == 0xd ) {
							break;
						}
					}
					if ( p[6] > CHAR_Z ) {
						continue;
					}

					//==> ([3] + [4] + [5]) mod 0x24 = 0x16
					if ( ( pico_ord( p[3] ) + pico_ord( p[4] ) + pico_ord( p[5] ) ) % 0x24 == 0x16 ) {

						//==> ([6] + [8] + [10]) mod 0x24 = 0x1f
						for ( p[8] = CHAR_0 ; p[8] <= CHAR_Z ; p[8]++ ) {
							if ( validate_key( p[8] ) ) {
								continue;
							}
							for ( p[10] = CHAR_0 ; p[10] <= CHAR_Z ; p[10]++ ) {
								if ( validate_key( p[10] ) ) {
									continue;
								}
								if ( ( pico_ord( p[6] ) + pico_ord( p[8] ) + pico_ord( p[10] ) ) % 0x24 == 0x1f ) {
									break;
								}
							}
							if ( p[10] > CHAR_Z ) {
								continue;
							}
							// ==> ([1] + [4] + [7]) mod 0x24 = 0x7
							for ( p[7] = CHAR_0 ; p[7] <= CHAR_Z ; p[7]++ ) {
								if ( validate_key( p[7] ) ) {
									continue;
								}
								if ( ( pico_ord( p[1] ) + pico_ord( p[4] ) + pico_ord( p[7] ) ) % 0x24 == 0x7 ) {
									break;
								}
							}
							if ( p[7] > CHAR_Z ) {
								continue;
							}
							// ==> ([9] + [12] + [15]) mod 0x24 = 0x14
							for ( p[9] = CHAR_0 ; p[9] <= CHAR_Z ; p[9]++ ) {
								if ( validate_key( p[9] ) ) {
									continue;
								}
								for ( p[12] = CHAR_0 ; p[12] <= CHAR_Z ; p[12]++ ) {
									if ( validate_key( p[12] ) ) {
										continue;
									}
									for ( p[15] = CHAR_0 ; p[15] <= CHAR_Z ; p[15]++ ) {
										if ( validate_key( p[15] ) ) {
											continue;
										}
										if ( ( pico_ord( p[9] ) + pico_ord( p[12] ) + pico_ord( p[15] ) ) % 0x24 == 0x14 ) {
											break;
										}
									}
									if ( p[15] > CHAR_Z ) {
										continue;
									}
									// ==> ([13] + [14] + [15]) mod 0x24 = 0xc
									for ( p[13] = CHAR_0 ; p[13] <= CHAR_Z ; p[13]++ ) {
										if ( validate_key( p[13] ) ) {
											continue;
										}
										for ( p[14] = CHAR_0 ; p[14] <= CHAR_Z ; p[14]++ ) {
											if ( validate_key( p[14] ) ) {
												continue;
											}
											if ( ( pico_ord( p[13] ) + pico_ord( p[14] ) + pico_ord( p[15] ) ) % 0x24 == 0xc ) {
												break;
											}
										}
										if ( p[14] > CHAR_Z ) {
											continue;
										}
										// ==> ([8] + [9] + [10]) mod 0x24 = 0x1b
										if ( ( pico_ord( p[8] ) + pico_ord( p[9] ) + pico_ord( p[10] ) ) % 0x24 == 0x1b ) {
											// ==> ([7] + [12] + [13]) mod 0x24 = 0x17
											if ( ( pico_ord( p[7] ) + pico_ord( p[12] ) + pico_ord( p[13] ) ) % 0x24 == 0x17 ) {
												print_flag( p );
												return 0;
											}
										}
									}
								}
							}
						}
					}
				}
			}
		}
	}
}
```

<pre>
captureamerica@pico-2018-shell:/problems/keygen-me-2_1_762036cde49fef79146a706d0eda80a3$ ./activate 0E6IW8BX07K00Q9D
Product Activated Successfully: picoCTF{c0n5tr41nt_50lv1nG_15_W4y_f45t3r_3846045707}
</pre>

Flag: `picoCTF{c0n5tr41nt_50lv1nG_15_W4y_f45t3r_3846045707}`



<br /><br />
<br /><br />
## [Reversing]: be-quick-or-be-dead-3 (350 points)
- - -
### Challenge
> As the [song](https://www.youtube.com/watch?v=CTt1vk9nM9c) draws closer to the end, another executable be-quick-or-be-dead-3 suddenly pops up. This one requires even faster machines. Can you run it fast enough too?
<br /><br />
Hint: How do you speed up a very repetitive computation?

Attachment:

- be-quick-or-be-dead-3  (ELF 64-bit)

<br />

### Solution
まず、Ghidraにかけます。

以下の箇所が再帰関数になっており、時間がかかるところです。
```C
ulong calc(uint uParm1)

{
  int iVar1;
  int iVar2;
  int iVar3;
  int iVar4;
  int iVar5;
  uint local_1c;
  
  if (uParm1 < 5) {
    local_1c = uParm1 * uParm1 + 0x2345;
  }
  else {
    iVar1 = calc((ulong)(uParm1 - 1));
    iVar2 = calc((ulong)(uParm1 - 2));
    iVar3 = calc((ulong)(uParm1 - 3));
    iVar4 = calc((ulong)(uParm1 - 4));
    iVar5 = calc((ulong)(uParm1 - 5));
    local_1c = iVar5 * 0x1234 + (iVar1 - iVar2) + (iVar3 - iVar4);
  }
  return (ulong)local_1c;
}
```

ヒントにしたがって、"再帰関数 高速化" でググってみると、「メモ化」というのがありました。

"一度処理した結果を保存しておく方法を「メモ化」といい、保存する量が多くなってくるとメモリの使用量は増えますが、処理が圧倒的に高速になります。" とのことです。

Rubyでできるようなので、普段Rubyは使わないのですが、ウェブで見つけたサンプルをならって以下を書きました。
```Ruby
@memo = {}
def calc(a)
  return @memo[[a]] if @memo[[a]]
  if a < 5 then
    ret = a*a+0x2345
  else
    ret = calc(a-5)*0x1234 + (calc(a-1)-calc(a-2)) +(calc(a-3)-calc(a-4))
  end
  @memo[[a]] = ret
  return ret
end
printf("0x%x\n", calc(0x18f4b) & 0xFFFFFFFF )
```

<br />
最初に実行した際に、以下のエラーが出ました。

<pre>
$ ruby be-quick-or-be-dead-3_solve.rb 
be-quick-or-be-dead-3_solve.rb:7:in `calc': stack level too deep (SystemStackError)
</pre>


<br />
これは、RUBY_THREAD_VM_STACK_SIZEという環境変数で回避できるそうなので、テキトーに100MBでやってみました。

<pre>
$ export RUBY_THREAD_VM_STACK_SIZE=100000000
$ ruby be-quick-or-be-dead-3_solve.rb 
0x2f8cdc3f
</pre>

<br />
gdbでcalc()をコールする直前でbreakして、戻り値をセットして、jumpでcalc()をスキップします。

```C
gef➤  i b
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x000000000040079b <calculate_key+9>
	breakpoint already hit 1 time

gef➤  disas calculate_key
Dump of assembler code for function calculate_key:
   0x0000000000400792 <+0>:	push   rbp
   0x0000000000400793 <+1>:	mov    rbp,rsp
   0x0000000000400796 <+4>:	mov    edi,0x18f4b
=> 0x000000000040079b <+9>:	call   0x400706 <calc>
   0x00000000004007a0 <+14>:	pop    rbp
   0x00000000004007a1 <+15>:	ret    
End of assembler dump.

gef➤  set $eax=0x2f8cdc3f

gef➤  jump *0x00000000004007a0
Continuing at 0x4007a0.

Program received signal SIGALRM, Alarm clock.
Done calculating key
Printing flag:
picoCTF{dynamic_pr0gramming_ftw_22ac7d81}
[Inferior 1 (process 1492) exited normally]
```

Flag: `picoCTF{dynamic_pr0gramming_ftw_22ac7d81}`

<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

