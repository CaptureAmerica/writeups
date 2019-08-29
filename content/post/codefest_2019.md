---
title: "Codefest'19 CTF Writeup"
date: 2019-08-25T15:00:00+09:00
lastmod: 2019-08-29T23:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
URL: [https://www.hackerrank.com/codefest19-ctf](https://www.hackerrank.com/codefest19-ctf)
<br /><br />
問題数は10問（WelcomeとSurveyを除く）あって、そのうちの1問（Mail capture）は添付ファイルにフラグがそのまま入っているという（おそらく）手違いがあり、実質9問だけでした。
<br /><br />
その9問中の1問を解きました。
<br /><br />
（後日、残りの問題の復習もやりました。）


<br /><br />
# Reversing: Linux RE 2
- - -
## Challenge
> You are given yet another ELF file. Reverse it to find the flag. The password is the flag.
<br /><br />
Update: The flag contains only alphabets, numbers and underscore.

Attachment:

- chall2


<br />
## Solution
Ghidraでデコンパイルします。

```C
undefined8 main(void)

{
  size_t sVar1;
  long in_FS_OFFSET;
  char local_78;
  char local_77;
  char local_76;
  char local_75;
  char local_74;
  char local_73;
  char local_72;
  char local_71;
  char local_70;
  char local_6f;
  byte local_6e;
  char local_6d;
  byte local_6c;
  char local_6b;
  byte local_6a;
  char local_69;
  char local_68;
  byte local_67;
  char local_66;
  byte local_65;
  char local_64;
  byte local_63;
  char local_62;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  __isoc99_scanf(&DAT_00400a88,&local_78);
  sVar1 = strlen(&local_78);
  if (sVar1 == 0x17) {
    if (local_74 == 'l') {
      if ((int)local_75 + 1 == (int)local_72) {
        if ((int)local_73 + 1 == (int)local_71) {
          if (local_71 == 'e') {
            if (local_75 == 'u') {
              if (local_76 == 'o') {
                if (local_78 == 's') {
                  if (local_77 == 'h') {
                    if (local_70 == '_') {
                      if ((int)(char)local_6c + 1 == (int)local_6d) {
                        if ((int)(char)local_6e + 2 == (int)local_6f) {
                          if ((local_6c ^ local_6e) == 0x17) {
                            if (local_6c == 100) {
                              if (local_6b == '_') {
                                if ((int)(char)local_67 + 8 == (int)local_68) {
                                  if ((int)local_69 + -2 == (int)local_68) {
                                    if ((local_67 ^ local_6a) == 0x16) {
                                      if (local_68 == 'm') {
                                        if (local_66 == '_') {
                                          if ((int)local_62 + 3 == (int)local_64) {
                                            if ((int)local_64 + 0x10 == (int)(char)local_63 + 0x10)
                                            {
                                              if ((local_65 ^ local_63) == 0x1b) {
                                                if (local_62 == 'l') {
                                                  puts("Congratulations! Correct password!");
                                                }
                                                else {
                                                  puts("Sorry! Wrong password!");
                                                }
                                              }
                                              else {
                                                puts("Sorry! Wrong password!");
                                              }
                                            }
                                            else {
                                              puts("Sorry! Wrong password!");
                                            }
                                          }
                                          else {
                                            puts("Sorry! Wrong password!");
                                          }
                                        }
                                        else {
                                          puts("Sorry! Wrong password!");
                                        }
                                      }
                                      else {
                                        puts("Sorry! Wrong password!");
                                      }
                                    }
                                    else {
                                      puts("Sorry! Wrong password!");
                                    }
                                  }
                                  else {
                                    puts("Sorry! Wrong password!");
                                  }
                                }
                                else {
                                  puts("Sorry! Wrong password!");
                                }
                              }
                              else {
                                puts("Sorry! Wrong password!");
                              }
                            }
                            else {
                              puts("Sorry! Wrong password!");
                            }
                          }
                          else {
                            puts("Sorry! Wrong password!");
                          }
                        }
                        else {
                          puts("Sorry! Wrong password!");
                        }
                      }
                      else {
                        puts("Sorry! Wrong password!");
                      }
                    }
                    else {
                      puts("Sorry! Wrong password!");
                    }
                  }
                  else {
                    puts("Sorry! Wrong password!");
                  }
                }
                else {
                  puts("Sorry! Wrong password!");
                }
              }
              else {
                puts("Sorry! Wrong password!");
              }
            }
            else {
              puts("Sorry! Wrong password!");
            }
          }
          else {
            puts("Sorry! Wrong password!");
          }
        }
        else {
          puts("Sorry! Wrong password!");
        }
      }
      else {
        puts("Sorry! Wrong password!");
      }
    }
    else {
      puts("Sorry! Wrong password!");
    }
  }
  else {
    puts("Sorry! Wrong password!");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

変なバッファの確保の仕方がされていて、scanfにlocal_78（char型）のポインタを渡していて、ここに入力したパスワードが入ってきます。

他のローカル変数を上書きするのを期待してるようなコードです。

入力文字列（パスワード）の長さは、23（0x17）文字です。

if文もそんなに複雑ではないので、順番にパズルを解いていく感じで簡単に解けます。

<pre>
local_78: s
local_77: h
local_76: o
local_75: u
local_74: l
local_73は、local_71-1: d
local_72はlocal_75(u)+1なので: v
local_71: e
local_70: _
local_6fはlocal_6e+2: u
local_6eはlocal_6c^0x17: s
local_6dはlocal_6c+1: e
local_6cは100なので、: d
local_6b: _
local_6aは、local_67^0x16: s
local_69はlocal_68+2: o
local_68: m
local_67はlocal_68-8: e
local_66: _
local_65は^local_63が0x1b: t
local_64はlocal_62(l)+3: o
local_63はlocal_64と同じ: o (111)
local_62: l
</pre>

Flag: `CodefestCTF{shouldve_used_some_tool}`



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後に行った復習です。他の方のWriteupを参照しました。
<br /><br />
何気にいろいろと勉強になりました。
<br /><br />
<br /><br />

# What language is this?
- - -
## Challenge
> Decode this piece of text that Alice got from a friend of hers. 
<br /><br />
iiisdsiiioiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiodddddddddddoioiodoiiiiiiiiiiiiiioiodddddddddddddddddddddddddddddddddddddddddddddddddoiiiiiiiiiiiiiiiiioddddddddddddddoiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiioddddddddddddddddddddddddddddddddddddoiiiiiiiiiiiiiioiiiiiiiodddddddddodddddddddddddddddddddddddddddddddddddddddddddddddddoddddddddddddddddddddddddddddddddddddddsiiiiiiiiioddddddddoddddddoiiiiiiiiiiiiiiiiiiiiioddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddoddddddddddddddddddddddddddddddddsiiisisdddddoddddddddddddddddddddddddddddodddddddddddddddddddoddddddddddddddddddddddddddddddddsiiisisoioiodoiiiiiiiiiiiiiioiodddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddoiiiiiiiioddddddddddddddddddddddddddddddddddddddddddddddsiiiio

<br />
## Solution
i, s, d, oの4文字を使ったesolangだろうということで、いろいろググったんですが、何なのかわかんなかったんですよね。

deadfishというやつらしいです。覚えておきます。



<br /><br />
<br /><br />
# Gibberish file
- - -
## Challenge
> This file seems to have a weird encoding. Reverse it to find the flag.

Attachment:

- output.txt (weird_file.zip)


<br />
## Solution

"Reverse it" を、"データを反転せよ" と解釈するのがミソ。そういう発想が大事なのがわかりました。



<br /><br />
<br /><br />
# Image Corruption
- - -
## Challenge
> You are given a corrupted file. This file has the flag. Find it.

Attachment:

- image.bmp

<br />
## Solution

このファイル、一部バイナリだけど、ほとんどテキストでmatrixmatrixmatrixがいっぱい出てくるところまでは見れていたんですが、そこからXORしようという発想がなかったです。

一応、コードはそれなりに自分で書いてみました。

```Python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import struct

infile = open("image.bmp", "rb")
outfile = open("image_out.bmp", "wb")

data = infile.read()

for i in range(len(data)//6):
    outfile.write(struct.pack("B", data[6*i+0] ^ ord('m')))
    outfile.write(struct.pack("B", data[6*i+1] ^ ord('a')))
    outfile.write(struct.pack("B", data[6*i+2] ^ ord('t')))
    outfile.write(struct.pack("B", data[6*i+3] ^ ord('r')))
    outfile.write(struct.pack("B", data[6*i+4] ^ ord('i')))
    outfile.write(struct.pack("B", data[6*i+5] ^ ord('x')))

infile.close()
outfile.close()
```


<br /><br />
<br /><br />
# Mail capture
- - -
## Challenge
> Bob got hold of this file when going through the files of the email client on his old computer. Help him find the hidden message.

Attachment: （後日ダウンロードし直したら、ちゃんと問題用ファイルが入ってました。）

- encoded_file

```
$ cat encoded_file
begin 664 flag_encoded
E0V]D969E<W1#5$9[-V@Q-5\Q-5\T7V,P,#%?,VYC,&0Q;CE]"@``
`
end
```

<br />
## Solution

$ uudecode encoded_file

これもちゃんと覚えておきます。


<br /><br />
<br /><br />
# Cats are innocent, right?
- - -
## Challenge
> This image has a flag hidden in it. Find it.

Attachment:

- cute_kittens.jpg

<br />
## Solution

この問題だけSuccess Rateがダントツに低いんです（4.76%）。みんな解けなかったみたいですね。

[stegify](https://github.com/DimitarPetrov/stegify)というツールと使って解くそうです。

それ以外の解き方はないのかなぁ。。


<br />
しかも、出力されたZipにはDecoyが入っているし。

Zipを解凍せずにバイナリで見るとか、面白いと思いました。

```
root@kali:~/Codefest_CTF_2019# stegify -op decode -carrier cute_kittens.jpg -result hello
root@kali:~/Codefest_CTF_2019# ll
total 204
drwxr-xr-x  2 root root   4096 Aug 29 22:40 ./
drwxr-xr-x 50 root root   4096 Aug 29 22:39 ../
-rw-r--r--  1 root root 193231 Aug 29 22:26 cute_kittens.jpg
-rw-r--r--  1 root root    192 Aug 29 22:40 hello
root@kali:~/Codefest_CTF_2019# file hello 
hello: Zip archive data, at least v?[0x30a] to extract
root@kali:~/Codefest_CTF_2019# zipinfo hello
Archive:  hello
Zip file size: 192 bytes, number of entries: 1
-rw-rw-r--  6.3 unx       16 bx stor 19-Aug-14 16:36 is this the flag?
1 file, 16 bytes uncompressed, 16 bytes compressed:  0.0%
root@kali:~/Codefest_CTF_2019# 
root@kali:~/Codefest_CTF_2019# 
root@kali:~/Codefest_CTF_2019# unzip hello
Archive:  hello
 extracting: is this the flag?       
root@kali:~/Codefest_CTF_2019# ll
total 208
drwxr-xr-x  2 root root   4096 Aug 29 22:40  ./
drwxr-xr-x 50 root root   4096 Aug 29 22:39  ../
-rw-r--r--  1 root root 193231 Aug 29 22:26  cute_kittens.jpg
-rw-r--r--  1 root root    192 Aug 29 22:40  hello
-rw-rw-r--  1 root root     16 Aug 14 16:36 'is this the flag?'
root@kali:~/Codefest_CTF_2019# emacs
root@kali:~/Codefest_CTF_2019# 
root@kali:~/Codefest_CTF_2019# 
root@kali:~/Codefest_CTF_2019# xxd hello 
00000000: 504b 0304 0a03 0000 0000 9984 0e4f 7826  PK...........Ox&
00000010: 3bec 1000 0000 1000 0000 1100 0000 6973  ;.............is
00000020: 2074 6869 7320 7468 6520 666c 6167 3f4e   this the flag?N
00000030: 6f70 652c 206e 6f74 2068 6572 652e 0a50  ope, not here..P
00000040: 4b01 023f 030a 0300 0000 0099 840e 4f78  K..?..........Ox
00000050: 263b ec10 0000 0010 0000 0011 0000 0000  &;..............
00000060: 0000 0000 0020 80b4 8100 0000 0069 7320  ..... .......is 
00000070: 7468 6973 2074 6865 2066 6c61 673f 504b  this the flag?PK
00000080: 0506 0000 0000 0100 0100 3f00 0000 3f00  ..........?...?.
00000090: 0000 0000 436f 6465 6665 7374 4354 467b  ....CodefestCTF{
000000a0: 6831 6431 6e67 5f62 3368 316e 645f 316e  h1d1ng_b3h1nd_1n
000000b0: 6e30 6333 6e74 5f6b 3174 7433 6e35 7d0a  n0c3nt_k1tt3n5}.
```

stegifyは、これからのCTFで使っていこうと思います。


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

