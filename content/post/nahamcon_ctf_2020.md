---
title: "NahamCon CTF 2020 Writeup"
date: 2020-06-14T11:00:00+09:00
lastmod: 2020-06-17T21:25:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fnahamcon_ctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2020/06/17 - 復習しました)

URL: [https://ctf.nahamcon.com/challenges](https://ctf.nahamcon.com/challenges)

土曜日にある程度やって、BinaryとScriptingは日曜日にゆっくりやろうと思ったら、イベントが終わってました。残念。。。

（やったところで、大して解けなかったかもですけど）

<br />
John Hammond がオーガナイザーのひとりだったみたいですね。間接的に、いろいろお世話になっております（YouTubeビデオとか、[ctf-katana](https://github.com/JohnHammond/ctf-katana)とか）。Thanks!

<br />
最終順位は、419位でした。

<img src="https://captureamerica.github.io/writeups/img/nahamcon_Score1.png" alt="nahamcon_Score1.png">

<br />
チャレンジリスト

<img src="https://captureamerica.github.io/writeups/img/nahamcon_Score2.png" alt="nahamcon_Score2.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/nahamcon_Score3.png" alt="nahamcon_Score3.png">
<br />
<img src="https://captureamerica.github.io/writeups/img/nahamcon_Score4.png" alt="nahamcon_Score4.png">



<br /><br />
## [Web]: Agent 95 (50 points)
- - -
### Challenge
> They've given you a number, and taken away your name~
<br /><br />
Connect here:
[http://jh2i.com:50000](http://jh2i.com:50000)

<br />
### Solution
アクセスすると、以下の文章が表示されます。

<pre>
You don't look like our agent!
We will only give our flag to our Agent 95! He is still running an old version of Windows...
</pre>

チャレンジ名から想像できる通り、User-Agentを設定してアクセスするやつです。

<br />
"Windows 95" "User-Agent" あたりをキーワードにググって、テキトーなUser-Agentを見つけます。

以下でフラグが取れました。
<pre>
wget -O - "http://jh2i.com:50000" --user-agent="Mozilla/4.0 (compatible; MSIE 5.5; Windows 95; BCD2000)"
</pre>

<br />

Flag: `flag{user_agents_undercover}`



<br /><br />
<br /><br />
## [Web]: Localghost (75 points)
- - -
### Challenge
> BooOooOooOOoo! This spooOoOooky client-side cooOoOode sure is scary! What spoOoOoOoky secrets does he have in stooOoOoOore??
<br /><br />
Connect here:
[http://jh2i.com:50003/](http://jh2i.com:50003/)
<br /><br />
Note, this flag is not in the usual format.


<br />
### Solution
ChromeのDeveloper Toolでjsファイルをテキトーに追っていったら、フラグが見つかりました。

<br />

Flag: `JCTF{spoooooky_ghosts_in_storage}`



<br /><br />
<br /><br />
## [Web]: Phphonebook (100 points)
- - -
### Challenge
> Ring ring! Need to look up a number? This phonebook has got you covered! But you will only get a flag if it is an emergency!
<br /><br />
Connect here:
[http://jh2i.com:50002](http://jh2i.com:50002)


<br />
### Solution
アクセスすると以下が表示されました。

<pre>
Sorry! You are in /index.php/?file=
The phonebook is located at phphonebook.php
</pre>


<br />
以下でアクセスしたら、phphonebook.phpの中身が見れました。
<br />
[http://jh2i.com:50002/index.php/?file=php://filter/convert.base64-encode/resource=phphonebook](http://jh2i.com:50002/index.php/?file=php://filter/convert.base64-encode/resource=phphonebook)

<pre>
PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KICA8aGVhZD4KICAgIDxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4KICAgIDx0aXRsZT5QaHBob25lYm9vazwvdGl0bGU+CiAgICA8bGluayBocmVmPSJtYWluLmNzcyIgcmVsPSJzdHlsZXNoZWV0Ij4KICA8L2hlYWQ+CgogIDxib2R5IGNsYXNzPSJiZyI+CiAgICA8aDEgaWQ9ImhlYWRlciI+IFdlbGNvbWUgdG8gdGhlIFBocGhvbmVib29rIDwvaDE+CgogICAgPGRpdiBpZD0iaW1fY29udGFpbmVyIj4KCiAgICAgIDxpbWcgc3JjPSJib29rLmpwZyIgd2lkdGg9IjUwJSIgaGVpZ2h0PSIzMCUiLz4KCiAgICAgIDxwIGNsYXNzPSJkZXNjIj4KICAgICAgVGhpcyBwaHBob25lYm9vayB3YXMgbWFkZSB0byBsb29rIHVwIGFsbCBzb3J0cyBvZiBudW1iZXJzISBIYXZlIGZ1bi4uLgogICAgICA8L3A+CgogICAgPC9kaXY+Cjxicj4KPGJyPgogICAgPGRpdj4KICAgICAgPGZvcm0gbWV0aG9kPSJQT1NUIiBhY3Rpb249IiMiPgogICAgICAgIDxsYWJlbCBpZD0iZm9ybV9sYWJlbCI+RW50ZXIgbnVtYmVyOiA8L2xhYmVsPgogICAgICAgIDxpbnB1dCB0eXBlPSJ0ZXh0IiBuYW1lPSJudW1iZXIiPgogICAgICAgIDxpbnB1dCB0eXBlPSJzdWJtaXQiIHZhbHVlPSJTdWJtaXQiPgogICAgICA8L2Zvcm0+CiAgICA8L2Rpdj4KCiAgICA8ZGl2IGlkPSJwaHBfY29udGFpbmVyIj4KICAgIDw/cGhwCiAgICAgIGV4dHJhY3QoJF9QT1NUKTsKCiAgICAJaWYgKGlzc2V0KCRlbWVyZ2VuY3kpKXsKICAgIAkJZWNobyhmaWxlX2dldF9jb250ZW50cygiL2ZsYWcudHh0IikpOwogICAgCX0KICAgID8+CiAgPC9kaXY+CiAgPC9icj4KICA8L2JyPgogIDwvYnI+CgoKPGRpdiBzdHlsZT0icG9zaXRpb246Zml4ZWQ7IGJvdHRvbToxJTsgbGVmdDoxJTsiPgo8YnI+PGJyPjxicj48YnI+CjxiPiBOT1QgQ0hBTExFTkdFIFJFTEFURUQ6PC9iPjxicj5USEFOSyBZT1UgdG8gSU5USUdSSVRJIGZvciBzdXBwb3J0aW5nIE5haGFtQ29uIGFuZCBOYWhhbUNvbiBDVEYhCjxwPgo8aW1nIHdpZHRoPTYwMHB4IHNyYz0iaHR0cHM6Ly9kMjR3dXE2bzk1MWkyZy5jbG91ZGZyb250Lm5ldC9pbWcvZXZlbnRzL2lkLzQ1Ny80NTc3NDgxMjEvYXNzZXRzL2Y3ZGEwZDcxOGViNzdjODNmNWNiNjIyMWEwNmEyZjQ1LmludGkucG5nIj4KPC9wPgo8L2Rpdj4KCiAgPC9ib2R5Pgo8L2h0bWw+
</pre>

<br />
抜粋です。

```HTML
<br>
    <div>
      <form method="POST" action="#">
        <label id="form_label">Enter number: </label>
        <input type="text" name="number">
        <input type="submit" value="Submit">
      </form>
    </div>

    <div id="php_container">
    <?php
      extract($_POST);

    	if (isset($emergency)){
    		echo(file_get_contents("/flag.txt"));
    	}
    ?>
```

<br />
emergencyをPOSTしたらいいみたいです。チャレンジの本文からGuessするだけでも解けたかもです。

<pre>
$ curl -d emergency=1 http://jh2i.com:50002/phphonebook.php
</pre>

<br />

Flag: `flag{phon3_numb3r_3xtr4ct3d}`



<br /><br />
<br /><br />
## [Scripting]: Rotten (100 points)
- - -
### Challenge
> Ick, this salad doesn't taste too good!
<br /><br />
Connect with:
nc jh2i.com 50034


<br />
### Solution
イベント終了後でしたが、サーバは生きていたのでやってみました。

手動で繋いでみると、以下のような結果になります。

<pre>
$ nc jh2i.com 50034
send back this line exactly. no flag here, just filler.
send back this line exactly. no flag here, just filler.
nziy wvxf ocdn gdiz zsvxogt. ij agvb czmz, epno adggzm.
send back this line exactly. no flag here, just filler.
kwfv tsuc lzak dafw wpsuldq. uzsjsulwj 19 gx lzw xdsy ak 'g'
send back this line exactly. character 19 of the flag is 'o'
:
</pre>

<br />
表示された文をrot(n)して送り返していると、その中にフラグの一文字が含まれているものが出てくる、というものです。

何度か下調べをして、'}' が30文字目（0始まり）に出てくるようなので、それでフラグの長さ(31)がわかります。

<br />
以下、書いたスクリプトです。rot()関数は前から持っていたので、今回書いたのはその下の箇所です。

```Python
#!/usr/bin/env python
from pwn import *

def rot(s, n):
    s = bytearray(s)
    for i, c in enumerate(s):
        if 0x41 <= c <= 0x5a:
            s[i] = ((c-0x41+n) % 0x1a) + 0x41
        elif 0x61 <= c <= 0x7a:
            s[i] = ((c-0x61+n) % 0x1a) + 0x61
    return s

flag_len = 31
flag = [""] * flag_len
count = 0
s = remote('jh2i.com', 50034)
while 1:
    q = s.recvline()
    for n in xrange(26):
        a = rot(q, n)
        if "character" in a:
            result = (re.findall(r'[0-9]+', a))
            pos = int(result[0])
            if flag[pos] != chr(a[-3]):
                flag[pos] = chr(a[-3])
                print("".join(flag))
                count += 1
            #print("char = {}".format(chr(a[-3])))
        if "send" in a:
            s.sendline(a)
            break
    if count >= flag_len:
        break
s.close()
```

<br />
解説:

- 実際に見つかった文字は、flagというリストに入れていってます。
- rotした結果、"send"とか"character"が見つかったやつが、正しくrotされたものです。
- ちょっとナゾだったのが、aには文字が入っているはずなのにa[-3]は数値で取れてたので、chr()で文字に変換してます。
- [-3]なのは、改行も入れて後ろから3文字目だからです。

<br />
実行結果:

<pre>
$ ./rotten_solve.py
[+] Opening connection to jh2i.com on port 50034: Done
r
lr
lyr
loyr
floyr
floyur
floyur}
floyurr}
floyurer}
floyourer}
floyourers}
floyou_rers}
fl{oyou_rers}
fl{noyou_rers}
fl{noyou_rcers}
fl{noyou_rcesrs}
fl{no_you_rcesrs}
fl{no_youo_rcesrs}
fl{no_youo_yrcesrs}
fl{no_youko_yrcesrs}
fl{no_youkow_yrcesrs}
fl{no_youkow_yurcesrs}
flg{no_youkow_yurcesrs}
flg{now_youkow_yurcesrs}
flg{now_youkow_yurcaesrs}
flag{now_youkow_yurcaesrs}
flag{now_youkow_yourcaesrs}
flag{now_you_kow_yourcaesrs}
flag{now_you_know_yourcaesrs}
flag{now_you_know_yourcaesars}
flag{now_you_know_your_caesars}
[*] Closed connection to jh2i.com port 50034
</pre>

<br />

Flag: `flag{now_you_know_your_caesars}`




<br /><br />
<br /><br />
## [Misc]: Vortex (75 points)
- - -
### Challenge
> Will you find the flag, or get lost in the vortex?
<br /><br />
Connect here:
nc jh2i.com 50017


<br />
### Solution
Wiresharkでキャプチャーしながら繋いで、Wiresharkでflagをサーチしました。

<br />

Flag: `flag{more_text_in_the_vortex}`



<br /><br />
<br /><br />
## [Misc]: Fake File (100 points)
- - -
### Challenge
> Wait... where is the flag?
<br /><br />
Connect here:
nc jh2i.com 50026



<br />
### Solution
｢..｣ というファイル名の中身をどうやって表示させるか、というチャレンジです。

<pre>
$ nc jh2i.com 50026
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
user@host:/home/user$ ls -la
ls -la
total 12
dr-xr-xr-x 1 nobody nogroup 4096 Jun 12 17:10 .
drwxr-xr-x 1 user   user    4096 Jun  4 18:54 ..
-rw-r--r-- 1 user   user      52 Jun 12 17:10 .. 

user@host:/home/user$ cat ".."
cat ".."
cat: ..: Is a directory

user@host:/home/user$ emacs
emacs
bash: emacs: command not found
</pre>

<br />
以下でフラグをゲットしました。

<pre>
user@host:/home/user$ find . -type f -exec cat {} \;
find . -type f -exec cat {} \;
flag{we_should_have_been_worried_about_u2k_not_y2k}
</pre>

<br />

Flag: `flag{we_should_have_been_worried_about_u2k_not_y2k}`



<br /><br />
<br /><br />
## [Misc]: Alkatraz (100 points)
- - -
### Challenge
> We are so restricted here in Alkatraz. Can you help us break out?
<br /><br />
Connect here:
nc jh2i.com 50024


<br />
### Solution
確か、catとかいろいろなコマンドが使えないやつでした。

<pre>
$ nc jh2i.com 50024
ls -la
total 24
dr-xr-xr-x 1 nobody nogroup 4096 Jun 12 17:09 .
drwxr-xr-x 1 user   user    4096 Jun  4 18:54 ..
-rw-r--r-- 1 nobody nogroup  220 Jun  4 18:54 .bash_logout
-rw-r--r-- 1 nobody nogroup 3771 Jun  4 18:54 .bashrc
-rw-r--r-- 1 nobody nogroup  807 Jun  4 18:54 .profile
-rw-r--r-- 1 user   user      41 Jun 12 17:09 flag.txt

cat flag.txt
/bin/rbash: line 2: cat: command not found
</pre>


<br />
以下でフラグをゲットしました。

<pre>
while read line; do echo $line; done < flag.txt
flag{congrats_you_just_escaped_alkatraz}
</pre>


<br />

Flag: `flag{congrats_you_just_escaped_alkatraz}`



<br /><br />
<br /><br />
## [Mobile]: Candroid (50 points)
- - -
### Challenge
> I think I can, I think I can!

Attachment:

- candroid.apk


<br />
### Solution
解凍して、flag{}を探すだけです。

<pre>
$ unzip candroid.apk

$ find . -name '*' | xargs grep flag 2>/dev/null
Binary file ./classes.dex matches
./META-INF/CERT.SF:Name: res/layout/activity_flag.xml
./META-INF/MANIFEST.MF:Name: res/layout/activity_flag.xml
Binary file ./candroid.apk matches
Binary file ./resources.arsc matches

$ strings resources.arsc | grep flag
flag{4ndr0id_1s_3asy}
</pre>


<br />

Flag: `flag{4ndr0id_1s_3asy}`



<br /><br />
<br /><br />
## [Mobile]: Simple App (50 points)
- - -
### Challenge
> Here's a simple Android app. Can you get the flag?

Attachment:

- simple-app.apk


<br />
### Solution
これも、前述したチャレンジと同様です。今度は、classes.dexの中にあります。

<pre>
$ unzip simple-app.apk

$ find . -name '*' | xargs grep flag 2>/dev/null

$ strings classes.dex | grep flag
</pre>


<br />

Flag: `flag{3asY_4ndr0id_r3vers1ng}`



<br /><br />
<br /><br />
## [Forensics]: Microsooft (100 points)
- - -
### Challenge
> We have to use Microsoft Word at the office!? Oof...

Attachment:

- microsooft.docx


<br />
### Solution
これも、前述したチャレンジと同様ですね。

<pre>
$ unzip microsooft.docx

$ find . -name '*' | xargs grep flag 2>/dev/null
</pre>


<br />

Flag: `flag{oof_is_right_why_gfxdata_though}`



<br /><br />
<br /><br />
## [Steg]: Ksteg (50 points)
- - -
### Challenge
> This must be a typo.... it was kust one letter away!

Attachment:

- luke.jpg


<br />
### Solution
"kust" は "just" のタイポだし、"ksteg" は "jsteg" のことです。


<pre>
$ jsteg reveal luke.jpg 
flag{yeast_bit_steganography_oops_another_typo}
</pre>


<br />

Flag: `flag{yeast_bit_steganography_oops_another_typo}`



<br /><br />
<br /><br />
## [Steg]: Beep Boop (50 points)
- - -
### Challenge
> That must be a really long phone number... right?

Attachment:

- flag.wav


<br />
### Solution
Windowsで動くFree Softwareの「Software DTMF Controller Version 1.2」を使いました。

以下の文字列になります。

`46327402297754110981468069185383422945309689772058551073955248013949155635325`

<br />
以下のオンラインツールでも同じ文字列が取れます。（もしかしたら、サーバダウンしちゃっているかもです。）

http://dialabc.com/sound/detect/index.html

<br />
16進数にしてから文字に変換しました。

`666c61677b646f5f796f755f737065616b5f7468655f626565705f626f6f707d`


<br />

Flag: `flag{do_you_speak_the_beep_boop}`




<br /><br />
<br /><br />
## [OSINT]: Time Keeper (50 points)
- - -
### Challenge
> There is some interesting stuff on this website. Or at least, I thought there was...
<br /><br />
Connect here:
https://apporima.com/
<br /><br />
Note, this flag is not in the usual format.


<br />
### Solution
https://web.archive.org/web/20200418213402/https://apporima.com/flag.txt

<br />
Flag: `JCTF{the_wayback_machine}`




<br /><br />
<br /><br />
## [OSINT]: New Years Resolution (50 points)
- - -
### Challenge
> This year, I resolve to not use old and deprecated nameserver technologies!
<br /><br />
There is nothing running on port 80. This is an OSINT challenge.
<br /><br />
Connect here: jh2i.com


<br />
### Solution
Name resolutionですね。こういうセンスのあるチャレンジ名、いいと思います。

これは、[PassiveTotal](https://www.passivetotal.org/) で見たら簡単にわかりました。SPFレコードを取ればよかったみたいです。

<br />
digでやるなら、

<pre>
$ dig jh2i.com -t SPF

; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> jh2i.com -t SPF
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56537
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;jh2i.com.			IN	SPF

;; ANSWER SECTION:
jh2i.com.		600	IN	SPF	"flag{next_year_i_wont_use_spf}"
</pre>

<br />

Flag: `flag{next_year_i_wont_use_spf}`




<br /><br />
<br /><br />
## [OSINT]: Finsta (50 points)
- - -
### Challenge
> This time we have a username. Can you track down NahamConTron?

<br />
### Solution
Instagram で "NahamConTron" をサーチしたら見つかります。

https://www.instagram.com/nahamcontron/

<br />
Flag: `flag{i_feel_like_that_was_too_easy}`




<br /><br />
<br /><br />
## [Warmup]: CLIsay (20 points)
- - -
### Challenge
> cowsay is hiding something from us!

Attachment:

- clisay


<br />
### Solution
stringsで２分割されたフラグが見つかります。

<pre>
$ strings clisay | egrep "{|}"
flag{Y0u_c4n_
r3Ad_M1nd5}
</pre>

<br />

Flag: `flag{Y0u_c4n_r3Ad_M1nd5}`




<br /><br />
<br /><br />
## [Warmup]: Metameme (25 points)
- - -
### Challenge
> Hacker memes. So meta.

Attachment:

- hackermeme.jpg


<br />
### Solution
exiftool

<br />
Flag: `flag{N0t_7h3_4cTuaL_Cr3At0r}`



<br /><br />
<br /><br />
## [Warmup]: Mr. Robot (25 points)
- - -
### Challenge
> Elliot needs your help. You know what to do.
<br /><br />
Connect here:
[http://jh2i.com:50032](http://jh2i.com:50032)


<br />
### Solution
[http://jh2i.com:50032/robots.txt](http://jh2i.com:50032/robots.txt)

<br />
Flag: `flag{welcome_to_robots.txt}`



<br /><br />
<br /><br />
## [Warmup]: UGGC (30 points)
- - -
### Challenge
> Become the admin!
<br /><br />
Connect here:
[http://jh2i.com:50018](http://jh2i.com:50018)


<br />
### Solution
anonymousでログインすると、`user=nabalzbhf` というCookieが作られました。

Cookieの値を `user=admin` にしたら、`You are logged in as nqzva` と表示されました。

rot13 になるようなので、`user=nqzva` にしたらadminになれます。


<br />
Flag: `flag{H4cK_aLL_7H3_C0okI3s}`



<br /><br />
<br /><br />
## [Warmup]: Easy Keesy (30 points)
- - -
### Challenge
> Dang it, not again...
<br /><br />

Attachment:

- easy_keesy

<pre>
$ file easy_keesy 
easy_keesy: Keepass password database 2.x KDBX
</pre>



<br />
### Solution

<pre>
$ keepass2john easy_keesy > easy_keesy.hash

$ john --format:keepass easy_keesy.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64])
Cost 1 (iteration count) is 100000 for all loaded hashes
Cost 2 (version) is 2 for all loaded hashes
Cost 3 (algorithm [0=AES, 1=TwoFish, 2=ChaCha]) is 0 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
monkeys          (easy_keesy)

</pre>

<br />
WindowsのKeePassを使って、`monkeys`を使ってデータベースを開くとフラグが見つかります。

<br />
Flag: `flag{jtr_found_the_keys_to_kingdom}`




<br /><br />
<br /><br />
## [Warmup]: Pang (40 points)
- - -
### Challenge
> This file does not open!
<br /><br />

Attachment:

- pang

<pre>
$ file pang
pang: PNG image data, 1567 x 70, 8-bit grayscale, non-interlaced
</pre>


<br />
### Solution
PNG imageなので、拡張子に .png を付けました。

確かに Kali 上だと開けなかったんですが、WindowsのPhoto viewerとかで普通に開けたんですよね。

<br />
Flag: `flag{wham_bam_thank_you_for_the_flag_maam}`




<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。


<br /><br />
## [Steg]: Doh (50 points)
- - -
### Challenge
> Doh! Stupid steganography...
<br /><br />
Note, this flag is not in the usual format.

Attachment:

- doh.jpg


<br />
### Solution
「青い空を見上げればいつもそこに白い猫」で開くと 「Steghideの可能性あり」とのことで、steghideを使ってはみたもののpassphraseが見当つかずに諦めたやつです。

<br />
passphrase無しで試すという発想がなかった。。。

<pre>
$ steghide extract -sf doh.jpg 
Enter passphrase: 
wrote extracted data to "flag.txt".

$ cat flag.txt
JCTF{an_annoyed_grunt}
</pre>

<br />

Flag: `JCTF{an_annoyed_grunt}`




<br /><br />
<br /><br />
## [Steg]: Snowflake (75 points)
- - -
### Challenge
> Frosty the Snowman is just made up of a lot of snowflakes. Which is the right one?
<br /><br />
Note, this flag is not in the usual format.

Attachment:

- frostythesnowman.txt


<br />
### Solution
チャレンジタイトルと、添付ファイルより、stegsnowは確定だったんですが、これまたpassphraseが見当つかずに諦めたやつです。

>Frosty the Snowman is just made up of a lot of snowflakes. Which is the right one?

と言っているので、snowflakesの名前の内のひとつがpassphraseなのかと思ってネットでsnowflakesの名前を探していろいろ試したけどダメでした。

<br />
rockyou.txtを使って総当りするという発想は無かった。。。（というか、正直のところ、このチャレンジ文はどうかと思います）


<pre>
$ cat snowflake_solve.sh
while read line
do
    stegsnow -Q -C -p $line frostythesnowman.txt | egrep "{.*}"
done < /usr/share/wordlists/rockyou.txt


$ ./snowflake_solve.sh 
you: No such file or directory
JCTF{gemmy_spinning_snowflake}
^C
</pre>

<br />

Flag: `JCTF{gemmy_spinning_snowflake}`




<br /><br />
<br /><br />
## [Steg]: Old school (125 points)
- - -
### Challenge
> Did Dade Murphy do this?
<br /><br />
Note, this flag is not in the usual format.

Attachment:

- hackers.bmp

<img src="https://captureamerica.github.io/writeups/img/hackers.bmp" alt="hackers.bmp">


<br />
### Solution
画像系はいつもzstegをかけることを心がけているんですが、-aオプションはいつも付けてなかったです。

こんな感じ。

<pre>
$ zsteg hackers.bmp 
imagedata           .. text: "348>?CFGKIJNRSWJKOcbdcbdUTVdceGFHCBD"
[=] nothing :(                    
</pre>


<br />
-aオプションを付けて実行してたら、フラグが取れたようです。

<pre>
$ zsteg -a hackers.bmp | grep -i "{.*}"
b1,bgr,lsb,xy       .. text: "4JCTF{at_least_the_movie_is_older_than_this_software}"
</pre>

<br />

Flag: `4JCTF{at_least_the_movie_is_older_than_this_software}`


<br />
ちなみに、Hackers (邦題：サイバーネット) のDVD持ってます。アンジェリーナ ジョリーがかわいいんですよね。




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

