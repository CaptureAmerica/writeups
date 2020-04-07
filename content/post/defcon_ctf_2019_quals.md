---
title: "DEF CON CTF 2019 Quals Writeup"
date: 2019-05-11T00:00:00+08:00
lastmod: 2019-05-25T14:05:00+08:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdefcon_ctf_2019_quals%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

## [Web]: cant_even_unplug_it
- - -
### Challenge
> You know, we had this up and everything. Prepped nice HTML5, started deploying on a military-grade-secrets.dev subdomain, got the certificate, the whole shabang. Boss-man got moody and wanted another name, we set up the new names and all. Finally he got scared and unplugged the server. Can you believe it? Unplugged. Like that can keep it secret…
<br /><br />
> Hint: these are HTTPS sites. Who is publicly and transparently logging the info you need?
<br /><br />
> Just in case: all info is freely accessible, no subscriptions are necessary. The names cannot really be guessed. 

<br />
### Solution
簡単に訳すと、

> military-grade-secrets.dev サブドメインを使ってサイトをがんばって作って立ち上げたのに、ボスが別の名前がいいとか言いだして、名前を変えてやったのに、結局なんか怖くなってサイトを落としたよ。

みたいなストーリー。

[VirusTotal](https://www.virustotal.com/#/domain/military-grade-secrets.dev) でサーチしたところ、以下の2つのFQDNが見つかりました。

- secret-storage.military-grade-secrets.dev
- now.under.even-more-militarygrade.pw.military-grade-secrets.dev

早速ブラウザでアクセスしてみると、

`This site can’t be reached`
<br />
`forget-me-not.even-more-militarygrade.pw refused to connect.`


<br />
wgetだと、こんな感じ。
<pre>
~ $ wget secret-storage.military-grade-secrets.dev
--2019-05-25 14:36:49--  http://secret-storage.military-grade-secrets.dev/
Resolving secret-storage.military-grade-secrets.dev (secret-storage.military-grade-secrets.dev)... 172.217.24.115
Connecting to secret-storage.military-grade-secrets.dev (secret-storage.military-grade-secrets.dev)|172.217.24.115|:80... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://forget-me-not.even-more-militarygrade.pw [following]
--2019-05-25 14:36:49--  https://forget-me-not.even-more-militarygrade.pw/
Resolving forget-me-not.even-more-militarygrade.pw (forget-me-not.even-more-militarygrade.pw)... 206.189.162.22
Connecting to forget-me-not.even-more-militarygrade.pw (forget-me-not.even-more-militarygrade.pw)|206.189.162.22|:443... failed: Connection refused.
</pre>

リダイレクトによって、別の名前のサイトに飛ぶようになってますね。
<br /><br />

最初の説明文にあるように、サーバはすでに落ちているのでアクセスできないですが、ウェブのキャッシュをみたらどんなサイトだったか確認ができます。


以下の、ウェブアーカイブを使いました。<br />
https://web.archive.org/web/20190427160922/https://forget-me-not.even-more-militarygrade.pw/

<br />
**Flag:** OOO{DAMNATIO_MEMORIAE}

<br />
＃HTTPSなんじゃらのヒント、使ってないや。。。

- - -
<br /><br />
<br /><br />