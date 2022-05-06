---
title: "ångstromCTF 2022 Writeup"
date: 2022-05-06T16:00:00+09:00
lastmod: 2022-05-06T16:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fangstromctf_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2022.angstromctf.com/challenges](https://2022.angstromctf.com/challenges)
<br /><br />

OSCP の勉強をしていて、自分のWeb関連のスキルが弱いと感じたので、

まだ OSCP の勉強の真っ最中ですが、CTFのWeb問題だけは時々トライしていこうと思います。

<br>
いちおう、210点を獲得し、順位は814位でした。<br />
<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_score.png" alt="angstromctf_2022_score.png">

<br>

以下は解いたチャレンジです。（相変わらず、ほとんど解けてないです。。。）<br />
<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_solves.png" alt="angstromctf_2022_solves.png">

<br>

Xtra Salty Sardines でやったことは、自分で今後も参照することもありそうなので、writeup残しておきます。

<br /><br />
## [Web]: Xtra Salty Sardines (70 points)
- - -
### Challenge
> Clam was intensely brainstorming new challenge ideas, when his stomach growled! He opened his favorite tin of salty sardines, took a bite out of them, and then got a revolutionary new challenge idea. What if he wrote [a site with an extremely suggestive acronym](https://xtra-salty-sardines.web.actf.co/)?
<br /><br />
[Admin Bot](https://admin-bot.actf.co/xtra-salty-sardines)

Attachment:

- index.js

中身：

```Javascript
const express = require("express");
const path = require("path");
const fs = require("fs");
const cookieParser = require("cookie-parser");

const app = express();
const port = Number(process.env.PORT) || 8080;
const sardines = {};

const alpha = "abcdefghijklmnopqrstuvwxyz";

const secret = process.env.ADMIN_SECRET || "secretpw";
const flag = process.env.FLAG || "actf{placeholder_flag}";

function genId() {
    let ret = "";
    for (let i = 0; i < 10; i++) {
        ret += alpha[Math.floor(Math.random() * alpha.length)];
    }
    return ret;
}

app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

// the admin bot will be able to access this
app.get("/flag", (req, res) => {
    if (req.cookies.secret === secret) {
        res.send(flag);
    } else {
        res.send("you can't view this >:(");
    }
});

app.post("/mksardine", (req, res) => {
    if (!req.body.name) {
        res.status(400).type("text/plain").send("please include a name");
        return;
    }
    // no pesky chars allowed
    const name = req.body.name
        .replace("&", "&amp;")
        .replace('"', "&quot;")
        .replace("'", "&apos;")
        .replace("<", "&lt;")
        .replace(">", "&gt;");
    if (name.length === 0 || name.length > 2048) {
        res.status(400)
            .type("text/plain")
            .send("sardine name must be 1-2048 chars");
        return;
    }
    const id = genId();
    sardines[id] = name;
    res.redirect("/sardines/" + id);
});

app.get("/", (req, res) => {
    res.sendFile(path.join(__dirname, "index.html"));
});


app.get("/sardines/:sardine", (req, res) => {
    const name = sardines[req.params.sardine];
    if (!name) {
        res.status(404).type("text/plain").send("sardine not found :(");
        return;
    }
    const sardine = fs
        .readFileSync(path.join(__dirname, "sardine.html"), "utf8")
        .replaceAll("$NAME", name.replaceAll("$", "$$$$"));
    res.type("text/html").send(sardine);
});

app.listen(port, () => {
    console.log(`Server listening on port ${port}.`);
});
```


<br />
### Solution
イワシの缶に名前が付けられるようになっています。かなり塩っぱそう（extra salty）。

XSSですね。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_mksardine.png" alt="angstromctf_2022_mksardine.png">

<br />

以下はAdmin Botです。URLを指定すると、アクセスしてくれるみたいです。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_adminbot.png" alt="angstromctf_2022_adminbot.png">

<br />

ソースコードを見ると、いくつかの特殊文字はエスケープされるみたいです。
```javascript
        .replace("&", "&amp;")
        .replace('"', "&quot;")
        .replace("'", "&apos;")
        .replace("<", "&lt;")
        .replace(">", "&gt;");
```


とりあえず、"&lt;h2&gt;aa&lt;/h2&gt;" を入れてみたところ、どうやらフィルターは同じ文字に対して一回だけかかるみたいです。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_XSS_filter.png" alt="angstromctf_2022_XSS_filter.png">

フィルターは簡単に回避できそう。

<br />

フラグは、Admin Bot のみアクセスできるようになっています。

```Javascript
// the admin bot will be able to access this
app.get("/flag", (req, res) => {
    if (req.cookies.secret === secret) {
        res.send(flag);
    } else {
        res.send("you can't view this >:(");
    }
});
```

<br />

iframeを2つ用意して、iframe1でフラグへのアクセス、iframe2ではiframe1の値を取り出して外部に送信するXSSを用意します。

イメージとしては、こんな感じです。[https://beeceptor.com/](https://beeceptor.com/) で書いたルールです。

（結果として、これじゃダメなんですが）

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_XSS_beeceptor_rule.png" alt="angstromctf_2022_XSS_beeceptor_rule.png">


<br />

XSSは、以下の通りです。最初の3文字は、フィルター回避用です。

これを、イワシの缶の名前としてセットして、URLを生成します。その後、そのURLをiframe2にセットします。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_XSS.png" alt="angstromctf_2022_XSS.png">


<br />

で、このiframe1とiframe2を含むページをどこでホストするかなんですが、[https://beeceptor.com/](https://beeceptor.com/) でやってしまうと Cookie が iframe1 に適用されずに Admin Botでもフラグが取れなくなってしまいます。

<br />

なので、そのページ自体も、イワシの缶の名前を使って生成します。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_XSS2.png" alt="angstromctf_2022_XSS2.png">

<br />

ブラウザでアクセスしてみると、こんな感じです。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_XSS_browser.png" alt="angstromctf_2022_XSS_browser.png">

<br />

これを、Admin Bot にアクセスさせると、フラグが得られます。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2022_XSS_flag.png" alt="angstromctf_2022_XSS_flag.png">

<br />

Flag: `actf{those_sardines_are_yummy_yummy_in_my_tummy}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

