---
title: "Codefest'19 CTF Writeup"
date: 2019-08-25T15:00:00+09:00
lastmod: 2019-08-25T15:00:00+09:00
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
- - -
<br /><br />
<br /><br />

