---
title: "picoCTF 2019 Writeup (General Skills)"
date: 2019-10-13T12:00:00+09:00
lastmod: 2019-10-13T12:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---
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
General Skills の Writeupです。




<br /><br />
# [General Skills]: whats-the-difference (200 points)
- - -
## Challenge
> Can you spot the difference? kitters cattos.
<br /><br />
Hints :<br />
How do you find the difference between two files?<br />
Dumping the data from a hex editor may make it easier to compare.

Attachment:

- cattos.jpg<br />
- kitters.jpg


<br />
## Solution
ここら辺から、<br />
diff <(od -x cattos.jpg) <(od -x kitters.jpg)<br />
diff <(hexdump -c cattos.jpg) <(hexdump -c kitters.jpg)<br />

grep, trとか駆使してごちゃごちゃやって、いちおうフラグっぽいのが取れてたんですが、なかなかフラグとして通らないので、Cで書いてみました。


```C
#include <stdio.h>

int main( int argc, char **argv )
{
	int i = 0;
	char c1, c2;
	FILE *fp1, *fp2;
	
	if ( ( fp1 = fopen( "cattos.jpg", "r" ) ) == NULL ) {
		printf( "No such file.\n" );
		return -1;
	}
	if ( ( fp2 = fopen( "kitters.jpg", "r" ) ) == NULL ) {
		printf( "No such file.\n" );
		return -1;
	}
	
	while (1) {
		if ( ( c1 = fgetc( fp1 ) ) == EOF ) {
			//break;
		}
		if ( ( c2 = fgetc( fp2 ) ) == EOF ) {
			//break;
		}
		if ( c1 != c2 ) {
			printf( "%c", c1 );
			//printf( "%c", c2 );
		}
		i++;
		if  ( i > 3406694 ) {
			break;
		}
	}
	fclose( fp1 );
	fclose( fp2 );
	printf( "\ni = %d\n", i );
	return 0;
}
```

かつ、ローカルPC上で実行したときと、シェルサーバ上でやった結果も違うという。。。


Flag: `picoCTF{th3yr3_a5_d1ff3r3nt_4s_bu773r_4nd_j311y_aslkjfdsalkfslkflkjdsfdszmz10548}`




<br /><br />
<br /><br />
# [General Skills]: flag_shop (300 points)
- - -
## Challenge
> There's a flag shop selling stuff, can you buy a flag? Source. Connect with nc 2019shell1.picoctf.com 25858.
<br /><br />
Hints :<br />
Two's compliment can do some weird things when numbers get really big!


Attachment:

- store.c



<br />
## Solution
これも去年と同じで、整数のオーバーフローを使う問題です。

テキトーにやってたのでよくわからないけど、21475000あたりを使って解いたと思います。

Flag: `picoCTF{m0n3y_bag5_325fcd2e}`






<br /><br />
<br /><br />
# [General Skills]: mus1c (300 points)
- - -
## Challenge
> I wrote you a song. Put it in the picoCTF{} flag format.
<br /><br />
Hints : Do you think you can master rockstar?<br />



Attachment:

- lyrics.txt



<br />
## Solution
ヒントのおかげで解けました。

rockstarというesolangでした。

以下で解けます。
https://codewithrockstar.com/online

```
python -c 'print("".join([chr(int(x)) for x in "114 114 114 111 99 107 110 114 110 48 49 49 51 114".split()]))'
```

Flag: `picoCTF{rrrocknrn0113r}`





<br /><br />
<br /><br />
# [General Skills]: 1_wanna_b3_a_r0ck5tar (350 points)
- - -
## Challenge
> I wrote you another song. Put the flag in the picoCTF{} flag format

Attachment:

- lyrics.txt



<br />
## Solution
不要なIF文やListen (scanfみたいなの) をコメントアウトするだけです。

コメントアウトは、()でできます。

<pre>
Rocknroll is right
Silence is wrong
A guitar is a six-string
Tommy's been down
Music is a billboard-burning razzmatazz!
(Listen to the music)
(If the music is a guitar)
(Say "Keep on rocking!")
(Listen to the rhythm)
(If the rhythm without Music is nothing)
Tommy is rockin guitar
Shout Tommy!
Music is amazing sensation 
Jamming is awesome presence
Scream Music!
Scream Jamming!
Tommy is playing rock
Scream Tommy!
They are dazzled audiences
Shout it!
Rock is electric heaven
Scream it!
Tommy is jukebox god
Say it!
Break it down
Shout "Bring on the rock!"
(Else Whisper "That ain't it, Chief")
Break it down
</pre>


```
python -c 'print("".join([chr(int(x)) for x in "66 79 78 74 79 86 73".split()]))'
BONJOVI
```

Flag: `picoCTF{BONJOVI}`






<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

