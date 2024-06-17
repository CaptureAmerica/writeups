---
title: "BCACTF 5.0 Writeup"
date: 2024-06-12T07:00:00+09:00
lastmod: 2024-06-12T07:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbcactf_2024%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://bcactf.com/](https://bcactf.com/)
<br /><br />

BCACTF は、4回目の参加です。去年（2024年）は何故か参加しなかったみたいです。たぶん、モンハンにハマってた時期だと思います。

[BCACTF 1.0](https://captureamerica.github.io/writeups/post/bcactf_2019/) (62位)

[BCACTF 2.0](https://captureamerica.github.io/writeups/post/bcactf_2021/) (63位)

[BCACTF 3.0](https://captureamerica.github.io/writeups/post/bcactf_2022/) (54位)

[BCACTF 4.0] - 不参加


<br /><br />

今回は610点を獲得し、最終順位は239位でした。だいぶチャレンジの難易度が上がってきていますね。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_score.png" alt="bcactf_2024_score.png">

<br><br>

以下はチャレンジのリストです。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_binex.png" alt="bcactf_2024_binex.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_crypto1.png" alt="bcactf_2024_crypto1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_crypto2.png" alt="bcactf_2024_crypto2.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_foren1.png" alt="bcactf_2024_foren1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_foren2.png" alt="bcactf_2024_foren2.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_misc1.png" alt="bcactf_2024_misc1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_misc2.png" alt="bcactf_2024_misc2.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_rev1.png" alt="bcactf_2024_rev1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_rev2.png" alt="bcactf_2024_rev2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_rev3.png" alt="bcactf_2024_rev3.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_webex1.png" alt="bcactf_2024_webex1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_webex2.png" alt="bcactf_2024_webex2.png">

<br />




<br /><br />
<br /><br />
## [Foren]: flagserver (100 points)
- - -
### Challenge
> It looks like Ircus have been using a fully exposed application to access their flags! Look at this traffic I captured. I can't seem to get it to work, though... can you help me get the flag for this very challenge?
<br /><br />
nc challs.bcactf.com 30145

Attachment:

- flagserver.pcapng

<br>

### Solution
基本的には、Wiresharkで flagserver.pcapng を開いて、payload をバイナリファイルとして保存し（Export Packet Bytes）リプレイするだけです。

<br>

以下のpayloadは、バイナリエディタで編集が必要です。
<pre>
BCACTF_2024 $ xxd 3_org.bin
00000000: 7870 7400 0966 616b 6563 6861 6c6c       xpt..fakechall
</pre>

<br>

まず、`fakechall` を `flagserver` にしないといけないです。

その後、実際にやってみるとわかるのですが、`flagserve` のように9文字しか送信されないので、長さのパラメータが存在していることがわかります。"0x09" を "0x0A" に変更します。

<pre>
BCACTF_2024 $ xxd 3.bin
00000000: 7870 7400 0a66 6c61 6773 6572 7665 72    xpt..flagserver
</pre>

<br />

以下が書いたコードです。

```python
#!/usr/bin/env python
from pwn import *

s = remote("challs.bcactf.com", 30134)

sleep(1)
payload = open("0.bin", 'rb').read()
s.send(payload)

sleep(1)
payload = open("1.bin", 'rb').read()
s.send(payload)

sleep(1)
payload = open("2.bin", 'rb').read()
s.send(payload)

sleep(1)
payload = open("3.bin", 'rb').read()
s.send(payload)

# sleep(1)
# msg = s.recvline()
# print(msg)
s.close()
```

コードの最後でフラグを表示しようとしましたが、結局、Wiresharkでトラフィックをキャプチャーしてフラグを確認しました。

<br />

Flag: `bcactf{thankS_5OCK3ts_and_tHreADInG_clA5s_2f6fb44c998fd8}`



<br /><br />
<br /><br />
## [Foren]: Chalkboard Gag (25 points)
- - -
### Challenge
> Matt Groening sent me an unused chalkboard gag, he says there's something special inside of it.

Attachment:

- chalkboardgag.txt

以下のようなファイル。
<pre>
I WILL NOT BE SNEAKY
I WILL NOT BE SNEAKY
:
(snip)
</pre>

<br>

### Solution

こんな感じのファイルです。ほとんどの行は、`I WILL NOT BE SNEAKY` ですが、そうでない行がいくつかあります。

<pre>
$ cat chalkboardgag.txt | sort | uniq -c
   1 I WI1L NOT BE SNEAKY
   1 I WIDL NOT BE SNEAKY
   2 I WILL N0T BE SNEAKY
   1 I WILL NOT B3 SNEAKY
   1 I WILL NOT BE SBEAKY
   1 I WILL NOT BE SNEADY
99974 I WILL NOT BE SNEAKY
   1 I WILL NOT BE SNEAKa
   1 I WILL NOT BE SNEAKt
   1 I WILL NOT BE SNEAK}
   1 I WILL NOT BE SNEBKY
   1 I WILL NOT BE SNEPKY
   1 I WILL NOT BE SNEUKY
   1 I WILL NOT BE SNRAKY
   1 I WILL NOT BE SNuAKY
   1 I WILL NOT Bf SNEAKY
   1 I WILL NOT _E SNEAKY
   1 I WILL NOT bE SNEAKY
   1 I WILL NaT BE SNEAKY
   1 I WILR NOT BE SNEAKY
   1 I WILT NOT BE SNEAKY
   1 I WILW NOT BE SNEAKY
   1 I WIcL NOT BE SNEAKY
   1 I W_LL NOT BE SNEAKY
   1 I WcLL NOT BE SNEAKY
   1 I _ILL NOT BE SNEAKY
   1 I {ILL NOT BE SNEAKY
</pre>

<br>

ということで、`I WILL NOT BE SNEAKY` 以外の行だけ取り出します。

<pre>
$ grep -v "I WILL NOT BE SNEAKY" chalkboardgag.txt
I WILL NOT bE SNEAKY
I WIcL NOT BE SNEAKY
I WILL NOT BE SNEAKa
I WcLL NOT BE SNEAKY
I WILL NOT BE SNEAKt
I WILL NOT Bf SNEAKY
I {ILL NOT BE SNEAKY
I WILL NOT BE SNEBKY
I WILL NaT BE SNEAKY
I WILL NOT BE SNRAKY
I WILT NOT BE SNEAKY
I WILL NOT _E SNEAKY
I WILW NOT BE SNEAKY
I WILL N0T BE SNEAKY
I WILL NOT BE SNEUKY
I WI1L NOT BE SNEAKY
I WILL NOT BE SNEADY
I W_LL NOT BE SNEAKY
I WILL NOT BE SBEAKY
I WILL NOT B3 SNEAKY
I _ILL NOT BE SNEAKY
I WILL NOT BE SNEPKY
I WILR NOT BE SNEAKY
I WILL N0T BE SNEAKY
I WILL NOT BE SNuAKY
I WIDL NOT BE SNEAKY
I WILL NOT BE SNEAK}
</pre>

<br />

コマンドを使ってもできたかもですが、ここからは手動でやりました。各行から変更されている文字を取り出すだけです。

<br />

Flag: `bcactf{BaRT_W0U1D_B3_PR0uD}`



<br /><br />
<br /><br />
## [Binex]: Inaccessible (50 points)
- - -
### Challenge
> I wrote a function to generate the flag, but don't worry, I bet you can't access it!
<br /><br />
Hint1: you could reverse engineer the function, but it's not necessary<br>
see if you can use any debugging tools to just call the function

Attachment:

- chall (ELF 64-bit)

### Solution

ghidraでデコンパイルします。

```C
undefined8 main(void)

{
  puts("No flag for you >:(");
  return 0;
}


void win(void)

{
  long lVar1;
  char cVar2;
  char cVar3;
  int iVar4;
  void *pvVar5;
  byte local_58 [48];
  long local_28;
  int local_1c;
  
  pvVar5 = (void *)0x0;
  memset(local_58,0,0x28);
  for (local_1c = 0; local_1c < 0x25; local_1c = local_1c + 1) {
    lVar1 = *(long *)(b + (long)local_1c * 8);
    iVar4 = f(local_1c + 1);
    local_28 = lVar1 / (long)iVar4;
    cVar3 = f((int)(char)i2[local_1c]);
    cVar2 = (char)local_28;
    iVar4 = c((void *)(ulong)(uint)(int)(char)i4[local_1c],pvVar5);
    local_58[local_1c] = (char)iVar4 + cVar3 + cVar2;
    local_58[local_1c] = ~local_58[local_1c];
  }
  puts((char *)local_58);
  return;
}
```

<br />

フラグを表示するwin()関数が用意されていてますが、win()関数自体がどこからも呼ばれていない、というコードです。

<br />

gdbでwin()へjumpするだけです。

<pre>
gef➤  b main

gef➤  r

gef➤  jump win
Continuing at 0x4005ee.
bcactf{W0w_Y0u_m4d3_iT_b810c453a9ac9}
</pre>

<br />

Flag: `bcactf{W0w_Y0u_m4d3_iT_b810c453a9ac9}`



<br /><br />
<br /><br />
## [Binex]: Pwnage (100 points)
- - -
### Challenge
> It's either a bug, a hack, an exploit, or it's pwnage.
<br /><br />
Let this challenge stand as one of the first of many stairs to mastery over that which can only be described as pwn.
<br /><br />
nc challs.bcactf.com 31049


Attachment:

- provided.c

中身：
```C
int main() {
    // Hint: how do these values get stored?
    void* first_var;
    char* guess;
    char flag[100];
    load_flag(flag, 100);

    puts("Welcome to the most tasmastic game of all time!");
    wait_for(3);
    puts("Basically it's just too simple, I've put the");
    puts("flag into the memory and your job is ... to");
    puts("guess where it is!!");
    wait_for(2);
    puts("Have fun!");
    wait_for(1);
    puts("Oh and before you start, I'll give you a little");
    puts("hint, the address of the current stackframe I'm");
    printf("in is %p\n", (&first_var)[-2]);
    wait_for(3);
    puts("Okay anyway, back to the game. Make your guess!");
    puts("(hexadecimals only, so something like 0xA would work)");
    printf("guess> ");

    guess = read_pointer();

    wait_for(3);

    puts("Okay, prepare yourself. If you're right this");
    puts("will print out the flag");
    
    wait_for(1);
    puts("Oh, and if your wrong, this might crash and");
    puts("disconnect you\nGood luck!");

    printf("%s\n", guess);

    return 1;
}
```


<br>

### Solution

とりあえず、`nc`で繋いで動作を見てみます。

<pre>
$ nc challs.bcactf.com 30810
Welcome to the most tasmastic game of all time!
 . . .
Basically it's just too simple, I've put the
flag into the memory and your job is ... to
guess where it is!!
 . .
How fun is that!
 .
Oh and before you start, I'll give you a little
hint, the address of the current stackframe I'm
in is 0x7ffe71e7eaf0
 . . .
Okay anyway, back to the game. Make your guess!
(hexadecimals only, so something like 0xA would work)
guess>
</pre>

<br />

`first_var` のポインターが表示されます。その後、任意のポインターを入力すると、その部分のデータを表示してくれるプログラムなのがわかります。

`flag`のポインターは、`first_var` のポインター + 32 bytes です。

32 bytesがわからなくても、トライアンドエラーでも解くことができると思います。

<pre>
>>> hex(0x7ffe71e7eaf0 + 32)
'0x7ffe71e7eb10'
</pre>

<br />

<pre>
guess> 0x7ffe71e7eb10
 . . .
Okay, prepare yourself. If you're right this
will print out the flag
 .
Oh, and if your wrong, this might crash and
disconnect you
Good luck!
bcactf{0nE_two_thR3E_f0ur_567___sT3ps_t0_PwN4G3_70cc0e5edd6ea}
</pre>

<br />

Flag: `bcactf{0nE_two_thR3E_f0ur_567___sT3ps_t0_PwN4G3_70cc0e5edd6ea}`



<br /><br />
<br /><br />
## [Rev]: XOR (50 points)
- - -
### Challenge
> The executable below outputs an encrypted flag using the XOR operator. Can you decompile and reveal the flag?
<br /><br />
nc challs.bcactf.com 32411

Attachment:

- xor (ELF 64-bit)

<br>

### Solution

ghidraでデコンパイルします。

```C
undefined8 main(void)

{
  FILE *__stream;
  undefined8 uVar1;
  size_t __n;
  void *__ptr;
  void *__ptr_00;
  
  __stream = fopen("flag.txt","r");
  if (__stream == (FILE *)0x0) {
    puts("Failed to open flag file. Make sure flag.txt exists.");
    uVar1 = 1;
  }
  else {
    fseek(__stream,0,2);
    __n = ftell(__stream);
    fseek(__stream,0,0);
    __ptr = malloc(__n + 1);
    if (__ptr == (void *)0x0) {
      puts("Memory allocation failed for input.");
      fclose(__stream);
      uVar1 = 1;
    }
    else {
      fread(__ptr,1,__n,__stream);
      *(undefined *)((long)__ptr + __n) = 0;
      fclose(__stream);
      __ptr_00 = malloc(__n * 3 + 1);
      if (__ptr_00 == (void *)0x0) {
        puts("Memory allocation failed for output.");
        free(__ptr);
        uVar1 = 1;
      }
      else {
        xorEncrypt(__ptr,__ptr_00,__n);
        printf("Encrypted flag: %s\n",__ptr_00);
        free(__ptr);
        free(__ptr_00);
        uVar1 = 0;
      }
    }
  }
  return uVar1;
}


void xorEncrypt(long param_1,long param_2,ulong param_3)

{
  ulong i;
  

for (i = 0; i < param_3; i = i + 1) {
    sprintf((char *)(param_2 + i * 3),"%02X ",
            (ulong)(uint)(int)(char)("ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7"[i % 0x28] ^
                                    *(byte *)(i + param_1)));
  }
  *(undefined *)(param_2 + param_3 * 3) = 0;
  return;
}
```

<br />

`nc`で繋いで、動作も見てみます。

<pre>
$ nc challs.bcactf.com 32411
Encrypted flag: 21 0F 0A 15 3F 29 29 6B 13 1C 2C 74 7D 30 5E 50 6E 29 2B 24 19 0C 67 7D 05 54 7C 34 5C 13 32 42 29 62 7B 0F 4E
</pre>

<br />

`ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7` をKeyとしてXORするだけです。CyberChefを使いました。

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_xor.png" alt="bcactf_2024_xor.png">

<br />

Flag: `bcactf{SYMmE7ric_eNcrYP710N_4WD0f229}`



<br /><br />
<br /><br />
## [Rev]: Flagtureiser (50 points)
- - -
### Challenge
> Here's a totally normal Minecraft mod (1.19.4, Forge) I've been making, check it out!
<br /><br />
(You do not need Minecraft to solve this challenge)

Attachment:

- flagtureiser-4.2.0.6.9.jar

<br>

### Solution

Jarファイルを JD-GUI で開いて、見つけた数字を CyberChef で文字にするだけです。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_Flagtureiser1.png" alt="bcactf_2024_Flagtureiser1.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_Flagtureiser2.png" alt="bcactf_2024_Flagtureiser2.png">

<br />

Flag: `bcactf{fRaCtur31s3R_sT8gE_z3R0}`



<br /><br />
<br /><br />
## [Web]: NoSQL (25 points)
- - -
### Challenge
> I found this database that does not use SQL, is there any way to break it?
<br /><br />
[challs.bcactf.com:30390](challs.bcactf.com:30390)
<br /><br />
Hint: Ricardo Olsen has an ID of 1

Attachment:

- provided.js

中身：
{{< highlight Javascript "linenos=table,hl_lines=21 28" >}}
const express = require('express')

const app = express();
const port = 3000;
const fs = require('fs')
try {
    const inputD = fs.readFileSync('table.txt', 'utf-8');
    text = inputD.toString().split("\n").map(e => e.trim());
} catch (err) {
    console.error("Error reading file:", err);
    process.exit(1);
}

app.get('/', (req, res) => {
    if (!req.query.name) {
        res.send("Not a valid query :(")
        return;
    }
    let goodLines = []
    text.forEach( line => {
        if (line.match('^'+req.query.name+'$')) {
            goodLines.push(line)
        }
    });
    res.json({"rtnValues":goodLines})
})

app.get('/:id/:firstName/:lastName', (req, res) => {
    // Implementation not shown
    res.send("FLAG")
})

app.listen(port, () => {
    console.log(`App server listening on ${port}. (Go to http://localhost:${port})`);
});
{{< / highlight >}}


<br>

### Solution

与えられたヒント `Ricardo Olsen has an ID of 1` とコードを見ると、以下にアクセスしたら何か得られるはずだと思ったんですが、`Not Valid Query`というエラーが出るだけだったので小一時間悩みました。

<pre>
http://challs.bcactf.com:30390/1/Ricardo/Olsen
</pre>

<br />

21行目のところで正規表現を使って line.match() をしているので、`.*` を入れたらテーブルの中身が全部取れました。

<pre>
http://challs.bcactf.com:30390/?name=.*
{"rtnValues":["Ricardo Olsen","April Park","Francis Jackson","Ana Barry","Clifford Craig","Andrew Wise","Ada Atkinson","Janis McIntosh","Rosie Parsons","Neal Weaver","Alyssa Robison","Michael Hurst","Roberto Thornton","Renee Schwartz","Darryl Wilson","Wayne Boyle","Loretta Camacho","Bert Morton","Suzanne Johnson","Carol Fowler","Rose Hansen","Aimee Norman","Bethany Foley","Benjamin Baily","David Hull","Sabrina Fish","Rick Kirby","Edgar Grimes","Blake McDermott","Alicia Crosby","Teresa Ortega","Carroll Darling","Louis Tate","Phillip Fuller","Clinton Kimball","Alma Matthews","Stacie Franklin","Lucinda Steward","Gina Andrews","Philip Hyde","Devin Riggs","Michelle Thornton","Rogelio Freeman","Arthur Stephens","Andy Leon","Megan Gould","Myrna Yates","Edwin Pearce","Shirley Cannon","Lowell Cochran","Flag Holder"]}
</pre>

<br />

末尾の51番目の要素のところに、それっぽいユーザ（Flag Holder）がいます。

ということで、以下でフラグが取れます。

<pre>
http://challs.bcactf.com:30390/51/Flag/Holder
</pre>

<br />

Flag: `bcactf{R3gex_WH1z_54dfa9cdba13}`



<br /><br />
<br /><br />
## [Crypto]: Time Skip (50 points)
- - -
### Challenge
> One of our problem writers got sent back in time! We found a piece a very very old piece of parchment where he disappeared, alongside a long cylinder. See if you can uncover his flag!

Attachment:

- parchment.txt

中身：

<pre>
hsggna0stiaeaetteyc4ehvdatyporwtyseefregrstaf_etposruouoy{qnirroiybrbs5edmothssavetc8hebhwuibihh72eyaoepmlvoet9lobulpkyenf4xpulsloinmelllisyassnousa31mebneedtctg_}eeedeboghbihpatesyyfolus1lnhnooeliotb5ebidfueonnactayseyl
</pre>

<br>

### Solution

"parchment cylinder cipher" を Keyword としてググって、Scytale Cipher を見つけました。

https://www.dcode.fr/cipher-identifier を使っても、4番目の候補として出てくるので、一個一個試したら解けますね。

<br />

Decryptした中にフラグが出てきます。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2024_parchment.png" alt="bcactf_2024_parchment.png">

<br />

Flag: `bcactf{5c7t4l3_h15t04y_qe829xl1}`




<br /><br />
<br /><br />
## [Foren]: magic (75 points)
- - -
### Challenge
> I found this piece of paper on the floor. I was going to throw it away, but it somehow screamed at me while I was holding it?!

Attachment:

- magic.pdf

<br>

### (Unsolved)

Javascriptの難読化が解けなくて、降参しました。

<pre>
$ python3 ./pdf-parser.py -o 2 -f -d obj.bin magic.pdf
obj 2 0
 Type: /ObjStm
 Referencing:
 Contains stream

  <<
    /Type /ObjStm
    /N 30
    /First 224
    /Length 2622
    /Filter /FlateDecode
  >>

$ strings obj.bin | grep -i flag
:
(snip)
:
</pre>

<br />

以下は、Beautifyした後のコードです。少し編集もしています。

```C
(function(_0x18b13a, _0x4d582d) {
    var _0x3da883 = _0x4113,
        _0x1d0353 = _0x18b13a();
    while (!![]) {
        try {
            var _0x10c45c = parseInt(_0x3da883(0x1be)) / 0x1 * (-parseInt(_0x3da883(0x1cc)) / 0x2) + parseInt(_0x3da883(0x1c2)) / 0x3 + parseInt(_0x3da883(0x1c6)) / 0x4 * (parseInt(_0x3da883(0x1c7)) / 0x5) + -parseInt(_0x3da883(0x1cb)) / 0x6 * (parseInt(_0x3da883(0x1c1)) / 0x7) + -parseInt(_0x3da883(0x1ca)) / 0x8 + parseInt(_0x3da883(0x1c0)) / 0x9 + parseInt(_0x3da883(0x1c4)) / 0xa * (parseInt(_0x3da883(0x1bf)) / 0xb);
            if (_0x10c45c === _0x4d582d) break;
            else _0x1d0353['push'](_0x1d0353['shift']());
        } catch (_0x53c9c0) {
            _0x1d0353['push'](_0x1d0353['shift']());
        }
    }
}(_0x43c8, 0xe20be));

function _0x4113(_0x44cfd2, _0x23b14b) {
    var _0x43c873 = _0x43c8();
    return _0x4113 = function(_0x4113e1, _0x43c2ed) {
        _0x4113e1 = _0x4113e1 - 0x1bd;
        var _0x2522f0 = _0x43c873[_0x4113e1];
	// console.log(_0x2522f0);
        return _0x2522f0;
    }, _0x4113(_0x44cfd2, _0x23b14b);
}

function _0x43c8() {
    var _0x1355d8 = ['getField', 'charCodeAt', '100554TvjbzQ', '11jHxsKn', '7564617EnopjV', '2219BJkXWe', '3372363teHOVr', 'alert', '5165870pcLTuS', 'producer', '32KYViix', '925835vZTXso', 'Flag is incorrect!', 'length', '8132288HsoZUP', '13494jFFdda', '26rtwUNT'];
    _0x43c8 = function() {
        return _0x1355d8;
    };
    return _0x43c8();
}

function update() {
    var _0x3d0e72 = _0x4113,
        // _0x2923fd = this[_0x3d0e72(0x1cd)]('A')['value'],
	_0x2923fd = 43,
        _0x12e8ec = [];
    for (var _0x28002d = 0x0; _0x28002d < _0x2923fd[_0x3d0e72(0x1c9)]; _0x28002d++) {
        _0x12e8ec['push'](_0x2923fd[_0x3d0e72(0x1bd)](_0x28002d) ^ parseInt(info[_0x3d0e72(0x1c5)]) % (0x75 + _0x28002d));
    }
    k = [0x46, 0x2d, 0x62, 0x11, 0x6b, 0x4c, 0x72, 0x5f, 0x76, 0x38, 0x19, 0x28, 0x5f, 0x31, 0x36, 0x63, 0xf7, 0xb1, 0x69, 0x2a, 0x18, 0x5e, 0x36, 0x1, 0x37, 0x3a, 0x1c, 0x5, 0x11, 0x56, 0xe5, 0x7b, 0x64, 0x2c, 0x11, 0x14, 0x53, 0x5a, 0x35, 0x17, 0x41, 0x62, 0x3];
    // console.log(k[_0x3d0e72(0x1c9)]);
    // 43
    if (_0x12e8ec['length'] != k[_0x3d0e72(0x1c9)]) {
        // app[_0x3d0e72(0x1c3)](_0x3d0e72(0x1c8));
        // return;
    }
    for (var _0x28002d = 0x0; _0x28002d < k[_0x3d0e72(0x1c9)]; _0x28002d++) {
	//console.log(k[_0x28002d]);
	//console.log(_0x12e8ec[_0x28002d]);
        if (_0x12e8ec[_0x28002d] != k[_0x28002d]) {
            // console.log(_0x3d0e72(0x1c8));
            // return;
        }
    }
    app[_0x3d0e72(0x1c3)]('Flag is correct!');
}
update();
```

<br />

`node` コマンドを使って実行してみたりしたけど、よくわからなかったです。。。

<br />


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />