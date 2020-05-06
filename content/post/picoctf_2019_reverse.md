---
title: "picoCTF 2019 Writeup (Reverse Engineering)"
date: 2019-10-13T12:00:00+09:00
lastmod: 2019-10-13T12:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2019_reverse%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2019game.picoctf.com/](https://2019game.picoctf.com/)
<br /><br />
2週間、お疲れ様です。

<br /><br />
最終結果は、こんな感じ。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Score.png" alt="pico2019_Score.png">

<br />
イベント終了時点で、ユーザは4万人弱でした。

<img src="https://captureamerica.github.io/writeups/img/pico2019_Users.png" alt="pico2019_Users.png">

<img src="https://captureamerica.github.io/writeups/img/pico2019_Rank.png" alt="pico2019_Rank.png">

PwnとWeb系があんまりできてないけど、スコアも切りのいい20000に行ったし、Globalで目標の300位以内 (283位) にも入れたのでかなり満足です。

去年出た問題に似てるものも結構あったので、picoCTF 2018をそれなりにやった人は結構アドバンテージがあったと思います。



<br /><br />
Reverse Engineering の Writeupです。




<br /><br />
## [Reverse Engineering]: vault-door-1 (100 points)
- - -
### Challenge
> This vault uses some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: VaultDoor1.java
<br /><br />
Hints : Look up the charAt() method online.

Attachment:

- VaultDoor1.java

<br />
### Solution
<pre>
cat temp.txt | cut -d'(' -f2 | sort -n | cut -d"'" -f2 | tr -d "\n" ; echo
d35cr4mbl3_tH3_cH4r4cT3r5_82e029
</pre>


Flag: `picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_82e029}`



<br /><br />
<br /><br />
## [Reverse Engineering]: vault-door-3 (200 points)
- - -
### Challenge
> This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java
<br /><br />
Hints : Make a table that contains each value of the loop variables and the corresponding buffer index that it writes to.


Attachment:

- VaultDoor3.java

<br />
### Solution
```C
#include <stdio.h>

char cipher[] = "jU5t_a_sna_3lpm13gf49_u_4_mar24c";
char buff[32];
	
int main( void )
{
	int i;

	for ( i = 0 ; i < 32 ; i++ ) {
		buff[i] = 'F';
	}
		
	for ( i = 0 ; i < 8 ; i++ ) {
		buff[i] = cipher[i];
	}

	for ( i = 8 ; i < 16 ; i++ ) {
		buff[i] = cipher[23 - i];
	}

	for ( i = 16 ; i < 32 ; i+=2 ) {
		buff[i] = cipher[46 - i];
	}

        for ( i = 31 ; i >= 17 ; i -=2 ) {
		buff[i] = cipher[i];
        }
	printf( "%s\n", buff );

	return 0;
}
```


Flag: `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_9af23c}`




<br /><br />
<br /><br />
## [Reverse Engineering]: vault-door-4 (250 points)
- - -
### Challenge
> This vault uses ASCII encoding for the password. The source code for this vault is here: VaultDoor4.java
<br /><br />
Hints :<br />
Use a search engine to find an "ASCII table".<br />
You will also need to know the difference between octal, decimal, and hexademical numbers.


Attachment:

- VaultDoor4.java

<br />
### Solution
<pre>
$ python -c 'print("".join([chr(int(x)) for x in "106  85   53   116  95   52   95   98".split()]))'
jU5t_4_b

$ python -c 'print("".join([chr(int(x,16)) for x in "0x55 0x6e 0x43 0x68 0x5f 0x30 0x66 0x5f".split()]))'
UnCh_0f_

$ python -c 'print("".join([chr(int(x,8)) for x in "0142 0131 0164 063  0163 0137 065  070".split()]))'
bYt3s_58

$ python -c 'print("".join([x for x in "'8'  '4'  '0'  '6'  '3'  '5'  'a'  '1'".split()]))'
840635a1

picoCTF{jU5t_4_bUnCh_0f_bYt3s_58840635a1}

</pre>

<br>

Flag: `picoCTF{jU5t_4_bUnCh_0f_bYt3s_58840635a1}`






<br /><br />
<br /><br />
## [Reverse Engineering]: vault-door-6 (350 points)
- - -
### Challenge
> This vault uses an XOR encryption scheme. The source code for this vault is here: VaultDoor6.java
<br /><br />
Hints : If X ^ Y = Z, then Z ^ Y = X. Write a program that decrypts the flag based on this fact.


Attachment:

- VaultDoor6.java

<br />
### Solution

<pre>
$ python -c 'print("".join([chr(int(x,16)^0x55) for x in "0x3b 0x65 0x21 0xa  0x38 0x0  0x36 0x1d 0xa  0x3d 0x61 0x27 0x11 0x66 0x27 0xa 0x21 0x1d 0x61 0x3b 0xa  0x2d 0x65 0x27 0xa 0x64 0x64 0x65 0x62 0x61 0x65 0x33".split()]))'
n0t_mUcH_h4rD3r_tH4n_x0r_110740f
</pre>

<br>

Flag: `picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_110740f}`





<br /><br />
<br /><br />
## [Reverse Engineering]: Need For Speed (400 points)
- - -
### Challenge
> The name of the game is speed. Are you quick enough to solve this problem and keep it above 50 mph? need-for-speed.
<br /><br />
Hints : What is the final key?


Attachment:

- need-for-speed

<br />
### Solution
gdbでalarmの引き数を変えて実行。

<pre>
gef➤  x ($rbp-0xc)
0x7fffffffe204:	0x00000001
gef➤  set {int}($rbp-0xc)=0xFFFF
gef➤  x ($rbp-0xc)
0x7fffffffe204:	0x0000ffff
gef➤  c
Continuing.
Creating key...
Finished
Printing flag:
PICOCTF{Good job keeping bus #2cd93bf3 speeding along!}
</pre>


Flag: `PICOCTF{Good job keeping bus #2cd93bf3 speeding along!}`




<br /><br />
<br /><br />
## [Reverse Engineering]: Time's Up (400 points)
- - -
### Challenge
> Time waits for no one. Can you solve this before time runs out?
<br /><br />
Hints : Can you interact with the program using a script?


Attachment:

- times-up

<br />
### Solution
確か、こんな風だったはず。recvuntil()してたらタイムアウトしちゃってたので、コメントアウトしました。
```Python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from pwn import *

elf = ELF('/problems/time-s-up_4_548d4bc5ce82bf27864a00001fcbd182/times-up')
p = elf.process()
p.recvuntil("Challenge: ")
chall = p.recv().strip().split("\n")[0]
print chall
#print p.recvuntil("Solution? ")
p.sendline(str(eval(chall)))
print p.recvall()

```


Flag: `picoCTF{Gotta go fast. Gotta go FAST. #046cc375}`





<br /><br />
<br /><br />
## [Reverse Engineering]: asm4 (400 points)
- - -
### Challenge
> What will asm4("picoCTF_376ee") return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format.
<br /><br />
Hints : Treat the Array argument as a pointer


Attachment:

- test.S

以下、中身。
```asm
asm4:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	push   ebx
	<+4>:	sub    esp,0x10
	<+7>:	mov    DWORD PTR [ebp-0x10],0x25c
	<+14>:	mov    DWORD PTR [ebp-0xc],0x0
	<+21>:	jmp    0x518 <asm4+27>
	<+23>:	add    DWORD PTR [ebp-0xc],0x1
	<+27>:	mov    edx,DWORD PTR [ebp-0xc]
	<+30>:	mov    eax,DWORD PTR [ebp+0x8]
	<+33>:	add    eax,edx
	<+35>:	movzx  eax,BYTE PTR [eax]
	<+38>:	test   al,al
	<+40>:	jne    0x514 <asm4+23>
	<+42>:	mov    DWORD PTR [ebp-0x8],0x1
	<+49>:	jmp    0x587 <asm4+138>
	<+51>:	mov    edx,DWORD PTR [ebp-0x8]
	<+54>:	mov    eax,DWORD PTR [ebp+0x8]
	<+57>:	add    eax,edx
	<+59>:	movzx  eax,BYTE PTR [eax]
	<+62>:	movsx  edx,al
	<+65>:	mov    eax,DWORD PTR [ebp-0x8]
	<+68>:	lea    ecx,[eax-0x1]
	<+71>:	mov    eax,DWORD PTR [ebp+0x8]
	<+74>:	add    eax,ecx
	<+76>:	movzx  eax,BYTE PTR [eax]
	<+79>:	movsx  eax,al
	<+82>:	sub    edx,eax
	<+84>:	mov    eax,edx
	<+86>:	mov    edx,eax
	<+88>:	mov    eax,DWORD PTR [ebp-0x10]
	<+91>:	lea    ebx,[edx+eax*1]
	<+94>:	mov    eax,DWORD PTR [ebp-0x8]
	<+97>:	lea    edx,[eax+0x1]
	<+100>:	mov    eax,DWORD PTR [ebp+0x8]
	<+103>:	add    eax,edx
	<+105>:	movzx  eax,BYTE PTR [eax]
	<+108>:	movsx  edx,al
	<+111>:	mov    ecx,DWORD PTR [ebp-0x8]
	<+114>:	mov    eax,DWORD PTR [ebp+0x8]
	<+117>:	add    eax,ecx
	<+119>:	movzx  eax,BYTE PTR [eax]
	<+122>:	movsx  eax,al
	<+125>:	sub    edx,eax
	<+127>:	mov    eax,edx
	<+129>:	add    eax,ebx
	<+131>:	mov    DWORD PTR [ebp-0x10],eax
	<+134>:	add    DWORD PTR [ebp-0x8],0x1
	<+138>:	mov    eax,DWORD PTR [ebp-0xc]
	<+141>:	sub    eax,0x1
	<+144>:	cmp    DWORD PTR [ebp-0x8],eax
	<+147>:	jl     0x530 <asm4+51>
	<+149>:	mov    eax,DWORD PTR [ebp-0x10]
	<+152>:	add    esp,0x10
	<+155>:	pop    ebx
	<+156>:	pop    ebp
	<+157>:	ret    
```

<br />
### Solution (Unsolved??)
どのように解こうか試行錯誤したんですが、アセンブラからCコードを書き起こすのが一番いいかな、と思ってそうしました。

```C
#include <stdio.h>

int asm4( char *letter )
{
	int j = 0x25c; // [ebp-0x10]
	int i = 0;     // [ebp-0xc]
	int k;         // [ebp-0x8]
	int c = 0;     // [ebp-0x4] dummy

	while( 1 ) {
		if ( *( letter + i ) == '\0' )
			break;
		i++;
	}
	k = 1;
	goto label;
	
	while( 1 ) {
		if ( k < i ) {
			j = ( *( letter + k ) - *( letter + k - 1 ) ) + (j * 1);
			j = *( letter + k + 1 ) - *( letter + k ) + j;
			k++;
label:
			i = i - 1;
		} else {
			return j;
		}
	}
}

int main( void )
{
	printf( "0x%x\n", asm4("picoCTF_376ee") );
	return 0;
}
```

さらに、これをコンパイルしてアセンブラコードを確認したところ、かなり酷似している所まで来ていることはわかりました。
```asm
$ objdump -D -M intel main_asm4.o | grep asm4 -A 100

0000051d <asm4>:
 51d:	55                   	push   ebp
 51e:	89 e5                	mov    ebp,esp
 520:	83 ec 10             	sub    esp,0x10
 523:	e8 e9 00 00 00       	call   611 <__x86.get_pc_thunk.ax>
 528:	05 b0 1a 00 00       	add    eax,0x1ab0
 52d:	c7 45 f0 5c 02 00 00 	mov    DWORD PTR [ebp-0x10],0x25c
 534:	c7 45 f4 00 00 00 00 	mov    DWORD PTR [ebp-0xc],0x0
 53b:	c7 45 fc 00 00 00 00 	mov    DWORD PTR [ebp-0x4],0x0
 542:	90                   	nop
 543:	8b 55 f4             	mov    edx,DWORD PTR [ebp-0xc]
 546:	8b 45 08             	mov    eax,DWORD PTR [ebp+0x8]
 549:	01 d0                	add    eax,edx
 54b:	0f b6 00             	movzx  eax,BYTE PTR [eax]
 54e:	84 c0                	test   al,al
 550:	74 06                	je     558 <asm4+0x3b>
 552:	83 45 f4 01          	add    DWORD PTR [ebp-0xc],0x1
 556:	eb eb                	jmp    543 <asm4+0x26>
 558:	90                   	nop
 559:	c7 45 f8 01 00 00 00 	mov    DWORD PTR [ebp-0x8],0x1
 560:	eb 53                	jmp    5b5 <asm4+0x98>
 562:	8b 55 f8             	mov    edx,DWORD PTR [ebp-0x8]
 565:	8b 45 08             	mov    eax,DWORD PTR [ebp+0x8]
 568:	01 d0                	add    eax,edx
 56a:	0f b6 00             	movzx  eax,BYTE PTR [eax]
 56d:	0f be d0             	movsx  edx,al
 570:	8b 45 f8             	mov    eax,DWORD PTR [ebp-0x8]
 573:	8d 48 ff             	lea    ecx,[eax-0x1]
 576:	8b 45 08             	mov    eax,DWORD PTR [ebp+0x8]
 579:	01 c8                	add    eax,ecx
 57b:	0f b6 00             	movzx  eax,BYTE PTR [eax]
 57e:	0f be c0             	movsx  eax,al
 581:	29 c2                	sub    edx,eax
 583:	8b 45 f0             	mov    eax,DWORD PTR [ebp-0x10]
 586:	01 c2                	add    edx,eax
 588:	8b 45 f8             	mov    eax,DWORD PTR [ebp-0x8]
 58b:	8d 48 01             	lea    ecx,[eax+0x1]
 58e:	8b 45 08             	mov    eax,DWORD PTR [ebp+0x8]
 591:	01 c8                	add    eax,ecx
 593:	0f b6 00             	movzx  eax,BYTE PTR [eax]
 596:	0f be c0             	movsx  eax,al
 599:	8d 0c 02             	lea    ecx,[edx+eax*1]
 59c:	8b 55 f8             	mov    edx,DWORD PTR [ebp-0x8]
 59f:	8b 45 08             	mov    eax,DWORD PTR [ebp+0x8]
 5a2:	01 d0                	add    eax,edx
 5a4:	0f b6 00             	movzx  eax,BYTE PTR [eax]
 5a7:	0f be c0             	movsx  eax,al
 5aa:	29 c1                	sub    ecx,eax
 5ac:	89 c8                	mov    eax,ecx
 5ae:	89 45 f0             	mov    DWORD PTR [ebp-0x10],eax
 5b1:	83 45 f8 01          	add    DWORD PTR [ebp-0x8],0x1
 5b5:	83 6d f4 01          	sub    DWORD PTR [ebp-0xc],0x1
 5b9:	8b 45 f8             	mov    eax,DWORD PTR [ebp-0x8]
 5bc:	3b 45 f4             	cmp    eax,DWORD PTR [ebp-0xc]
 5bf:	7c a1                	jl     562 <asm4+0x45>
 5c1:	8b 45 f0             	mov    eax,DWORD PTR [ebp-0x10]
 5c4:	c9                   	leave  
 5c5:	c3                   	ret
```

<br />
ただし、このコードで取れる戻り値は 0x248 で正解ではなかったです。。。（泣）


やっている事はだいたいわかったし、処理の内容からそんなに大きく値がズレることはないだろう、というのは何となくわかったので、時間があるときにテキトーに値を変えてsubmitしてたら 0x24d で通りました。

うーん、これも、イベント中に問題が書き換わっていたのかな。相当悩まされました。。。

Flag: `picoCTF{0x24d}`





<br /><br />
<br /><br />
## [Reverse Engineering]: droids3 (450 points)
- - -
### Challenge
> Find the pass, get the flag. Check out this file.
<br /><br />
Hints : <br />
Try using apktool and an emulator<br />
https://ibotpeaches.github.io/Apktool/<br />
https://developer.android.com/studio<br />


Attachment:

- three.apk

<br />
### Solution
1. $ java -jar /usr/local/bin/apktool.jar d three.apk
2. three_apk/three/smali/com/hellocmu/picoctf/FlagstaffHill.smali の編集
3. $ java -jar /usr/local/bin/apktool.jar b ./three -o three_modified.apk
4. $ keytool -genkeypair -alias androiddebugkey -keypass android -keyalg RSA -keysize 2048 -sigalg SHA256withRSA -validity 10950 -dname "CN=Android Debug,O=Android,C=US" -keystore debug.keystore -storepass android
5. $ jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA1 -tsa http://timestamp.digicert.com -keystore debug.keystore -storepass android three_modified.apk androiddebugkey
6. Android Studioで実行

Flag: `picoCTF{tis.but.a.scratch}`




<br /><br />
<br /><br />
## [Reverse Engineering]: vault-door-8 (450 points)
- - -
### Challenge
> Apparently Dr. Evil's minions knew that our agency was making copies of their source code, because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! The result is a quite mess, but I trust that my best special agent will find a way to solve it. The source code for this vault is here: VaultDoor8.java
<br /><br />
Hints : <br />
Clean up the source code so that you can read it and understand what is going on.<br />
Draw a diagram to illustrate which bits are being switched in the scramble() method, then figure out a sequence of bit switches to undo it. You should be able to reuse the switchBits() method as is.



Attachment:

- VaultDoor8.java

<br />
### Solution
javaのソースを変えて、そのままデバッグしたらいいのかも知れないんですけどね。よくわからないからCでやりました。

```C
#include <stdio.h>
#include <string.h>

char cipher[] = {0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xC1, 0xF1, 0xF0, 0xE0, 0xF1, 0x94, 0xB4, 0xF0, 0x85};
char a[32];

char switchBits(char c, int p1, int p2);

char *scramble( char *password ) {
	int b;
	char c;
	a[0] = '\0';
	memcpy( a, password, 32 );
	
        for ( b = 0; b < 32 ; b++ ) {
		c = a[b];

		printf("b = %d\n", b);
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 7, 6);
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 5, 2);
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 4, 3);
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 1, 0); /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 7, 4);
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 6, 5);
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 3, 0); /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */
		printf("0x%02x ", (int)c&0xff);
		c = switchBits(c, 2, 1);
		printf("0x%02x ", (int)c&0xff);
		puts("");

		a[b] = c;
        }
	puts("");
        return a;
}

char switchBits(char c, int p1, int p2) {
	int mask1 = 0;
	int mask2 = 0;
	int bit1;
	int bit2;
	int rest, result;
	int shift;
	
	mask1 = (1 << p1);
	mask2 = (1 << p2); /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */
	bit1 = (c & mask1);
	bit2 = (c & mask2);
	rest = (c & ~(mask1 | mask2));
	shift = (p2 - p1);
	if (shift < 0 ) {
		shift = shift * (-1);
	}
	result = ((bit2 << shift) | (bit1 >> shift) | rest);
	result = result & 0xFF;

        return result;
}

int main( void )
{
	printf( "%s\n", scramble( cipher ) );
	return 0;
}
```

Flag: `picoCTF{s0m3_m0r3_b1t_sh1fTiNg_47327ac3d}`




<br /><br />
<br /><br />
## [Reverse Engineering]: droids4 (500 points)
- - -
### Challenge
> reverse the pass, patch the file, get the flag. Check out this file.
<br /><br />


Attachment:

- four.apk

<br />
### Solution
基本的にやることは前述の droids3 と同じです。

smaliコードの書き換え方は以下の通り。

<pre>
(before)
    if-eqz v5, :cond_0

    const-string v5, "call it"

    return-object v5

(after)
    if-eqz v5, :cond_0
    invoke-static {p0}, Lcom/hellocmu/picoctf/FlagstaffHill;->cardamom(Ljava/lang/String;)Ljava/lang/String;
    move-result-object v5

    return-object v5
</pre>

<br>

Flag: `picoCTF{not.particularly.silly}`




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

