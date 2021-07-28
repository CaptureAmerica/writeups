---
title: "CyBRICS CTF 2021 Writeup"
date: 2021-07-28T21:00:00+09:00
lastmod: 2021-07-28T21:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fcybrics_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://cybrics.net/](https://cybrics.net/)
<br /><br />
[CyBRICS CTF 2019](https://captureamerica.github.io/writeups/post/cybrics_ctf_2019/)では、1問しか解けず。

[CyBRICS CTF 2020](https://captureamerica.github.io/writeups/post/cybrics_ctf_2020/)では、なんとか4問解いて275位。

今年はまた1問だけです。。。 50点で459位でーす。

<br>

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2021_Score1.png" alt="cybrics_CTF_2021_Score1.png">

<br><br>
チャレンジのリスト。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2021_Score2.png" alt="cybrics_CTF_2021_Score2.png">

<br>

解けなかったけど、途中までやった Network Challenge ("localhost") をメモも兼ねて残しておきます。

<br /><br />
## [rebyC, Baby]: Scanner (50 points)
- - -
### Challenge
> Check out this cool new game!
<br /><br />
I heard they serve flags at level 5.


<br />
### Solution
アニメーションGIFで、画像をあたかもスキャンしているように、画像が部分的に流れていきます。

Level1 ~ Level4は目を凝視して解きました。

<br />

しかし、Level5は明らかにQRコードだったので、元の画像を取り出さないといけません。

Pythonとかを使ってスマートに解くのが想定解なのかも知れないですが、力技でやりました。

<br>

MacのPreview AppでGIFファイルを開き、範囲指定を使ってスクリーンショットを撮って、Libre OfficeのSheetにペタペタ貼り付けてできたのがこちら。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2021_scanner.png" alt="cybrics_CTF_2021_scanner.png">


<br><br>

しかし、これだけでは正しく読み取れず、ImageMagickを使ってなるべく正方形になるようにしました。

<pre>
$ convert aaa.png -resize 256x290\! QR.png
</pre>

{{% admonition tip "Tips" %}}
\\!をつけるとratioを無視してリサイズできます。
{{% /admonition %}}


<br><br>
こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/cybrics_CTF_2021_QR.png" alt="cybrics_CTF_2021_QR.png">


<br><br>
iPhone cameraだと読み取れなかったけど、Android phone の QR & Barcode scannerアプリでなんとか読めました。


<br />
Flag: `cybrics{N0w_Y0u_4r3_4_c4sh13r_LOL}`



<br /><br />
<br /><br />
## [Network, Hard]: localhost (About 300 points)
- - -
### Challenge
> Remember NET fleeks? I’ve pwned a box in another corporate network, and there is some peculiarly configured server near my foothold. Take a look.

<br />
### (Unsolved)
OSCPを取って依頼、久々にPentestっぽいことをしました。

<br />
`NET fleeks` って何かな？と思ってググったら、CyBRICS CTF 2020で出てきたチャレンジでした。完全スルーしたやつです。

去年のWriteupを見ると、scapyやらをインストールしたりして解いていたようです。

<br>
問題がlocalhostだったので、とりあえずlocalでlistenしているサービスを見てみました。

<pre>
root@PWNED:~# netstat -altpn
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.11:37021        0.0.0.0:*               LISTEN      -


root@PWNED:~# ifconfig -a
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.193.21.7  netmask 255.255.255.0  broadcast 10.193.21.255
        ether 02:42:0a:c1:15:07  txqueuelen 0  (Ethernet)
        RX packets 11  bytes 1226 (1.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@PWNED:~# tcpdump -i lo port 37021
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
^C
0 packets captured
0 packets received by filter
0 packets dropped by kernel
</pre>

<br>

特に何も受信はしていませんでした。

<br>

そもそも問題文の中で、`peculiarly configured server near my foothold` と言っているので、他のマシンがあるのは明らかなのでnmapをかけたところ、10.193.21.180が Up しているのが見つかりました。

（10.193.21.1もUPしてますが、単純にGatewayっぽかったので無視）

<pre>
root@PWNED:~# nmap -v -sn 10.193.21.1-254
Starting Nmap 7.70 ( https://nmap.org ) at 2021-07-25 03:55 UTC
Initiating ARP Ping Scan at 03:55
Scanning 253 hosts [1 port/host]
Completed ARP Ping Scan at 03:55, 5.36s elapsed (253 total hosts)
Initiating Parallel DNS resolution of 253 hosts. at 03:55
Completed Parallel DNS resolution of 253 hosts. at 03:55, 0.04s elapsed
Nmap scan report for 10.193.21.1
Host is up (0.000049s latency).
MAC Address: 02:42:7F:4D:62:52 (Unknown)
Nmap scan report for 10.193.21.2 [host down]
:
Nmap scan report for lh_tgt_277.lh_277 (10.193.21.180)
Host is up (0.000062s latency).
MAC Address: 02:42:0A:C1:15:B4 (Unknown)
:
</pre>

<br>

サービスは、TCPだと80番のみオープン。

<pre>
root@PWNED:~# ports=$(nmap -p- --min-rate=1000 -T4 10.193.21.180 | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
root@PWNED:~# nmap -p$ports -sC -sV 10.193.21.180
Starting Nmap 7.70 ( https://nmap.org ) at 2021-07-25 03:59 UTC
Nmap scan report for lh_tgt_277.lh_277 (10.193.21.180)
Host is up (0.00012s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.10.3
|_http-server-header: nginx/1.10.3
|_http-title: Site doesn't have a title (text/html).
MAC Address: 02:42:0A:C1:15:B4 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.97 seconds
</pre>

<br>
アクセスしてみると、confファイルへのリンクが得られます。

```C
root@PWNED:~# curl -v 10.193.21.180
* Expire in 0 ms for 6 (transfer 0x555769cd41a0)
*   Trying 10.193.21.180...
* TCP_NODELAY set
* Expire in 200 ms for 4 (transfer 0x555769cd41a0)
* Connected to 10.193.21.180 (10.193.21.180) port 80 (#0)
> GET / HTTP/1.1
> Host: 10.193.21.180
> User-Agent: curl/7.64.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx/1.10.3
< Date: Sun, 25 Jul 2021 03:59:45 GMT
< Content-Type: text/html
< Content-Length: 388
< Last-Modified: Thu, 22 Jul 2021 22:38:14 GMT
< Connection: keep-alive
< ETag: "60f9f356-184"
< Accept-Ranges: bytes
<
<h1>Storage Node Status Page</h1>
<table border=1>
    <tr><td>Records</td><td>1</td></tr>
    <tr><td>Flag-containing Records</td><td>1</td></tr>
    <tr><td>Redis Configuration</td><td>Default: <a href="redis.conf">redis.conf</a></td></tr>
    <tr><td>System Configuration</td><td><a href="sysctl.conf">sysctl.conf</a></td></tr>
    <tr><td>Security</td><td>perfect!</td></tr>
</table>
* Connection #0 to host 10.193.21.180 left intact
```

<br>
sysctl.confの中では、以下が有効になっていました。

<pre>
net.ipv4.conf.all.route_localnet=1
</pre>


<br>
これをググってみると、セキュリティ問題があり、かつ localhost に関係していることがわかります。ビンゴっぽいです。

参考1：<br>
[https://github.com/kubernetes/kubernetes/issues/90259](https://github.com/kubernetes/kubernetes/issues/90259)

参考2：<br>
[https://unit42.paloaltonetworks.jp/cve-2020-855/](https://unit42.paloaltonetworks.jp/cve-2020-855/)


<br>
宛先のMac addressを指定して、127.0.0.1宛てにパケットを送ると通信ができるっぽい？ので、scapyをインストールして以下のスクリプトを実行してみました。

宛先のポートは、redis.confをベースに6379にしました。

```Python
#!/usr/bin/env python
from scapy.all import *
answer = sendp(Ether(dst="02:42:0a:c1:15:b4",src="02:42:0a:c1:15:07")/IP(dst="127.0.0.1",src="10.193.21.7")/TCP(dport=6379,sport=50000)/"PING", iface="eth0", verbose=1)
print(answer)
```

<br>
何も応答が返ってこないので、ここで降参。。。


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
