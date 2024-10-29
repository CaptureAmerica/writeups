---
title: "Rooters CTF 2019 Writeup"
date: 2019-10-14T16:00:00+09:00
lastmod: 2019-10-20T11:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Frootersctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2019/10/20 - 復習しました)

URL: [https://rootersctf.in/](https://rootersctf.in/)
<br /><br />
picoCTF 2019の直後でほとんど時間がなかったですが、ちょっとだけやりました。

後日、復習もやりました（偉い）。


<br /><br />
## [Forensics]: You Can't See Me
- - -
### Challenge
> See for yourself

Attachments:

- youcantseeme.pdf

<br />

### Solution
100 ページくらい真っ白が続くPDFが与えられます。

やったこと：

- pdf-parser.py<br />
- pdfid.py<br />
- Adobe Reader（特定のページになんかしらテキストっぽいのがあるのがわかるんですが、フォントの色の変え方がわかんなかったです。）<br />
- Libre Office (少しずつフォントの色は変えられたんですが、全ページまとめてやる方法がわかんなかったです。)<br />
- PDFStreamDumper (しばらく使わないうちに、起動がうまくいかなくなってた。。。)

<br />
以下でフラグをゲットしました。

- pdftotext -f 16 youcantseeme.pdf text.txt

(16を指定したのは、最初に見つけたテキストが16ページだったからです。)

Flag: `rooters{Ja1_US1CT}ctf`



<br /><br />
<br /><br />
## [Forensics]: Find The Pass
- - -
### Challenge
> Find the admin Password. Put the flag in rooters{}ctf

Attachments:

- inc0gnito.cap (inc0gnito.pcapにリネームしました)


<br />

### Solution

<pre>
# aircrack-ng inc0gnito.pcap

                                                      Aircrack-ng 1.5.2 


                                         [00:00:01] Tested 50922 keys (got 20006 IVs)

   KB    depth   byte(vote)
    0    0/  1   FF(32256) CE(25856) 0F(25600) 07(24832) 59(24832) 73(24576) 7B(24320) CC(24320) F5(24320) 
    1   41/ 54   00(22272) 11(22016) 1E(22016) 25(22016) 33(22016) 3F(22016) 52(22016) 64(22016) 88(22016) 
    2   11/ 19   AD(23808) EB(23808) F4(23808) 06(23808) 5F(23552) 8A(23552) 98(23552) 29(23552) 58(23296) 
    3    2/  5   BE(26112) 58(25600) A7(25600) EA(25088) BF(24320) 5C(24064) 70(24064) B9(24064) BE(24064) 
    4    0/ 10   EF(28928) 8F(27392) 30(27392) BF(26112) AD(25856) 6A(25856) 04(25344) 19(25344) C5(24832) 

                         KEY FOUND! [ FF:DE:AD:BE:EF ] 
	Decrypted correctly: 100%
</pre>

Wireshark > Protocols > IEEE 802.11 > Decryption KeysでWEP Keyを設定して復号します。

文字列 "admin" をサーチしたら、Basic Authの箇所 (Frame #2209057) がありました。

<pre>
Credentials: admin:blu3_c0rp_p4ss
</pre>

Flag: `rooters{blu3_c0rp_p4ss}ctf`





<br /><br />
<br /><br />
## [Forensics]: Luna - 3
- - -
### Challenge
> Luna - 3 is sending some images of the far side of the moon we need to show the world the unseen part of the moon help us retrieve the images.

Attachments:

- recv_13.10.1959.wav

<br />

### Solution
picoCTF 2019で出てきたm00nwalkと同じ問題です。

RX-SSTV というツールを使いました。<br />
http://users.belgacom.net/hamradio/rxsstv.htm

<img src="https://captureamerica.github.io/writeups/img/Luna3.PNG" alt="Luna3.PNG">

なんて書いてあるのか、よく見えないけどね。。。

Flag: `rooters{Looq_L1x3_Lun4-3}ctf` (予想)


<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後に行った復習です。（他の方のWriteupとか参照してます。）


<br /><br />
## [Forensics]: Frames per Story
- - -
### Challenge
> Finally, Toby is out of the frames. I hope he never returns again.

Attachments:

- 結構な数のjpegファイル

<br />

### Solution
exiftoolをかけると、Comment欄に少しずつ文字が入っているのがわかるので、全ファイルに対してexiftoolをかけて、がっちゃんこします。

<table><tr><td>
<b>$ exiftool * | grep Comment | cut -d: -f2 | tr -d "\n" ; echo </b><br />
 MICHAEL right Jim, your quarterlies look very good. How the thing is going at the library?JIM Oh I told you couldn't close it soMICHAEL So you've come to the master for guidance? (imitating) Is http ny.cc/rosvdz this what you're saying grass hopper?JIM Actually you called me in here, but ye ah.MICHAEL All right, well let me show you how it's done. h ttp c/rosvdz (gets on phone) Yes, I liked to speak to your office manager please. Yes hello this is Michael Scott, I am the regional manager of Dunder Mifflin paper products. Just wanted to talk to you, manager-on- manager. (cut to the office and cut back) All right done deal, thank you very much sir, you're a gentleman and a scholar. Oh I'm sorry, ok, I'm sorry, my mistake. (hangs up) That was a woman I was talking to she had a very low voice. Probably a smoker. So, so that's the way it's done. Michael the camera) I've been in Dundler Mifflin for twelve years, the last four as regional manager. If you want to come through here, (opens the door to the main office) so we have the entire floor, so this is my kingdom, as far as the eye can see, ah this is our receptionist Pam. (goes to the recep tionist) Pam, Pam Pam! http tiny.cc/rosv dz Pam Beesly. Pam has been with us for' for ever, Lavc57.70.100
</td></tr></table>

<br />
URLっぽいの（http ny.cc/rosvdz）があるのでアクセスしてみると、final.pngが手に入ります。

（ここまでは自力でいけました）


<br />
final.pngを開いて、よーーく目を凝らすと、変なデータが見つかります。（オレンジで囲ったところ）

<img src="https://captureamerica.github.io/writeups/img/rooters_final.png" alt="rooters_final.png">

いやー、これはわからんよな。。。


<br />
MacのPreview.appで拡大、選択、コピー、New from Clipboardをして、別ファイルとして保存します。

<img src="https://captureamerica.github.io/writeups/img/rooters_final2.png" alt="rooters_final2.png">


<br />
「青い空を見上げればいつもそこに白い猫」で開いて、ビット抽出前バイト列を確認します。
<img src="https://captureamerica.github.io/writeups/img/rooters_final3.png" alt="rooters_final3.png">


Flag: `rooters{WHY_4R3_TH3_W4Y_TH4T_Y0U_4R3!}ctf`





<br /><br />
<br /><br />
## [Forensics]: Mind Awake Body Asleep!
- - -
### Challenge
> Everything visible has a flip side, like a coin.

Attachments:

- challenge.wav

<br />

### Solution
challenge.wavを再生すると、誰かがモニャモニャ喋っていて、ほんとに眠たくなるファイルでした。

とりあえず、audacityでスペクトラムとか見てみましたが、何も見つからず。

<br />
こういうやつは、LSBをチェックするのがいいらしいです。

LSBを取り出すやつは、picoCTF 2018 の LoadSomeBits で以前書いたコードがあったので、それを流用しました。

いちおう、以下が以前書いたWriteupです。<br />
https://captureamerica.github.io/writeups/post/picoctf_2018/

今後も使うことあるかもしれないので、ファイル名を引き数に取るようにしておきました。

```C
#include <stdio.h>

int sub( int offset, char *fname )
{
	FILE *fp;
	int  c;
	int i;
	char ch;

	if ( ( fp = fopen( fname, "rb" ) ) == NULL ) {
		perror( "Can't fopen" );
		return -1;
        }
	for ( i = 0 ; i < offset ; i++ ) {
		fgetc( fp );
	}

	i = 7;
	ch = 0;
	while ( ( c = fgetc( fp ) ) != EOF ) {
		c = c & 0x01;
		ch = ch | ( c << i);
		i--;
		if ( i < 0 ) {
			if ( ( ch >= 0x20 ) && (ch <= 0x7e ) ) {
				printf( "%c", ch );
			}
			i = 7;
			ch = 0;
		}
	}
	puts("");
        fclose( fp );
	
	return 0;
}

int main( int argc, char **argv )
{
	int i;

	if ( argc != 2 ) {
		printf( "Usage: lsb.o filename\n" );
		return -1;
	}
	for ( i = 0 ; i < 8 ; i++ ) {
		sub( i, argv[1] );
	}
}
```

<br />
出力がやたらと多いので、もうちょっと改良した方がいいかもだけど。

<pre>
$ ./lsb.o challenge.wav | fold -120 | grep -i http
-@@RMr. Robot is an American drama thriller television series created by Sam Esmail. http://tiny.cc/72nwdz##############
</pre>

長い文字列が1行で出てくるので、foldで適当に区切って、その中で"http"やら"ctf"やらのキーワードになりそうなものをサーチする感じですかね。


<br />
見つかった http://tiny.cc/72nwdz にアクセスすると、newというテキストファイルが入手できます。

<pre>
$ file new 
new: ASCII text, with very long lines, with no line terminators

$ cat new | fold -32 | tail -n 4 ; echo
81c9a9001031b0000031b00000379584
0790000000457e3cec00000060800610
00003950000025448494d0000000a0a1
a0d074e40598
</pre>

<br />
この末尾の文字列から、PNGヘッダ(89 50 4E 47 0D 0A 1A 0A)が思いつくかどうかと言われたら、普通思いつかないですよね。

問題文にある`flip`でそれに気づくかどうかかな。

<br />
リバースしてバイナリとして保存します。

<pre>
$ python -c 'import binascii; open("output", "wb").write(binascii.unhexlify(open("new", "rb").read()[::-1]))'
$ file output
output: PNG image data, 1427 x 352, 8-bit/color RGBA, non-interlaced
</pre>

<img src="https://captureamerica.github.io/writeups/img/rooters_python_is_awesome.png" alt="rooters_python_is_awesome.png">

Flag: `rooters{pyth0n_is_aw3s0m3}ctf`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

