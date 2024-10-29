---
title: "UMDCTF 2021 Writeup"
date: 2021-04-25T11:00:00+09:00
lastmod: 2021-05-29T21:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fumdctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2021/05/29 - 少し復習しました。下の方に追記してます。)

URL: [https://umdctf.io/challenges](https://umdctf.io/challenges)
<br /><br />
1501 points を取り、175thでした。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_Score.png" alt="umdctf_2021_Score.png">

<br />
ほとんど解けてないです^^;

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_Challenges1.png" alt="umdctf_2021_Challenges1.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_Challenges2.png" alt="umdctf_2021_Challenges2.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_Challenges3.png" alt="umdctf_2021_Challenges3.png"> <br />


<br />
あんまり特記すべきこともないんですが、いくつか Writeup 残しておきます。


<br /><br />
## [Forensics]: Not Slick (150 points)
- - -
### Challenge
> My friend always messes with PNGs.... what did he do this time?

Attachment:

- notslick.png

<br />

### Solution
ファイル・タイプを確認します。正常な png ファイルではないようです。

<pre>
$ file notslick.png
notslick.png: data
</pre>

<br />
どうやら反転しているようです。

<pre>
 $ xxd notslick.png | tail
00018c00: fb5d 6b9e b5ed 7bd9 8663 d71e 3d6b f6b5  .]k...{..c..=k..
00018c10: af7b 5efd ed75 c3df 7f7c d72b ae6d e3f7  .{^..u...|.+.m..
00018c20: 6fb8 4ce3 cf9c 66e0 3a7f 4621 42bb 8687  o.L...f.:.F!B...
00018c30: 1a42 6665 10ee a10f f42f e10e 515f e1dc  .Bfe...../..Q_..
00018c40: a5fe 10ca 1fe2 89a9 4e52 634e 4343 93dd  ........NRcNCC..
00018c50: 21c9 50df cadd 3926 a222 49a1 0e57 fdf7  !.P...9&."I..W..
00018c60: fff7 5cd9 7efc 0bdd ec5e 7854 4144 4900  ..\.~....^xTADI.
00018c70: 2000 002b 3a4e fb00 0000 0608 a903 0000   ..+:N..........
00018c80: 8007 0000 5244 4849 0d00 0000 0a1a 0a0d  ....RDHI........
00018c90: 474e 5089                                GNP.
</pre>


<br />

<pre>
$ python3
>>> open("notslick_rev.png", "wb").write(open("notslick.png", "rb").read()[::-1])
</pre>

このやり方は、過去の何かのCTFイベントでやったやつです。どれだったかは覚えてないです。

<br />

Flag: `UMDCTF-{abs01ute1y_r3v3r53d}`





<br /><br />
<br /><br />
## [Forensics]: Protocol One and Zero (200)
- - -
### Challenge
> We have noticed some suspicious pings on localhost that appear to hide a message. Can you recover the message?

Attachment:

- protocol_one_and_zero.pcapng


<br />

### Solution
タイトルの通りです。以下のように、0x00 のものと 0xFF のものがあるので、それぞれを0と1と見立てて2値化した後で文字にするだけです。

<pre>
$ tshark -r protocol_one_and_zero.pcapng -T fields -e data.data | uniq
866e0d000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
dc7f0d0000000000ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
87910d000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
1da40d0000000000ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
f7b50d000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
:
:
</pre>

<pre>
$ tshark -r protocol_one_and_zero.pcapng -T fields -e data.data | uniq | rev | cut -c -1 | tr -d "\n" | gsed -e 's/f/1/g' ; echo
01010101010011010100010001000011010101000100011000101101011110110110001000110001011011100101111101110000001100010100111001100111010111110101000000110000011011100110011101111101
</pre>

<br />

Flag: `UMDCTF-{b1n_p1Ng_P0ng}`




<br /><br />
<br /><br />
## [Forensics]: Pretty Dumb File (200)
- - -
### Challenge
> You find what looks to be a regular PDF, maybe Didier Stevens can help figure out what it contains.

Attachment:

- paper.pdf


<br />

### Solution
エキストエディタで開くと、Object 8 のstreamの部分に読めないバイナリがあるのがわかります。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_paperpdf.png" alt="umdctf_2021_paperpdf.png"> <br />

<br />

pdf-parser.py を使って取り出します。

<pre>
$ pdf-parser.py -o 8 -f -d obj8.dump paper.pdf
obj 8 0
 Type: /EmbeddedFile
 Referencing:
 Contains stream

  <<
    /Length 43
    /Filter /FlateDecode
    /Type /EmbeddedFile
  >>

$ file obj8.dump
obj8.dump: ASCII text

$ cat obj8.dump
UMDCTF-{actually_1ts_pr3tty_smart}
</pre>

<br>

Flag: `UMDCTF-{actually_1ts_pr3tty_smart}`



<br /><br />
<br /><br />
## [Crypto]: Celebration (100)
- - -
### Challenge
> I know these aren't my usual card tricks, but these little men made me laugh. I hope they bring a smile to your face too :)

Attachment:

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_Celebration.png" alt="umdctf_2021_Celebration.png">


<br />

### Solution
この手のシンボルの問題は無数にあるので、いかにデコーダを見つけるかがキーになります。今回は、"CTF crypto symbol" をキーワードにGoogleで画像検索していたら見つかりました。

[Dancing Men Cipher](https://www.dcode.fr/dancing-men-cipher) というそうです。

<br />

Flag: `UMDCTF-{yo_ITS_A_PARTYYY}`



<br /><br />
<br /><br />
## [Rev]: Starbucks (150)
- - -
### Challenge
> Unfortunately, you have been forced to use Java, but you are only given a single class file which doesn't seem to work.

Attachment:

- IsThisTheFlag.class


<br />

### Solution
jadを使ってデコンパイルしました。

<pre>
$ jad IsThisTheFlag.class 
Parsing IsThisTheFlag.class...The class file version is 52.0 (only 45.3, 46.0 and 47.0 are supported)
 Generating Challenge.jad
</pre>

```Java
// Decompiled by Jad v1.5.8e. Copyright 2001 Pavel Kouznetsov.
// Jad home page: http://www.geocities.com/kpdus/jad.html
// Decompiler options: packimports(3) 
// Source File Name:   Challenge.java

import java.io.PrintStream;

public class Challenge
{

    public Challenge()
    {
    }

    public static String f1(String s)
    {
        StringBuilder b = new StringBuilder();
        char arr[] = s.toCharArray();
        for(int i = 0; i < arr.length; i++)
            b.append((char)(arr[i] + i));

        return b.toString();
    }

    public static String f1_rev(String s)
    {
        StringBuilder b = new StringBuilder();
        char arr[] = s.toCharArray();
        for(int i = 0; i < arr.length; i++)
            b.append((char)(arr[i] - i));

        return b.toString();
    }

    public static String f2(String s)
    {
        int half = s.length() / 2;
        return (new StringBuilder(String.valueOf(s.substring(half + 1)))).append(s.substring(0, half + 1)).toString();
    }

    public static String f3()
    {
        return f1(f2("$aQ\"cNP `_\035[eULB@PA'thpj]"));
    }

    public static void main(String args[])
    {
        System.out.println("You really thought finding the flag would be so easy?");
    }
}
```

<br />
main()は文字列を表示しているだけです。

f3()がなんとなくフラグ生成関数っぽいので、main()からf3()を呼ぶようにしてコンパイルして実行します。

ファイル名が "Challenge.jad" になっているので、"Challenge.java" に変えておきます。

<pre>
$ javac Challenge.java

$ java Challenge
UMDCTF-{pyth0n_1s_b3tt3r}
</pre>

<br />

Flag: `UMDCTF-{pyth0n_1s_b3tt3r}`


<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。


<br /><br />
## [Misc]: John's Return (350 points)
- - -
### Challenge
> I received this network traffic from John, but I don't what he's trying to say? Can you figure it out?

Attachment:

- received.pcapng

<br />

### Solution
ワイヤレスのトラフィックが入ったpcapなので、まず aircrack-ng をやらないといけないやつでした。

最初に、received.pcapng は、Wireshark から Save As で pcap として保存しておいてから aircrack-ng にかけます。

<pre>
$ aircrack-ng received.pcap
Reading packets, please wait...
Opening received.pcap
Read 76 packets.

   #  BSSID              ESSID                     Encryption

   1  00:23:69:AA:69:F5  linksys                   WPA (1 handshake, with PMKID)

Choosing first network as target.

Reading packets, please wait...
Opening received.pcap
Read 76 packets.

1 potential targets

Please specify a dictionary (option -w).
</pre>

<br />
-w で辞書を指定するように言われるので、rockyou.txtを使ってみます。

<pre>
$ aircrack-ng received.pcap -w /usr/share/wordlists/rockyou.txt
Reading packets, please wait...
Opening received.pcap
Read 76 packets.

   #  BSSID              ESSID                     Encryption

   1  00:23:69:AA:69:F5  linksys                   WPA (1 handshake, with PMKID)

Choosing first network as target.

Reading packets, please wait...
Opening received.pcap
Read 76 packets.

1 potential targets

                               Aircrack-ng 1.6

      [00:00:00] 59/10303727 keys tested (966.86 k/s)

      Time left: 2 hours, 57 minutes, 46 seconds                 0.00%

                           KEY FOUND! [ chocolate ]

:
(snip)
:
</pre>

<br />
Key `chocolate` が見つかりました。

<br />
Wiresharkで wpa-pwd として指定します。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_wpapwd.png" alt="umdctf_2021_wpapwd.png">

<br />
Configuration Test Protocol とかが見えるようになりました。その中にフラグが入ってます。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2021_receivedpcap.png" alt="umdctf_2021_receivedpcap.png">

<br />

Flag: `UMDCTF-{wh3r3_j0hn}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

