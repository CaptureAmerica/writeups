---
title: "RedpwnCTF 2019 Writeup"
date: 2019-08-17T10:00:00+09:00
lastmod: 2019-08-17T10:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fredpwnctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.redpwn.net/](https://ctf.redpwn.net/)
<br /><br />
なかなか興味深いチャレンジがいくつかありました。
<br /><br />
ほとんど解けてないので、他の人のWriteupを見て勉強しようと思います。
<br /><br />
いちおう、やった中からいくつか記念に残しておきます。


<br /><br />
## [Web]: easycipher
- - -
### Challenge
> This is an easy cipher? I heard it's broken.
<br /><br />
[http://chall.2019.redpwn.net:8006/](http://chall.2019.redpwn.net:8006/)

<br />

### Solution
アクセスすると、以下が表示されます。

<img src="https://captureamerica.github.io/writeups/img/easycipher.PNG" alt="easycipher.PNG">

View sourceすると、1行で書かれたJava Scriptがあるので、ChromeのDeveloper ToolでBeautifyします。

以下、Beautify後。
{{< highlight html "linenos=table,hl_lines=129" >}}
<script>
    var _0x29a9 = ["\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x61\x62\x63\x64\x65\x66", "", "\x63\x68\x61\x72\x41\x74", "\x6C\x65\x6E\x67\x74\x68", "\x63\x68\x61\x72\x43\x6F\x64\x65\x41\x74", "\x57\x68\x61\x74\x20\x69\x73\x20\x74\x68\x65\x20\x70\x61\x73\x73\x77\x6F\x72\x64", "\x61\x61\x34\x32\x62\x32\x33\x34\x63\x62\x30\x35\x39\x31\x35\x37\x31\x36\x63\x31\x34\x33\x34\x30\x35\x38\x66\x65\x31\x61\x65\x65\x31\x36\x63\x31\x34\x33\x34\x30\x63\x62\x30\x35\x39\x31\x35\x37\x61\x61\x34\x32\x62\x32\x33\x34", "\x73\x75\x62\x6D\x69\x74\x20\x61\x73\x20\x72\x65\x64\x70\x77\x6E\x63\x74\x66\x7B\x50\x41\x53\x53\x57\x4F\x52\x44\x7D", "\x3A\x28"];
    var hex_chr = _0x29a9[0];
    function rhex(_0x8e5dx3) {
        str = _0x29a9[1];
        for (j = 0; j <= 3; j++) {
            str += hex_chr[_0x29a9[2]]((_0x8e5dx3 >> (j * 8 + 4)) & 0x0F) + hex_chr[_0x29a9[2]]((_0x8e5dx3 >> (j * 8)) & 0x0F)
        }
        ;return str
    }
    function str2blks_MD5(_0x8e5dx5) {
        nblk = ((_0x8e5dx5[_0x29a9[3]] + 8) >> 6) + 1;
        blks = new Array(nblk * 16);
        for (i = 0; i < nblk * 16; i++) {
            blks[i] = 0
        }
        ;for (i = 0; i < _0x8e5dx5[_0x29a9[3]]; i++) {
            blks[i >> 2] |= _0x8e5dx5[_0x29a9[4]](i) << ((i % 4) * 8)
        }
        ;blks[i >> 2] |= 0x80 << ((i % 4) * 8);
        blks[nblk * 16 - 2] = _0x8e5dx5[_0x29a9[3]] * 8;
        return blks
    }
    function add(_0x8e5dx7, _0x8e5dx8) {
        var _0x8e5dx9 = (_0x8e5dx7 & 0xFFFF) + (_0x8e5dx8 & 0xFFFF);
        var _0x8e5dxa = (_0x8e5dx7 >> 16) + (_0x8e5dx8 >> 16) + (_0x8e5dx9 >> 16);
        return (_0x8e5dxa << 16) | (_0x8e5dx9 & 0xFFFF)
    }
    function rol(_0x8e5dx3, _0x8e5dxc) {
        return (_0x8e5dx3 << _0x8e5dxc) | (_0x8e5dx3 >>> (32 - _0x8e5dxc))
    }
    function cmn(_0x8e5dxe, _0x8e5dxf, _0x8e5dx10, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12) {
        return add(rol(add(add(_0x8e5dxf, _0x8e5dxe), add(_0x8e5dx7, _0x8e5dx12)), _0x8e5dx11), _0x8e5dx10)
    }
    function ff(_0x8e5dxf, _0x8e5dx10, _0x8e5dx14, _0x8e5dx15, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12) {
        return cmn((_0x8e5dx10 & _0x8e5dx14) | ((~_0x8e5dx10) & _0x8e5dx15), _0x8e5dxf, _0x8e5dx10, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12)
    }
    function gg(_0x8e5dxf, _0x8e5dx10, _0x8e5dx14, _0x8e5dx15, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12) {
        return cmn((_0x8e5dx10 & _0x8e5dx15) | (_0x8e5dx14 & (~_0x8e5dx15)), _0x8e5dxf, _0x8e5dx10, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12)
    }
    function hh(_0x8e5dxf, _0x8e5dx10, _0x8e5dx14, _0x8e5dx15, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12) {
        return cmn(_0x8e5dx10 ^ _0x8e5dx14 ^ _0x8e5dx15, _0x8e5dxf, _0x8e5dx10, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12)
    }
    function ii(_0x8e5dxf, _0x8e5dx10, _0x8e5dx14, _0x8e5dx15, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12) {
        return cmn(_0x8e5dx14 ^ (_0x8e5dx10 | (~_0x8e5dx15)), _0x8e5dxf, _0x8e5dx10, _0x8e5dx7, _0x8e5dx11, _0x8e5dx12)
    }
    function calcMD5(_0x8e5dx5) {
        x = str2blks_MD5(_0x8e5dx5);
        a = 1732584193;
        b = -271733879;
        c = -1732584194;
        d = 271733878;
        for (i = 0; i < x[_0x29a9[3]]; i += 16) {
            olda = a;
            oldb = b;
            oldc = c;
            oldd = d;
            a = ff(a, b, c, d, x[i + 0], 7, -680876936);
            d = ff(d, a, b, c, x[i + 1], 12, -389564586);
            c = ff(c, d, a, b, x[i + 2], 17, 606105819);
            b = ff(b, c, d, a, x[i + 3], 22, -1044525330);
            a = ff(a, b, c, d, x[i + 4], 7, -176418897);
            d = ff(d, a, b, c, x[i + 5], 12, 1200080426);
            c = ff(c, d, a, b, x[i + 6], 17, -1473231341);
            b = ff(b, c, d, a, x[i + 7], 22, -45705983);
            a = ff(a, b, c, d, x[i + 8], 7, 1770035416);
            d = ff(d, a, b, c, x[i + 9], 12, -1958414417);
            c = ff(c, d, a, b, x[i + 10], 17, -42063);
            b = ff(b, c, d, a, x[i + 11], 22, -1990404162);
            a = ff(a, b, c, d, x[i + 12], 7, 1804603682);
            d = ff(d, a, b, c, x[i + 13], 12, -40341101);
            c = ff(c, d, a, b, x[i + 14], 17, -1502002290);
            b = ff(b, c, d, a, x[i + 15], 22, 1236535329);
            a = gg(a, b, c, d, x[i + 1], 5, -165796510);
            d = gg(d, a, b, c, x[i + 6], 9, -1069501632);
            c = gg(c, d, a, b, x[i + 11], 14, 643717713);
            b = gg(b, c, d, a, x[i + 0], 20, -373897302);
            a = gg(a, b, c, d, x[i + 5], 5, -701558691);
            d = gg(d, a, b, c, x[i + 10], 9, 38016083);
            c = gg(c, d, a, b, x[i + 15], 14, -660478335);
            b = gg(b, c, d, a, x[i + 4], 20, -405537848);
            a = gg(a, b, c, d, x[i + 9], 5, 568446438);
            d = gg(d, a, b, c, x[i + 14], 9, -1019803690);
            c = gg(c, d, a, b, x[i + 3], 14, -187363961);
            b = gg(b, c, d, a, x[i + 8], 20, 1163531501);
            a = gg(a, b, c, d, x[i + 13], 5, -1444681467);
            d = gg(d, a, b, c, x[i + 2], 9, -51403784);
            c = gg(c, d, a, b, x[i + 7], 14, 1735328473);
            b = gg(b, c, d, a, x[i + 12], 20, -1926607734);
            a = hh(a, b, c, d, x[i + 5], 4, -378558);
            d = hh(d, a, b, c, x[i + 8], 11, -2022574463);
            c = hh(c, d, a, b, x[i + 11], 16, 1839030562);
            b = hh(b, c, d, a, x[i + 14], 23, -35309556);
            a = hh(a, b, c, d, x[i + 1], 4, -1530992060);
            d = hh(d, a, b, c, x[i + 4], 11, 1272893353);
            c = hh(c, d, a, b, x[i + 7], 16, -155497632);
            b = hh(b, c, d, a, x[i + 10], 23, -1094730640);
            a = hh(a, b, c, d, x[i + 13], 4, 681279174);
            d = hh(d, a, b, c, x[i + 0], 11, -358537222);
            c = hh(c, d, a, b, x[i + 3], 16, -722521979);
            b = hh(b, c, d, a, x[i + 6], 23, 76029189);
            a = hh(a, b, c, d, x[i + 9], 4, -640364487);
            d = hh(d, a, b, c, x[i + 12], 11, -421815835);
            c = hh(c, d, a, b, x[i + 15], 16, 530742520);
            b = hh(b, c, d, a, x[i + 2], 23, -995338651);
            a = ii(a, b, c, d, x[i + 0], 6, -198630844);
            d = ii(d, a, b, c, x[i + 7], 10, 1126891415);
            c = ii(c, d, a, b, x[i + 14], 15, -1416354905);
            b = ii(b, c, d, a, x[i + 5], 21, -57434055);
            a = ii(a, b, c, d, x[i + 12], 6, 1700485571);
            d = ii(d, a, b, c, x[i + 3], 10, -1894986606);
            c = ii(c, d, a, b, x[i + 10], 15, -1051523);
            b = ii(b, c, d, a, x[i + 1], 21, -2054922799);
            a = ii(a, b, c, d, x[i + 8], 6, 1873313359);
            d = ii(d, a, b, c, x[i + 15], 10, -30611744);
            c = ii(c, d, a, b, x[i + 6], 15, -1560198380);
            b = ii(b, c, d, a, x[i + 13], 21, 1309151649);
            a = ii(a, b, c, d, x[i + 4], 6, -145523070);
            d = ii(d, a, b, c, x[i + 11], 10, -1120210379);
            c = ii(c, d, a, b, x[i + 2], 15, 718787259);
            b = ii(b, c, d, a, x[i + 9], 21, -343485551);
            a = add(a, olda);
            b = add(b, oldb);
            c = add(c, oldc);
            d = add(d, oldd)
        }
        ;return rhex(a) + rhex(b) + rhex(c) + rhex(d) + rhex(c) + rhex(b) + rhex(a)
    }
    if (calcMD5(prompt(_0x29a9[5])) === _0x29a9[6]) {
        alert(_0x29a9[7])
    } else {
        alert(_0x29a9[8])
    }
</script>
{{< / highlight >}}

一番下で判定している箇所がポイントです。途中のコードは解析不要です。

_0x29a9[]の配列は先頭で定義されていて、それぞれ、

_0x29a9[5] : "What is the password" <br />
_0x29a9[6] : "aa42b234cb05915716c1434058fe1aee16c14340cb059157aa42b234" <br />
_0x29a9[7] : "submit as redpwnctf{PASSWORD}" <br />
_0x29a9[8] : ":(" <br />

入力した文字列がMD5計算されて、aa42b234cb05915716c1434058fe1aee16c14340cb059157aa42b234になればオッケー。

Rainbow table ( [https://crackstation.net/](https://crackstation.net/) )で、"shazam"が取れました。

Flag: `redpwnctf{shazam}`

<br />
どこら辺が"broken"なのかは不明。。



<br /><br />
<br /><br />
## [Rev]: Generic Crackme Redux
- - -
### Challenge
> Note: Enclose the flag with flag{}.

Attachment:

- generic_crackme_redux

<br />

### Solution
まずは、Ghidraにかけます。

```C
undefined8 FUN_00101186(void)
{
  char cVar1;
  long in_FS_OFFSET;
  uint local_14;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  printf("Enter access code: ");
  __isoc99_scanf(&DAT_00102018,&local_14);
  cVar1 = FUN_00101169((ulong)local_14);   // 以下を参照
  if (cVar1 == 0) {
    puts("Bzzzzrrrppp");
  }
  else {
    puts("Access granted");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

```C
ulong FUN_00101169(int iParm1)
{
  return (ulong)(iParm1 * 10 & 0xffffff00U | (uint)(iParm1 * 10 == 0xac292));
}
```

<br />
デコンパイルされた結果（上記）が、入力値を10倍してマスクして、判定文っぽいのとORして、それをリターンして、と意味不明だったので、アセンブラコードを見ました。

ここら辺。
<pre>
    1164:	e9 67 ff ff ff       	jmp    10d0 <__isoc99_scanf@plt+0x70>
    1169:	55                   	push   rbp
    116a:	48 89 e5             	mov    rbp,rsp
    116d:	89 7d fc             	mov    DWORD PTR [rbp-0x4],edi
    1170:	8b 55 fc             	mov    edx,DWORD PTR [rbp-0x4]
    1173:	89 d0                	mov    eax,edx  <--- edxが入力値
    1175:	c1 e0 02             	shl    eax,0x2  <--- 2ビット左シフト(4倍)
    1178:	01 d0                	add    eax,edx  <--- 入力値と足す
    117a:	01 c0                	add    eax,eax  <--- 2倍
    117c:	3d 92 c2 0a 00       	cmp    eax,0xac292
</pre>

入力値をxとすると、<br />
(4x + x) * 2 = 0xac292
<br />
10 * x = 0xac292
<br />
x = 0xac292 / 10

<br />
ということで、計算。
<pre>
$ python
>>> 0xac292/10
70517.0
</pre>

<br />
動作確認。
<pre>
# ./generic_crackme_redux 
Enter access code: 70517
Access granted
</pre>

Flag: `flag{70517}`


<br />
ちなみに、Ghidraのデコンパイル結果の通りだったやｗｗ （今、気づいた）

```C
  (iParm1 * 10 == 0xac292)
```


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

