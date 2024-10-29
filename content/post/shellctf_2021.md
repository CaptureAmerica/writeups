---
title: "S.H.E.L.L. CTF 2021 Writeup"
date: 2021-06-08T13:00:00+09:00
lastmod: 2021-06-08T13:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fshellctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://shellctf.games/challenges](https://shellctf.games/challenges)
<br /><br />

1380点を獲得し、最終順位は116位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/shellctf_2021_score1.png" alt="shellctf_2021_score1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/shellctf_2021_score2.png" alt="shellctf_2021_score2.png">

<br><br>

以下はチャレンジのリストです。<br />

<img src="https://captureamerica.github.io/writeups/img/shellctf_2021_solves1.png" alt="shellctf_2021_solves1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/shellctf_2021_solves2.png" alt="shellctf_2021_solves2.png">

<br><br>



<br /><br />
## [Web]: Under Development (50 points)
- - -
### Challenge
> http://3.142.122.1:8885/

<br />

### Solution
HTTPレスポンスを見ると、以下のCookieがセットされるのがわかります。

<pre>
HTTP/1.1 200 OK
X-Powered-By: Express
Set-Cookie: privilege=dXNlcg%3D%3D; Path=/   <---
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Sun, 30 May 2021 06:01:58 GMT
ETag: W/"15b-179bbdd5ff0"
Content-Type: text/html; charset=UTF-8
Content-Length: 347
Date: Sat, 05 Jun 2021 08:11:22 GMT
Connection: close
</pre>

<br />

%3D%3D は `==` なので、Base64できそうです。デコードしたところ、`user`でした。

`admin`をBase64エンコードして、HTTPリクエストのCookieにセットするだけです。

<br />

Flag: `SHELL{0NLY_0R30_8e1a91a632ecaf2dd6026c943eb3ed1e}`



<br /><br />
## [Web]: Collide (100 points)
- - -
### Challenge
> http://3.142.122.1:9335/

<br />

### Solution
アクセスすると、ソースコードが表示されます。

```php
Make sha256 collide and you shall be rewarded
The source!
<html>
    <body>
        <h1> Make sha256 collide and you shall be rewarded </h1>

        <b>The source!</b>

        <?php
            $source = show_source("index.php", true);
            echo("<div>");
            print $source;
            echo("</div>");

            if (isset($_GET['shell']) && isset($_GET['pwn'])) {
                if ($_GET['shell'] !== $_GET['pwn'] && hash("sha256", $_GET['shell']) === hash("sha256", $_GET['pwn'])) {
                    include("flag.php");
                    echo("<h1>$flag</h1>");
                } else {
                    echo("<h1>Try harder!</h1>");
                }
            } else {
                echo("<h1>Collisions are fun to see</h1>");
            }
        ?>
    </body>
</html>
```

"sha256 collision CTF" をキーワードでググったら、同じ過去問を見つけてしまったんですよね。

[https://ctftime.org/task/12375](https://ctftime.org/task/12375)

<br />

以下でアクセスするとフラグが取れます。
<pre>
http://3.142.122.1:9335/?shell[0]=0&pwn[1]=0
</pre>

<br />

Flag: `SHELL{1nj3ct_&_coll1d3_9d25f1cfdeb38a404b6e8584bec7a319}`



<br /><br />
## [Web]: login (200 points)
- - -
### Challenge
> Sam really need to get past this login portal but isn't able too, can you help him? http://3.142.122.1:8889/

<br />

### Solution
main.jsを使って、ローカルで認証が走ります。

結構複雑なコードのわりにSolveした人が多かったので、なんか簡単な解き方がありそう。

```javascript
function checkIt() {
  var user = document.getElementById("username").value; var pass = document.getElementById("password").value;
  if (user != "din_djarin11") alert("Only for user: din_djarin11"); else {
    var s = Hash(pass);
    if (s == "9ef71a8cd681a813cfd377817e9a08e5") window.location = "./" + pass; 
    else alert("Invalid login");
  }
}
:
(snip。複雑なコードの部分は長いので省略)
:
```

<br />
開催日初日には、`9ef71a8cd681a813cfd377817e9a08e5`をググっても何もヒットしなかったんですが、2日目にググったら`ir0nm4n`がヒットしました。

ということで、din_djarin11 / ir0nm4n でログインするだけです。

<br />

Flag: `SHELL{th1s_i5_th3_wa7_845ad42f4480104b698c1e168d29b739}`





<br /><br />
## [Crypto]: Subsi (50 points)
- - -
### Challenge
> cipher : HITSS{5X65Z1ZXZ10F_E1LI3J}

Attachment:

- script.py

以下、中身。

```Python
alpha = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_1234567890'
key   = 'QWERTPOIUYASDFGLKJHZXCVMNB{}_1234567890'

text = <flag>

def encrypter(text,key):
    encrypted_msg = ''
    for i in text:
        index = alpha.index(i)
        encrypted_msg += key[index]
    # print(encrypted_msg)
    return encrypted_msg
```


<br />

### Solution
逆の処理をしてdecryptするだけです。

{{< highlight python "linenos=table,hl_lines=14-19" >}}
alpha = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_1234567890'
key   = 'QWERTPOIUYASDFGLKJHZXCVMNB{}_1234567890'

# text = <flag>

def encrypter(text,key):
    encrypted_msg = ''
    for i in text:
        index = alpha.index(i)
        encrypted_msg += key[index]
    # print(encrypted_msg)
    return encrypted_msg

text = "HITSS{5X65Z1ZXZ10F_E1LI3J}"
decrypted_msg = ''
for i in text:
    index = key.index(i)
    decrypted_msg += alpha[index]
print(decrypted_msg)
{{< / highlight >}}

<br />

Flag: `SHELL{5U65T1TUT10N_C1PH3R}`

Pythonで文字列のIndexのとり方が勉強になりました。



<br /><br />
## [Crypto]: Cjk (92 points)
- - -
### Challenge
> There is a locker protected with a pin.
<br /><br />
Can you help me out in finding the pin in the image, and there is some labeling side of it.
<br /><br />
Note : flag is not in SHELL{} format, just a positive number.

Attachment:

<img src="https://captureamerica.github.io/writeups/img/shellctf_2021_pin.png" alt="shellctf_2021_pin.png">


<br />
### (Unsolved)
CJKというのは、Chinese Japanese Koreanのイニシャルでした。

郵便マーク (U+3036)、JISマーク (U+3004) はいいとして、3つ目の記号はなんだよ！

もう少し判別つきやすいものを使ってほしいところ。




<br /><br />
## [Rev]: assembly (50 points)
- - -
### Challenge
> fun1(0x74,0x6f) + fun1(0x62,0x69) = ?

Attachment:

- asm_code.txt

以下、中身です。

```asm
fun1:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	sub    esp,0x10
	<+6>:	mov    eax,DWORD PTR [ebp+0xc]
	<+9>:	mov    DWORD PTR [ebp-0x4],eax
	<+12>:	mov    eax,DWORD PTR [ebp+0x8]
	<+15>:	mov    DWORD PTR [ebp-0x8],eax
	<+18>:	jmp    <fun1+28>
	<+20>:	add    DWORD PTR [ebp-0x4],0x7
	<+24>:	add    DWORD PTR [ebp-0x8],0x70
	<+28>:	cmp    DWORD PTR [ebp-0x8],0x227
	<+35>:	jle    <fun1+20>
	<+37>:	mov    eax,DWORD PTR [ebp-0x4]
	<+40>:	leave  
	<+41>:	ret  
```


<br />

### Solution
C言語に書き起こしました。

```C
#include <stdio.h>

int fun1(int a, int b)
{
    b = b + 7;
    a = a + 0x70;
    while ( a <= 0x227 ) {
	b = b + 7;
	a = a + 0x70;
    }
    return b;
}

void main()
{
    printf("0x%x\n", fun1(0x74,0x6f) + fun1(0x62,0x69));
}
```

<pre>
$ gcc assembly_solve.c
$ ./a.out
0x117
</pre>

<br />

Flag: `SHELL{0x117}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

