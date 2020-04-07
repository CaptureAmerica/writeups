---
title: "Cryptix CTF 2019 Writeup"
date: 2019-10-14T17:00:00+09:00
lastmod: 2019-10-14T17:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fcryptix_ctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://cryptixctf.com/challenges](https://cryptixctf.com/challenges)
<br /><br />
上位に賞金が出るやつでしたけど、参加者が300チーム弱と、意外と狙い目のCTFだったかもです。

ただし、全問正解しないとダメですけどね。

<img src="https://captureamerica.github.io/writeups/img/cryptix_Score.png" alt="cryptix_Score.png">



<br /><br />
## [Forensics]: Hidden deep within (400 points)
- - -
### Challenge
> "This is just noise... There is nothing...."

Attachments:

- useless.png

<br />
### Solution
「青い空を見上げればいつもそこに白い猫」でLSB立てたらPNGのヘッダが見えたので、バイナリデータ保存したところ、"flag{Haha_fake_flag}" と書かれたPNGファイルになりました。

そのPNGに対してZstegかけたらフラグが取れました。

Flag: `flag{st3g4n0gr4phy_i5_34sy}`



<br /><br />
<br /><br />
## [Web]: Your ID please
- - -
### Challenge
> This is super secure, confidential research. You are just not meant to access it. Don't even try, it's futile.<br />
Okay, you don't believe me? have the source code too!<br />
https://cryptixctf.com/web4/php_code.txt<br />
<br />
https://cryptixctf.com/web4

Attachments:

- php_code.txt

```Python
include_once 'flag.php';

if($_SERVER["REQUEST_METHOD"] == "POST"){
    if(isset($_POST["ID"])&&isset($_POST["pwd"])){
        if(strcmp($secretpassphrase, $_POST["pwd"]) == 0){
            echo "Hey, you are in!  " . $_POST["ID"]  . "<br>";
            if($_POST["ID"] == "SuperUser1337"){
                echo "Your Flag: " . $flag;
            }
        }else{
            echo "<script type='text/javascript'>alert('Unable to Login');</script>";
        }
    }
}
```

<br />
### Solution
Burpでpwdのところをpwd[]に書き換えてフォワードします。

ID="SuperUser1337"&pwd[]=pass

Flag: `flag{Why_Juggl3_th3_Typ5}`





<br /><br />
<br /><br />
## [Reversing]: Let's climb the ladder (250 points)
- - -
### Challenge
> Here is an executable. You know what to do.<br />
Note: The flag format is as usual flag{XXXX...}<br />

Attachments:

- passphrase (ELF 64-bit)


<br />
### Solution (Unsolved...)
Ghidraでデコンパイルします。

```C
void check(char *pcParm1)

{
  size_t sVar1;
  
  sVar1 = strlen(pcParm1);
  if ((int)sVar1 == 0x15) {
    if ((int)pcParm1[7] == (uint)(pcParm1[0x10] == 0)) {
      fail();
    }
    else {
      if ((int)pcParm1[1] == (uint)(pcParm1[0xb] == 0)) {
        fail();
      }
      else {
        if ((int)pcParm1[2] == (uint)(pcParm1[8] == 0)) {
          fail();
        }
        else {
          if ((int)pcParm1[8] == (uint)(pcParm1[0x12] == 0)) {
            fail();
          }
          else {
            if ((int)pcParm1[3] == (uint)(pcParm1[0x11] == 0)) {
              fail();
            }
            else {
              if ((int)pcParm1[5] == (uint)(pcParm1[0x14] == 0)) {
                fail();
              }
              else {
                if ((int)pcParm1[9] == (uint)(pcParm1[10] == 0)) {
                  fail();
                }
                else {
                  if ((int)pcParm1[0xc] == (uint)(pcParm1[0x13] == 0)) {
                    fail();
                  }
                  else {
                    if (*pcParm1 == 'r') {
                      if ((int)pcParm1[2] - (int)pcParm1[1] == 1) {
                        if ((int)pcParm1[10] - (int)pcParm1[8] == 1) {
                          if (pcParm1[2] == '4') {
                            if ((int)*pcParm1 + (int)pcParm1[3] == 0xd6) {
                              if ((int)pcParm1[4] - (int)pcParm1[3] == 5) {
                                if ((int)pcParm1[6] + (int)pcParm1[5] == 0xd5) {
                                  if ((int)pcParm1[5] - (int)pcParm1[6] == 7) {
                                    if ((int)pcParm1[7] - (int)pcParm1[8] == 0x2b) {
                                      if ((int)pcParm1[0xd] + (int)pcParm1[0xc] == 0xcf) {
                                        if ((int)pcParm1[0xd] * (int)pcParm1[0xc] == 0x29ba) {
                                          if ((int)pcParm1[0xf] - (int)pcParm1[0xe] == 0xd) {
                                            if ((int)pcParm1[0xf] - (int)*pcParm1 == 7) {
                                              success();
                                            }
                                            else {
                                              fail();
                                            }
                                          }
                                          else {
                                            fail();
                                          }
                                        }
                                        else {
                                          fail();
                                        }
                                      }
                                      else {
                                        fail();
                                      }
                                    }
                                    else {
                                      fail();
                                    }
                                  }
                                  else {
                                    fail();
                                  }
                                }
                                else {
                                  fail();
                                }
                              }
                              else {
                                fail();
                              }
                            }
                            else {
                              fail();
                            }
                          }
                          else {
                            fail();
                          }
                        }
                        else {
                          fail();
                        }
                      }
                      else {
                        fail();
                      }
                    }
                    else {
                      fail();
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  }
  else {
    fail();
  }
  return;
}
```

<br />
長さ0x15 (21文字) でチェックしていて、それぞれのif文を解析すると、たぶんこんな感じかな〜と思ったんですけどね。
<pre>
00 : r
01 : 3    // if ((int)pcParm1[2] - (int)pcParm1[1] == 1) {
02 : 4
03 : d    // if ((int)*pcParm1 + (int)pcParm1[3] == 0xd6) {
04 : i    // if ((int)pcParm1[4] - (int)pcParm1[3] == 5) {
05 : n    // if ((int)pcParm1[6] + (int)pcParm1[5] == 0xd5) {           // 110 + 103 = 213
06 : g    // if ((int)pcParm1[5] - (int)pcParm1[6] == 7) {              // 110 - 103
07 : _    // if ((int)pcParm1[7] - (int)pcParm1[8] == 0x2b) {
08 : 4    // if ((int)pcParm1[2] == (uint)(pcParm1[8] == 0)) { 
09 : 5    // if ((int)pcParm1[9] == (uint)(pcParm1[10] == 0)) {
10 : 5    // if ((int)pcParm1[10] - (int)pcParm1[8] == 1) {
11 : 3    // if ((int)pcParm1[1] == (uint)(pcParm1[0xb] == 0)) {
12 : m    // if ((int)pcParm1[0xd] + (int)pcParm1[0xc] == 0xcf) {    // 98 + 109
13 : b    // if ((int)pcParm1[0xd] * (int)pcParm1[0xc] == 0x29ba) {  // 2 * 7^2 * 109 = 98 * 109
14 : l    // if ((int)pcParm1[0xf] - (int)pcParm1[0xe] == 0xd) {
15 : y    // if ((int)pcParm1[0xf] - (int)*pcParm1 == 7) {
16 : _    // if ((int)pcParm1[7] == (uint)(pcParm1[0x10] == 0)) {
17 : d    // if ((int)pcParm1[3] == (uint)(pcParm1[0x11] == 0)) {
18 : 4    // if ((int)pcParm1[8] == (uint)(pcParm1[0x12] == 0)) {
19 : m    // if ((int)pcParm1[0xc] == (uint)(pcParm1[0x13] == 0)) {
20 : n    // if ((int)pcParm1[5] == (uint)(pcParm1[0x14] == 0)) {
</pre>


<br />
このパスフレーズを使ってプログラムを実行すると、
```
root@kali:~/cryptixctf# ./passphrase r34ding_455embly_d4mn
Please wait while we are authenticating....
.......
....
You are in!
```

"You are in!" と言われるんですが、実際にはフラグとして通りませんでした。

Discordでも、ご機嫌ナナメの人が数名いました。


Flag: <s>`flag{r34ding_455embly_d4mn}`</s> (?)



<br /><br />
それはさておき、Ghidraのデコンパイルの結果がちょっと変だと思いました。
```C
    if ((int)pcParm1[7] == (uint)(pcParm1[0x10] == 0)) {
      fail();
    }
```

if文にマッチしたらfail()だし。

"== 0" だし。。


<br /><br />
radare2で見ると、こんな感じ。

```C
[0x000005d0]> pdd@sym.check
/* r2dec pseudo code output */
/* passphrase @ 0x71c */
#include <stdint.h>
 
int64_t check (char * arg1) {
    char * s;
    size_t var_4h;
    rdi = arg1;
    s = rdi;
    rax = rdi;
    eax = strlen (rdi);
    var_4h = eax;
    if (var_4h != 0x15) {
        eax = 0;
        fail ();
    } else {
        rax = s;          // arg1
        rax += 7;         
        eax = *(rax);     // 7文字目
        edx = (int32_t) al;
        rax = s;
        rax += 0x10;
        eax = *(rax);     // 16文字目
        al = (al == 0) ? 1 : 0;
        eax = (int32_t) al;
        if (edx != eax) {
:
```


ふーん、まいっか（笑）



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

