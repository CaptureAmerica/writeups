---
title: "InCTFi CTF 2021 Writeup"
date: 2021-08-14T14:00:00+09:00
lastmod: 2021-08-14T14:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Finctfi_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.bi0s.in/challenges](https://ctf.bi0s.in/challenges)
<br /><br />
もうアクセスできなくなっちゃったので、チャレンジリストとか順位とかが見れないですが、

CTFTimeによると、110点で157位だったようです。

<br>



<br /><br />
## [Network Pentest]: Listen (about 700 points??)
- - -
### Challenge
> The quieter you become, the more you are able to listen.

Attachment:

- listen.ovpn

<br />

### Solution
OpenVPNで接続すると、172.30.0.14/28がもらえます。

同じネットワークでどのホストがUpしているか確認します。

<pre>
$ ipcalc 172.30.0.14/28
Address:   172.30.0.14          10101100.00011110.00000000.0000 1110
Netmask:   255.255.255.240 = 28 11111111.11111111.11111111.1111 0000
Wildcard:  0.0.0.15             00000000.00000000.00000000.0000 1111
=>
Network:   172.30.0.0/28        10101100.00011110.00000000.0000 0000
HostMin:   172.30.0.1           10101100.00011110.00000000.0000 0001
HostMax:   172.30.0.14          10101100.00011110.00000000.0000 1110
Broadcast: 172.30.0.15          10101100.00011110.00000000.0000 1111
Hosts/Net: 14                    Class B, Private Internet

$ nmap -v -sn 172.30.0.1-13
Starting Nmap 7.80 ( https://nmap.org ) at 2021-08-14 09:32 +08
Initiating Ping Scan at 09:32
Scanning 13 hosts [2 ports/host]
Completed Ping Scan at 09:32, 1.50s elapsed (13 total hosts)
Initiating Parallel DNS resolution of 13 hosts. at 09:32
Completed Parallel DNS resolution of 13 hosts. at 09:32, 13.00s elapsed
Nmap scan report for 172.30.0.1 [host down]
Nmap scan report for 172.30.0.2 [host down]
Nmap scan report for 172.30.0.3 [host down]
Nmap scan report for 172.30.0.4 [host down]
Nmap scan report for 172.30.0.5 [host down]
Nmap scan report for 172.30.0.6 [host down]
Nmap scan report for 172.30.0.7 [host down]
Nmap scan report for 172.30.0.8
Host is up (0.0096s latency).
Nmap scan report for 172.30.0.9 [host down]
Nmap scan report for 172.30.0.10 [host down]
Nmap scan report for 172.30.0.11 [host down]
Nmap scan report for 172.30.0.12 [host down]
Nmap scan report for 172.30.0.13 [host down]
Read data files from: /usr/bin/../share/nmap
Nmap done: 13 IP addresses (1 host up) scanned in 14.51 seconds

</pre>



<br />

上記の結果より、172.30.0.8がいることがわかります。

172.30.0.8は、Full ポートスキャンしても開いているポートがありませんでした。

チャレンジ文より、172.30.0.8からトラフィックが自分宛てに生成されている予感がしたのでtcpdumpを取ってみたらその通りでした。

<pre>
$ sudo tcpdump -i tap0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on tap0, link-type EN10MB (Ethernet), capture size 262144 bytes
09:39:34.484628 ARP, Reply 172.30.0.8 is-at 02:42:c7:9d:29:f5 (oui Unknown), length 28
09:39:34.492835 IP 172.30.0.8.42174 > 172.30.0.14.31337: Flags [S], seq 3505602371, win 64240, options [mss 1325,sackOK,TS val 2909552799 ecr 0,nop,wscale 7], length 0
09:39:34.492858 IP 172.30.0.14.31337 > 172.30.0.8.42174: Flags [R.], seq 0, ack 3505602372, win 0, length 0
09:39:34.502521 IP 172.30.0.8.46584 > 172.30.0.14.31336: Flags [S], seq 3915094571, win 64240, options [mss 1325,sackOK,TS val 2909552809 ecr 0,nop,wscale 7], length 0

...(snip)...
</pre>

<br />

とりあえず、31337でListenしてみたところフラグが送られて来ていました。

<pre>
$ nc -l -p 31337

o, in aliquam ex. Maecenas id mauris nisl. Sed vitae lorem dictum, volutpat risus accumsan, posuere arcu. Aliquam fermentum, est eu auctor volutpat, mi augue dapibus nunc, in blandit urna erat eget eros. Suspendisse nec varius tortor. Sed nec velit elit.

...(snip)...

vel pulvinar elit blandit nec. inctf{s0_y0u_finally_d3cid3d_t0_listen!!} Lorem ipsum dolor sit amet

...(snip)...
</pre>

<br />

Flag: `inctf{s0_y0u_finally_d3cid3d_t0_listen!!}`



<br /><br />
## [Network Pentest]: Legacy (1000 points)
- - -
### Challenge
> There's no guarantee, it's not up to me, you can only see.

Attachment:

- buggy.ovpn

<br />
### (Unsolved)
OpenVPNで接続すると、172.30.0.14/28がもらえます。

172.30.0.5 のみがUpです。

以下のようにポートスキャンをすると、8080が開いているのがわかります。

<pre>
$ sudo nmap -p- -T4 -A -v 172.30.0.5 -Pn
Starting Nmap 7.80 ( https://nmap.org ) at 2021-08-14 10:53 +08
NSE: Loaded 151 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 10:53
Completed NSE at 10:53, 0.00s elapsed
Initiating NSE at 10:53
Completed NSE at 10:53, 0.00s elapsed
Initiating NSE at 10:53
Completed NSE at 10:53, 0.00s elapsed
Initiating ARP Ping Scan at 10:53
Scanning 172.30.0.5 [1 port]
Completed ARP Ping Scan at 10:53, 0.07s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 10:53
Completed Parallel DNS resolution of 1 host. at 10:53, 13.00s elapsed
Initiating SYN Stealth Scan at 10:53
Scanning 172.30.0.5 [65535 ports]
Discovered open port 8080/tcp on 172.30.0.5
Completed SYN Stealth Scan at 10:53, 10.57s elapsed (65535 total ports)
Initiating Service scan at 10:53
Scanning 1 service on 172.30.0.5
Completed Service scan at 10:53, 6.04s elapsed (1 service on 1 host)
Initiating OS detection (try #1) against 172.30.0.5
Retrying OS detection (try #2) against 172.30.0.5
Retrying OS detection (try #3) against 172.30.0.5
Retrying OS detection (try #4) against 172.30.0.5
Retrying OS detection (try #5) against 172.30.0.5
NSE: Script scanning 172.30.0.5.
Initiating NSE at 10:54
Completed NSE at 10:54, 0.35s elapsed
Initiating NSE at 10:54
Completed NSE at 10:54, 0.05s elapsed
Initiating NSE at 10:54
Completed NSE at 10:54, 0.00s elapsed
Nmap scan report for 172.30.0.5
Host is up (0.011s latency).
Not shown: 65534 closed ports
PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-methods:
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:42:A9:94:51:F0 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=8/14%OT=8080%CT=1%CU=42484%PV=Y%DS=1%DC=D%G=Y%M=0242A9
OS:%TM=61173051%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=103%TI=Z%CI=Z%II
OS:=I%TS=A)OPS(O1=M52DST11NW7%O2=M52DST11NW7%O3=M52DNNT11NW7%O4=M52DST11NW7
OS:%O5=M52DST11NW7%O6=M52DST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%
OS:W6=FE88)ECN(R=Y%DF=Y%T=40%W=FAF0%O=M52DNNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S
OS:=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%R
OS:D=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=
OS:0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U
OS:1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DF
OS:I=N%T=40%CD=S)

...(snip)...
</pre>

<br />

gobusterで /debug が見つかります。

<pre>
$ gobuster dir -u http://172.30.0.5:8080/ -w ~/common.txt -x php,txt
captureamerica@kali:~$ gobuster dir -u http://172.30.0.5:8080/ -w ~/common.txt -x php,txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://172.30.0.5:8080/
[+] Threads:        10
[+] Wordlist:       /home/captureamerica/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php,txt
[+] Timeout:        10s
===============================================================
2021/08/14 10:55:21 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htaccess.php (Status: 403)
/.hta (Status: 403)
/.hta.php (Status: 403)
/.hta.txt (Status: 403)
/.htaccess.txt (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.php (Status: 403)
/.htpasswd.txt (Status: 403)
/debug (Status: 301)
/index.html (Status: 200)
/server-status (Status: 403)
</pre>

<br/>

アクセスすると、Host Addressが入力できるフォームが出てきます。

コマンドインジェクションができそうな予感がしますが、Burp Suiteで見ていると、何をやっても Internal Server Error (500) が返ってくるのがわかります。

<br/>

ここで、/v3/debug/ を /v1/debug/ に変更すると、違った表示が出てきます。

<img src="https://captureamerica.github.io/writeups/img/inctfi_CTF_2021_Legacy1.png" alt="inctfi_CTF_2021_Legacy1.png">


<br />

Referer を変更してやればよさそうです。

<img src="https://captureamerica.github.io/writeups/img/inctfi_CTF_2021_Legacy2.png" alt="inctfi_CTF_2021_Legacy2.png">

<br />

コマンドインジェクションができるようになりました。

<img src="https://captureamerica.github.io/writeups/img/inctfi_CTF_2021_Legacy3.png" alt="inctfi_CTF_2021_Legacy3.png">

<br />


perlが使える環境だったので、perlを使ってreverse shellを確立しました。

<pre>
$ sudo nc -l -p 443
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

</pre>


<br />
比較的すんなりshellが取れたんですが、その後が全くわからなくて、flagっぽいのも見つからないし、rootも取れなくて降参しました。

誰かのwriteupを見て復習しようと思ったけど、見つからないです。。。



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
