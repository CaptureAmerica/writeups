---
title: "hack.lu CTF 2019 Writeup | フラxxグゲット"
date: 2019-10-26T05:00:00+09:00
lastmod: 2019-10-26T05:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
URL: [https://fluxfingersforfuture.fluxfingers.net/challenges/](https://fluxfingersforfuture.fluxfingers.net/challenges/)
<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/hack.lu_ctf_2019_chall_list.png" alt="hack.lu_ctf_2019_chall_list.png">
<br /><br />
平日だったこともあり、17番のmisc問題だけトライしました。

実は、解けてないですが、なんとなく方向性はあってる気がするのでWriteup残しておきます。


<br /><br />
## [Misc]: Futuristic Communication
- - -
### Challenge
> Communication in the future is a big thing. With the rising amount of data in the internet new handshake techniques are required. Here is my implementation, where I can identify who did request which resources.
<br /><br />
<b>Remark:</b> The remote host in the PCAPs is not the host you have to connect to.
<br /><br />
<b>Note:</b> Because we use network protocols in a very futuristic way, you should check your network setup like firewall or router if you have problems solving this challenge. Maybe it's easier for you to use a cloud provider.
<br /><br />
Announcements for Futuristic Communication<br />
(Published on 2019-10-23 02:08:22)<br />
Because we use network protocols in a very futuristic way, you should check your network setup like firewall or router if you have problems solving this challenge. Maybe it's easier for you to use a cloud provider.
<br /><br />
(Published on 2019-10-22 16:56:08)<br />
Due to high load the server may sometimes have difficulty keeping up with processing identifiers which may cause the package to be dropped. Please insert a delay of about 1-2 seconds before sending a package containing an identifier to ensure those packages will be processed.


Attachments:

- futuristic_communication.pcapng

<br />
### Unsolved
まずは、Wiresharkでpcapを開いて、Follow TCP Streamします。

<pre>
Welcome to handshake in future!
Your personal identifier is: 051733e3-ec47-4518-9ff9-9ec90af9b27b
Waiting for identification, on TCP port 13792
Identification successful. Waiting for further instructions.
Expecting handshake, on TCP port 38462, with sequence number 2339380618
Connection setup noticed. Your Commands:
synchronize
privileged
flag

All your TCP flags should be SYN flags.
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
synchronize... success
Check if the connection is synchronized.
Connection synchronized. Entering privileged mode.
flag{only-to-show-now-comes-flag}
</pre>


"tcp.port==13792" でフィルターすると、受け取った personal identifier を SYNパケットにくっつけて送っているのが見えます。

<img src="https://captureamerica.github.io/writeups/img/hack.lu_ctf_2019_tcp_13792.png" alt="hack.lu_ctf_2019_tcp_13792.png">


<br />
"tcp.port==38462" でフィルターすると、受け取った sequence number をSYNパケットにくっつけて送っているのが見えます。あと、personal identifier も付いてます。

<img src="https://captureamerica.github.io/writeups/img/hack.lu_ctf_2019_tcp_38462.png" alt="hack.lu_ctf_2019_tcp_38462.png">

これを見る限り、SYNを送った後にまずRSTが返り、その後SYN/ACKが返ってくるように見えます。



<br /><br />
とりあえず、ここまでの処理をScapyを使って書きました。

```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from pwn import *
from scapy.all import *

host, port = '31.22.123.60', 1337
s = remote(host, port)
msg = s.recvline()
msg = s.recvline()
id = re.findall(r"Your personal identifier is: (.*)\n", msg)[0]
# print id

msg = s.recvline()
port = int(re.findall(r"Waiting for identification, on TCP port (.*)\n", msg)[0])
# print port

srcport = 37614
pkt=IP(dst="31.22.123.60")/TCP(dport=port, sport=srcport)/id
send(pkt)

msg = s.recvuntil("with sequence number")
port = int(re.findall(r"Expecting handshake, on TCP port (.*), ", msg)[0])
print port

msg = s.recvuntil("\n")
seqnum  = msg.strip()
print seqnum
pkt=IP(dst="31.22.123.60")/TCP(flags='S',dport=port,sport=srcport,seq=int(seqnum))
sleep(2)
ans=sr(pkt)
sleep(1)
## This fails with "Invalid packet. Exiting" error...

```

<br />
残念ながら、sequence numberを指定した SYNパケットがInvalidのようです。

以下、Follow TCP Streamです。
<pre>
Welcome to handshake in future!
Your personal identifier is: ae08b500-d644-47fa-9c7e-23acbd82d00f
Waiting for identification, on TCP port 20428
Identification successful. Waiting for further instructions.
Expecting handshake, on TCP port 54538, with sequence number 728714096
Invalid packet. Exiting     <==これ
</pre>

<br />
"tcp.port==54538" でフィルターしたものです。sequence number もちゃんと設定できているし、合っていると思うんですけどね。どこら辺がInvalidなのかわかんないです。

<img src="https://captureamerica.github.io/writeups/img/hack.lu_ctf_2019_tcp_54538.png" alt="hack.lu_ctf_2019_tcp_54538.png">


<br />
Handshakeがうまく行けば、あとは、synchronize、privileged、flagコマンドを送るだけなんだと思います。
```Python
pkt=IP(dst="31.22.123.60")/TCP(dport=port, sport=srcport)/"synchronize"
pkt=IP(dst="31.22.123.60")/TCP(dport=port, sport=srcport)/"privileged"
pkt=IP(dst="31.22.123.60")/TCP(dport=port, sport=srcport)/"flag"
```

<br />
なお、SYN/ACKを受信するにあたって、iptablesの設定も要ります。今回は、SYNがInvalid packet扱いされているので、そこまで行ってないですけど。

Mac OSだと、pfctlでやるみたいです。




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
