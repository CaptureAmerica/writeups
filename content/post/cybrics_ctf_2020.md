---
title: "CyBRICS CTF 2020 Writeup"
date: 2020-07-26T18:00:00+09:00
lastmod: 2020-07-26T18:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fcybrics_ctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://cybrics.net/](https://cybrics.net/)
<br /><br />
[1年前](https://captureamerica.github.io/writeups/post/cybrics_ctf_2019/)もほとんど解けなかったですが、今年もほとんど解けませんでした。

まぁ、去年は1問だけで、今年は4問解いたので、多少は成長したかな。。。

<br>

[youtube.com/SPbCTF](youtube.com/SPbCTF) にて、Writeup を配信してくれるみたいです。Thank you!

<br>

最終順位は、275位でした。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2020_Score1.png" alt="cybrics_CTF_2020_Score1.png">


チェックが付いているやつが、解けているやつです。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2020_Score2.png" alt="cybrics_CTF_2020_Score2.png">



<br /><br />
## [Cyber, Baby]: Mic Check
- - -
### Challenge
> Have you read the game rules? There's a flag there. But this year it's ENCRYPTED, the same way as UserAssist values in Windows.

<br />

### Solution
Ruleに出てくるのは、`cybrics{Na5JRe_g0_G3u_Z1P_Pu3PX}` です。

WindowsのUserAssistについていろいろググってみましたが、単純にRot (Rot 13) のようですね。

<br />
Flag: `cybrics{An5WEr_t0_T3h_M1C_Ch3CK}`



<br /><br />
<br /><br />
## [Forensic, Eazy]: Krevedka
- - -
### Challenge
> Some user of our service hacked another user.
<br /><br />
Name of the victim user was 'caleches'. But we don't know the real login of the attacker. Help us find it!
<br /><br />
Flag format: cybrics{login of the attacker}

Attachment:

- selected_packets.pcapng

<br />

### Solution
`caleches` で grep してみたところ、3つ目のパスワードが怪しいのがわかります。これが attacker が生成したトラフィックです。

<pre>
$ strings selected_packets.pcapng | grep caleches
login=caleches&password=vixie
login=caleches&password=vixie
login=caleches&password=%22+or+1%3D1+--
</pre>

Wiresharkで開き、display filter `urlencoded-form.value == "caleches"` を使って上記のトラフィックを見つけます。

3つ目のトラフィックは Frame 536633 で、User-Agent がユニークなのがわかります。

<pre>
POST /login HTTP/1.1
Host: kr3vedko.com
User-Agent: UCWEB/2.0 (Linux; U; Opera Mini/7.1.32052/30.3697; www1.smart.com.ph/; GT-S5360) U2/1.0.0 UCBrowser/9.8.0.534 Mobile
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Cookie: session=b75d53bb-1326-4d78-aedf-9bd92e237fbf
Content-Length: 39
Content-Type: application/x-www-form-urlencoded
</pre>

<br>

Wiresharkにて、display filter `http.user_agent == "UCWEB/2.0 (Linux; U; Opera Mini/7.1.32052/30.3697; www1.smart.com.ph/; GT-S5360) U2/1.0.0 UCBrowser/9.8.0.534 Mobile"` でフィルターすると、"login" = "micropetalous" が見つかります。

<br />

Flag: `cybrics{micropetalous}`


<br /><br />
<br /><br />
## [Web, Baby]: Hunt
- - -
### Challenge
> I couldn't not make this web10
<br /><br />
[http://109.233.57.94:54040/](http://109.233.57.94:54040/)

こんな感じで、5つのキャプチャが飛び回っているページです。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2020_Hunt.png" alt="cybrics_CTF_2020_Hunt.png">


<br />

### Solution
Developer Toolを使ってスマートに解けないか考えたけどさっぱりわからなかったです。

解いている人数が結構多かったので、アタマを使わずに解ける問題だと推測し、動き回っているキャプチャを頑張ってクリックする方針にしました。

案の定、5つクリックできるとフラグがもらえます。

<br/>
単なるゲームですな。（あと、何気にCPUを食います）


<br />

Flag: `cybrics{Th0se_c4p7ch4s_c4n_hunter2_my_hunter2ing_hunter2}`



<br /><br />
<br /><br />
## [Reverse, Baby]: Baby Rev
- - -
### Challenge
> I started teaching my daugther some reversing. She is capable to solve this crackme. Are you?
<br /><br />
This link can be helpful: snap.berkeley.edu/offline

Attachment:

- babyrev.tar.gz (babyrev.xml)


<br />

### Solution
Scratchみたいなやつですね。

[https://snap.berkeley.edu/snap/snap.html](https://snap.berkeley.edu/snap/snap.html) に行って、babyrev.xml を読み込むと、以下のコードが見つかります。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2020_Snap.png" alt="cybrics_CTF_2020_Snap.png">

secret に設定された値を、それぞれ 33 で XOR したら良さそうです。

なお、`show variable secret` と `pause all` を使って、secretには値が逆順に入っていることを確認してます。

<br />

<pre>
$ python -c 'print("".join([chr(int(x) ^ 33) for x in "66 88 67 83 72 66 82 90 86 18 77 16 98 17 76 18 126 97 79 69 126 102 17 17 69 126 77 116 66 74 0 92".split()]))'
</pre>


<br />

Flag: `cybrics{w3l1C0m3_@nd_G00d_lUck!}`





<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
