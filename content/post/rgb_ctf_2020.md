---
title: "RGB CTF 2020 Writeup"
date: 2020-07-14T13:00:00+09:00
lastmod: 2020-07-14T13:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Frgb_ctf_2020%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.rgbsec.xyz/home](https://ctf.rgbsec.xyz/home)

<br />
最終順位は、334位でした。

<img src="https://captureamerica.github.io/writeups/img/rgb_CTF_2020_Score0.png" alt="rgb_CTF_2020_Score0.png">

<img src="https://captureamerica.github.io/writeups/img/rgb_CTF_2020_Score1.png" alt="rgb_CTF_2020_Score1.png">

こうやってみると、全然解けてないですね。。



<br /><br />
## [Beginner]: Pieces
- - -
### Challenge
> My flag has been divided into pieces :( Can you recover it for me?

Attachment:

- Main.java

以下が中身です。

{{< highlight java "linenos=table,hl_lines=6 16 17" >}}
import java.io.*;
public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		String input = in.readLine();
		if (divide(input).equals("9|2/9/:|4/7|8|4/2/1/2/9/")) {
			System.out.println("Congratulations! Flag: rgbCTF{" + input + "}");
		} else {
			System.out.println("Incorrect, try again.");
		}
	}
	
	public static String divide(String text) {
		String ans = "";
		for (int i = 0; i < text.length(); i++) {
			ans += (char)(text.charAt(i) / 2);
			ans += text.charAt(i) % 2 == 0 ? "|" : "/";
		}
		return ans;
	}
}
{{< / highlight >}}

<br />
### Solution
Javaのコードの中でやっていることは、
1. 文字を2で割ったものをansに追加。（16行目）
2. 文字を2で割り切れた場合はansに"|"を追加、割り切れなければansに"/"を追加。（17行目）
3. その結果が `9|2/9/:|4/7|8|4/2/1/2/9/` になる。（6行目）

<br>
ということで、

<pre>
$ python3
>>> chr((ord('9'))*2)
'r'
>>> chr((ord('2'))*2+1)
'e'
>>> chr((ord('9'))*2+1)
's'
>>> chr((ord(':'))*2)
't'
>>> chr((ord('4'))*2+1)
'i'
>>> chr((ord('7'))*2)
'n'
>>> chr((ord('8'))*2)
'p'
>>> chr((ord('4'))*2+1)
'i'
>>> chr((ord('2'))*2+1)
'e'
>>> chr((ord('1'))*2+1)
'c'
>>> chr((ord('2'))*2+1)
'e'
>>> chr((ord('9'))*2+1)
's'
</pre>

<br />

Flag: `rgbCTF{restinpieces}`




<br /><br />
<br /><br />
## [Misc]: Differences
- - -
### Challenge
> If a difference could difference differences from differences to calculate differences, would the difference differently difference differences to difference the difference from different differences differencing?
<br />

Attachment:

- DifferenceTest.java

<br />
### Solution
所々、文字化けしてます。

<img src="https://captureamerica.github.io/writeups/img/rgb_CTF_2020_differences_solved.png" alt="rgb_CTF_2020_differences_solved.png">

やることは簡単で、バイナリエディタで値を取得し、本来の文字との値の差分を取るとフラグになる、というだけです。

最初の7文字は、`rgbCTF{` なので、以下は8文字目以降です。

<br />
文字化けしている箇所のHex値は以下の通り。（8文字目以降）
<pre>
E7, D5, 9F, DE, D1, A6, D3, E1, A1, A2, E2, 9F, C1, D7, 7D
</pre>

<br />
本来、入るべきの文字は以下の通り。（8文字目以降）
<pre>
's', 'c', 'n', 'n', 'e', 's', 't', 't', 'n', 'n', 't', 'n', 'S', 'p', '}'
</pre>

<br />
ということで、
<pre>
$ python3
>>> chr(0xE7-ord('s'))
't'
>>> chr(0xD5-ord('c'))
'r'
>>> chr(0x9F-ord('n'))
'1'
>>> chr(0xDE-ord('n'))
'p'
>>> chr(0xD1-ord('e'))
'l'
>>> chr(0xA6-ord('s'))
'3'
>>> chr(0xD3-ord('t'))
'_'
>>> chr(0xE1-ord('t'))
'm'
>>> chr(0xA1-ord('n'))
'3'
>>> chr(0xA2-ord('n'))
'4'
>>> chr(0xE2-ord('t'))
'n'
>>> chr(0x9F-ord('n'))
'1'
>>> chr(0xC1-ord('S'))
'n'
>>> chr(0xD7-ord('p'))
'g'
>>> chr(0x7D-ord('}'))
'\x00'
</pre>

<br />

Flag: `rgbCTF{tr1pl3_m34n1ng}`

<br />
Triple Meaning (どこがトリプル?)



<br /><br />
<br /><br />
## [Pwn/Rev]: Too Slow
- - -
### Challenge
> I've made this flag decryptor! It's super secure, but it runs a little slow.
<br />

Attachment:

- a.out

<br />
### Solution
Ghidraでデコンパイルします。

```C
undefined8 main(void)

{
  uint uVar1;
  
  puts("Flag Decryptor v1.0");
  puts("Generating key...");
  uVar1 = getKey();
  win((ulong)uVar1);
  return 0;
}

ulong getKey(void)

{
  uint local_10;
  uint local_c;
  
  local_10 = 0;
  while (local_10 < 0x265d1d23) {
    local_c = local_10;
    while (local_c != 1) {
      if ((local_c & 1) == 0) {
        local_c = (int)local_c / 2;
      }
      else {
        local_c = local_c * 3 + 1;
      }
    }
    local_10 = local_10 + 1;
  }
  return (ulong)local_10;
}
```

main() を見ると、getKey() の戻り値を win() の引数にして呼んでいるのがわかります。

getKey() では、local_10 が 0x265d1d23 になるまで特定の計算をしつつループします。これが時間かかります。

local_10 は 1 ずつインクリメントしているだけなので、戻り値は計算するまでもなく 0x265d1d23 になります。

よって、単純に win() に 0x265d1d23 を渡してやるとフラグが取れます。

<br />
gdbを使って解きました。

{{< highlight asm "linenos=table,hl_lines=14 15 16" >}}
gef➤  disas main
Dump of assembler code for function main:
   0x00005555555552af <+0>:	endbr64 
   0x00005555555552b3 <+4>:	push   rbp
   0x00005555555552b4 <+5>:	mov    rbp,rsp
   0x00005555555552b7 <+8>:	sub    rsp,0x10
   0x00005555555552bb <+12>:	mov    DWORD PTR [rbp-0x4],edi
   0x00005555555552be <+15>:	mov    QWORD PTR [rbp-0x10],rsi
   0x00005555555552c2 <+19>:	lea    rdi,[rip+0xd54]        # 0x55555555601d
   0x00005555555552c9 <+26>:	call   0x555555555070 <puts@plt>
   0x00005555555552ce <+31>:	lea    rdi,[rip+0xd5c]        # 0x555555556031
   0x00005555555552d5 <+38>:	call   0x555555555070 <puts@plt>
   0x00005555555552da <+43>:	mov    eax,0x0
=> 0x00005555555552df <+48>:	call   0x555555555257 <getKey>
   0x00005555555552e4 <+53>:	mov    edi,eax
   0x00005555555552e6 <+55>:	call   0x555555555189 <win>
   0x00005555555552eb <+60>:	mov    eax,0x0
   0x00005555555552f0 <+65>:	leave  
   0x00005555555552f1 <+66>:	ret

gef➤  set $edi = 0x265d1d23
gef➤  jump win
Continuing at 0x555555555189.
Your flag: rgbCTF{pr3d1ct4bl3_k3y_n33d5_no_w41t_cab79d}

{{< / highlight >}}

<br />

Flag: `rgbCTF{pr3d1ct4bl3_k3y_n33d5_no_w41t_cab79d}`


<br /><br />
<br /><br />
## [Beginner]: Shoob
- - -
### Challenge
> s h o o b

Attachment:

- shoob.png


<br />
### (Unsolved)
「青い空を見上げればいつもそこに白い猫」の「左ローテート一回」が一番見やすかったです。

<img src="https://captureamerica.github.io/writeups/img/rgb_CTF_2020_shoob_solved.png" alt="rgb_CTF_2020_shoob_solved.png">

ただし、字がちゃんと読めなくて、フラグが通りませんでした。。。めんどくさくなって、降参。



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
