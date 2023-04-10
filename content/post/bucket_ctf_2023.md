---
title: "BUCKET CTF 2023 Writeup"
date: 2023-04-10T10:00:00+09:00
lastmod: 2023-04-10T10:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbucket_ctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.ebucket.dev/challenges](https://ctf.ebucket.dev/challenges)
<br /><br />

1636ポイントで、最終順位は196番でした。

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_score.png" alt="bucket_ctf_2023_score.png">

<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_web.png" alt="bucket_ctf_2023_web.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_misc.png" alt="bucket_ctf_2023_misc.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_crypto.png" alt="bucket_ctf_2023_crypto.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_rev.png" alt="bucket_ctf_2023_rev.png"> <br />

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_pwn.png" alt="bucket_ctf_2023_pwn.png"> <br />

<br /><br />

MiscとWebを少々解いただけです。。。

自分用のメモとして、2つwriteup残します。


## [Misc]: Transmission (278 points)
- - -
### Challenge
> The United States space force was one day containing routine tests on intergalactic light when they captured a random beam of light. Senior General Hexy Pictora believes this beam of light may actually be a new communication method used by aliens. Analyze the image to find out of any secrets are present.

Attachments:

- beamoflight.png

<img src="https://captureamerica.github.io/writeups/img/beamoflight.png" alt="beamoflight.png"> <br />

<br />
### Solution

「青い空を見上げればいつもそこに白い猫」のビット抽出で、全てのビットをオンにしてバイナリデータの表示をすると、フラグが見つかります。

<img src="https://captureamerica.github.io/writeups/img/bucket_ctf_2023_neko.png" alt="bucket_ctf_2023_neko.png">

<br />

Flag: `bucket{d3c0d3_th3_png_f7c74c1dc7}`



<br /><br />
<br /><br />
## [Misc]: clocks (358 points)
- - -
### Challenge
> One of my cybersecurity professors, Dr. Timely, randomly sent my this file and said if I can decode the message he will give me an A in the class. Can you help me out?

Attachments:

- clocks_medium.pcap


<br />
### Solution

Wiresharkで開いてみると、全く同じ ping request が並んでいて、違うのは Timestamp のみです。

チャレンジ文やタイトルからも、Timestampに注目すべきなのは分かります。

表示を、Seconds Since Previous Displayed Packetにすると、0.1秒と0.5秒の2値に分かれているので、2進数を文字列に変えてフラグが得られると予想できます。

<pre>
$ tshark -r clocks_medium.pcap -T fields -e frame.time_delta_displayed | cut -c -3
0.0
0.1
0.5
0.5
0.1
0.1
0.1
0.5
0.1
0.1
0.5
:
(以下、略)
</pre>


<br />

`0.1`を`0`に、`0.5`を`1`に変えて、あとはそのまま CyberChef で文字列に変換しました。

<br />

Flag: `bucket{look_at_the_times_sometimes}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
