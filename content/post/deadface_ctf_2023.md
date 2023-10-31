---
title: "Deadface CTF 2023 Writeup"
date: 2023-10-31T13:00:00+09:00
lastmod: 2023-10-31T13:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdeadface_ctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://deadface.ctfd.io/challenges](https://deadface.ctfd.io/challenges)
<br /><br />

600点を獲得し、387位でした。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2023_Score1.png" alt="deadface_CTF_2023_Score1.png">

<br><br>

[Deadface CTF 2021](http://localhost:1313/writeups/post/deadface_ctf_2021/) のときと同様、日曜日に見てみたら、追加チャレンジを見る前にイベント終了してました。そもそもあんまりモチベも無かったので、別にいいんですが。


<br><br>
チャレンジのリストです。自分がやったところだけスクリーンショット撮りました。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2023_chall1.png" alt="deadface_CTF_2023_chall1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2023_chall2.png" alt="deadface_CTF_2023_chall2.png">


<br /><br />
## [Steg]: Syncopated Beat (300 points)
- - -
### Challenge
> We know there's a hidden message somewhere here, but none of our steg tools are able to reveal it. Maybe we need to think outside the box?
<br /><br />
It is a well-known fact that rock musicians are all Non-Incarnate Conscious Entities (NICEs) influenced. NICEs speak lyrics to them and insinuate their evil messages into the song.
<br /><br />
Find the flag and enter it like this : flag{Syncopated_Beats_Are_EVIL!!!}

Attachment:

- syncopated_beat.zip

<br />
### Solution
Zipファイルの中には、以下の2つの.wavファイルが入っています。

<pre>
$ zipinfo syncopated_beat.zip
Archive:  syncopated_beat.zip
Zip file size: 108457666 bytes, number of entries: 2
-rw-rw-r--  3.0 unx 35986500 bx defN 23-Aug-21 06:14 mysterious_music.wav
-rw-rw-r--  3.0 unx 74699518 bx defN 23-Aug-21 06:22 Syncopated-Beat-2023.wav
</pre>

<br><br>
Syncopated-Beat-2023.wav の方を聞いてみると、途中で人の声のようなものがあり、Audacityでその部分を選択して Effect -> Reverse すると英語として聞こえるようになります。

自分は、英語のネイティブ スピーカーではないので完全にすべて聞き取れなかったのですが、大体以下のような感じです。

<pre>
Time is reversible but the music is not.
Go back go back!
Hey I remember you!
Well that won't be that easy this year.
You need to get a stego program used by Elliot XXX Mr.Robot.
Use it on the audio file linked to the challenge.
The password is the name of the new CTO of the evil corp, all caps no spaces.
</pre>

<br><br>
ちなみに、「Mr.Robot」は Neflix でしばらく観てたのですが、Elliotの妄想を見ているのかな何なのかわからくなってきて、途中で挫折しました。

<br><br>
さて本題ですが、ググってみたところ、`DeepSound` というWindowsのアプリを使えばよさそうです。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2023_Syncopated_Beat1.png" alt="deadface_CTF_2023_Syncopated_Beat1.png">

<br><br>
Evil corpの新しいCTOの名前は、`TYRELLWELLICK` でした。（実際には、TyrellよりもScottの方が新しいっぽい？）

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2023_Syncopated_Beat2.png" alt="deadface_CTF_2023_Syncopated_Beat2.png">

<br><br>
Jpegファイルを extract すると、そこにフラグが書かれています。

<br />

Flag: `flag{Lookout_COVID_BATZ!!!}`


<br /><br />
<br /><br />
## [Traffic Analysis]: Sometimes IT Lets You Down (25 points)
- - -
### Challenge
> Lytton Labs has been having a slew of weird activity in the network lately. This recent PCAP capture we know contains a user account who compromised our domain controller. Can you figure out what user account was compromised?
<br /><br />
Submit the flag as: flag{username}.

Attachment:

- ITletsyoudown.pcapng

<br />
### Solution
Wiresharkで "user" をサーチしていると、そのうち `Users\mmeyers\` のような文字列が見つかります。

<br />

あるいは、strings + grep でも見つけられます。

<pre>
$ strings ITletsyoudown.pcapng | grep -i user | grep -v -i admin | grep -v -i Agent
:
(snip)
:
Users\mmeyers\Desktop\*.doc*
Users\mmeyers\Desktop\*.txt
Users\mmeyers\Desktop\*.lnk
:
</pre>

<br>

Flag: `flag{mmeyers}`



<br /><br />
<br /><br />
## [Traffic Analysis]: Git Rekt (50 points)
- - -
### Challenge
> One of our teammates at Turbo Tactical ran a phishing campaign on spookyboi and thinks spookyboi may have submitted credentials. We need you to take a look at the PCAP and see if you can find the credentials.
<br /><br />
Submit the password as the flag: flag{password}.

Attachment:

- pcap-20231010.pcap

<br />
### Solution
このpcapの中で見つかる HTTP POST は、1つだけです。

Follow HTTP Streamをして中身を見たら、`password=SpectralSecrets%232023` が見つかります。

<br>

Flag: `flag{SpectralSecrets#2023}`



<br /><br />
<br /><br />
## [Traffic Analysis]: Creepy Crawling (75 points)
- - -
### Challenge
> One of our clients, TGRI, had an SSH server compromised at one of their smaller remote locations. Their only security analyst was fired and “accidentally” deleted information specific to the attack. Thankfully, TGRI still has the PCAP that captured the SSH brute force attack. What SSH protocol did TGRI run that was eventually compromised by DEADFACE?
<br /><br />
Submit the SSH protocol as the flag: flag{SSH-1.1.1: Simple SSH Server}

Attachment:

- PCAP02.pcapng

<br />
### Solution

Wiresharkで `tcp.port==22` でフィルターした後、Lengthでソートして見てみると、他とは違う逆方向の通信（source port 46682）が見つかります（以下）。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2023_Creepy_Crawling.png" alt="deadface_CTF_2023_Creepy_Crawling.png">

<br><br>

あとは、Follow TCP streamしてSSHサーバーのバナーを見るだけです。

<br>

Flag: `flag{SSH-2.0-9.29 FlowSsh: Bitvise SSH Server (WinSSHD) 9.29}`



<br /><br />
<br /><br />
## [Traffic Analysis]: Keys to the Kingdom (100 points)
- - -
### Challenge
> An image was sent across De Monne Financial’s network. They’re still paranoid after being the primary target of DEADFACE last year, and they suspect this is DEADFACE activity. We’re not sure what to make of the PCAP they sent us, but we found a file on the anomalous De Monne machine named keytothekingdom.txt. We suspect that contents of the file have undergone some form of encoding or encryption. See if you can piece these two things together to find the flag in the pcap.
<br /><br />
Submit the answer as flag{here-is-the-answer}.
<br /><br />
Key ciphertext: ÓÄÖëÄøõàñããàøâñãõùãªÞåýåøåûåýñûùñûùñùñüåþñýÿâí

Attachment:

- Thekeytothekingdom.pcap

<br />
### Solution

データを含んでいる TCP SYN パケット だけが含まれている pcapです。

1パケット目を見たら、`JFIF`というマジックナンバーが出てくるので、画像データが含まれていることがわかります。

以下で画像ファイルが取り出せます。

<pre>
$ tshark -r Thekeytothekingdom.pcap -T fields -e tcp.payload | tr -d "\n" | xxd -r -p > aaa.jpg
</pre>

<br>
チャレンジ文に出てくる Key ciphertext の方は、CyberChef で XOR Brute Forceすると、以下の文字列が見つかります。
<pre>
Key = d0: CTF{ThepassphraseiszNumuhukumakiakiaialunamor}
</pre>

<br>
`Numuhukumakiakiaialunamor` を pass phrase として steghide を使ったらフラグが得られます。

<pre>
$ steghide extract -p 'Numuhukumakiakiaialunamor' -sf aaa.jpg
wrote extracted data to "flag.txt".
</pre>

<br>

Flag: `flag{Error404FlagNotFound}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
