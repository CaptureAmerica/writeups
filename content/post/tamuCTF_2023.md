---
title: "TAMUctf 2023 Writeup"
date: 2023-05-01T18:00:00+09:00
lastmod: 2023-05-01T18:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Ftamuctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://tamuctf.com/challenges](https://tamuctf.com/challenges)
<br /><br />

TAMUctfは3回目の参加ですが、今回は全く手が出せなかったです。

同時期に開催していたUMDCTFの方が個人的にはあっていたので、そっちをやりました。

<br>
Sanity checkと、Miscの1問を解いただけです。。。

287点を獲得し、順位は376位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2023_Score1.png" alt="tamuctf_2023_Score1.png">

<br /><br />



## [Misc]: Pick Me Up (187 points)
- - -
### Challenge

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2023_pickmeup1.png" alt="tamuctf_2023_pickmeup1.png"><br>

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2023_pickmeup1.jpg" alt="tamuctf_2023_pickmeup1.jpg">


<br />
### Solution

私は少し中国語は読めるんですが、漢字が読める日本人ならチャットの内容は大体理解できるのかな、と思います。

大雑把だけど、だいたい以下のような会話なはず。

「台北に着いたよ〜。そちらはどんな感じ？」

「明日は時間あるよ。IAH空港に迎えに行くね。」

「急いで飛行機に乗らなきゃ」

「ちょっと待って、まだ飛行機の情報教えてもらってないよ。。」

<br />

"EVA IAH 台北 4/20" でググったら、答えは見つかりました。

[https://flightaware.com/live/flight/EVA52/history/20230420/1410Z/RCTP/KIAH](https://flightaware.com/live/flight/EVA52/history/20230420/1410Z/RCTP/KIAH)

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2023_pickmeup3.png" alt="tamuctf_2023_pickmeup3.png">

<br />

Flag: `gigem{00:28-D7}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

