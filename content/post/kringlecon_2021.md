---
title: "SAN Holiday Hack Challenge (Kringlecon) 2021 Writeup"
date: 2022-01-19T16:00:00+09:00
lastmod: 2022-01-19T16:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fkringlecon_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://2021.kringlecon.com/](https://2021.kringlecon.com/)
<br /><br />

新年明けまして おめでとうございます！

OSCPの勉強を少し休憩して、SANS の Holiday Hack Challenge (Kringlecon) 2021 に参加しました。

ちょっとしかやってないですが、勉強になったこともあるので、自分の覚書きとして記録を残しておきます。

<br />

<br /><br />
## Yara Analysis
- - -
### Challenge
> HELP!!!
<br /><br />
This critical application is supposed to tell us the sweetness levels of our candy
manufacturing output (among other important things), but I can't get it to run.
<br /><br />
It keeps saying something something yara. Can you take a look and see if you
can help get this application to bypass Sparkle Redberry's Yara scanner?
<br /><br />
If we can identify the rule that is triggering, we might be able change the program
to bypass the scanner.
<br /><br />
We have some tools on the system that might help us get this application going:
vim, emacs, nano, yara, and xxd
<br /><br />
The children will be very disappointed if their candy won't even cause a single cavity.


<br />
### Solution
まずは、ホームにあるファイルと、プログラムを実行したときの結果を以下に示します。
<pre>
snowball2@43ca77f5017d:~$ ls -al
total 44
drwxr-xr-x 1 snowball2 snowball2  4096 Dec  2 14:25 .
drwxr-xr-x 1 root      root       4096 Dec  2 14:25 ..
-rw-r--r-- 1 snowball2 snowball2   220 Feb 25  2020 .bash_logout
-r-xr-xr-x 1 snowball2 snowball2  3926 Dec  2 14:25 .bashrc
-r-xr-xr-x 1 snowball2 snowball2   807 Feb 25  2020 .profile
-rw-r--r-- 1 root      root          0 Dec  2 14:25 .sudo_as_admin_successful
-rwxr-xr-x 1 snowball2 snowball2 16688 Nov 24 15:51 the_critical_elf_app
drwxr-xr-x 1 root      root       4096 Dec  2 14:25 yara_rules

snowball2@43ca77f5017d:~$ find yara_rules/
yara_rules/
yara_rules/rules.yar

snowball2@43ca77f5017d:~$ ./the_critical_elf_app 
yara_rule_135 ./the_critical_elf_app

snowball2@43ca77f5017d:~$ 
</pre>

<br />

プログラムを実行すると、どのyaraルールでひっかかったのかが表示されます。

バイナリをいじってプログラムを改変し、yara ruleにひっかからないようにする、というのがお題です。

<br />
該当ルールを見てみます。

emacsがあるということだったので、emacsを使ってみようとしたのですが、ブラウザ内で動いているためにemacsショートカットがうまく使えずgrepで見ることにしました。

<pre>
snowball2@7d7d6314aac4:~/yara_rules$ grep "yara_rule_135 " -A20 rules.yar 
rule yara_rule_135 {
   meta:
      description = "binaries - file Sugar_in_the_machinery"
      author = "Sparkle Redberry"
      reference = "North Pole Malware Research Lab"
      date = "1955-04-21"
      hash = "19ecaadb2159b566c39c999b0f860b4d8fc2824eb648e275f57a6dbceaf9b488"
   strings:
      $s = "candycane"
   condition:
      $s
}
:
</pre>

<br />

今回、学んだこと： 以下のようにバイナリ編集が可能

- vim -b &lt;program>
- :%!xxd
- 編集
- :%!xxd -r
- :wq

<br />
上記のやり方で、elfファイルの中のstring部分を変更していくだけです。

ひとつ直すと別のyaraルールにひっかかり、合計3つひっかかります。

最後のルールは、以下のようでした。

<pre>
rule yara_rule_1732 {
   meta:
      description = "binaries - alwayz_winter.exe"
      author = "Santa"
      reference = "North Pole Malware Research Lab"
      date = "1955-04-22"
      hash = "c1e31a539898aab18f483d9e7b3c698ea45799e78bddc919a7dbebb1b40193a8"
   strings:
      $s1 = "This is critical for the execution of this program!!" fullword ascii
      $s2 = "__frame_dummy_init_array_entry" fullword ascii
      $s3 = ".note.gnu.property" fullword ascii
      $s4 = ".eh_frame_hdr" fullword ascii
      $s5 = "__FRAME_END__" fullword ascii
      $s6 = "__GNU_EH_FRAME_HDR" fullword ascii
      $s7 = "frame_dummy" fullword ascii
      $s8 = ".note.gnu.build-id" fullword ascii
      $s9 = "completed.8060" fullword ascii
      $s10 = "_IO_stdin_used" fullword ascii
      $s11 = ".note.ABI-tag" fullword ascii
      $s12 = "naughty string" fullword ascii
      $s13 = "dastardly string" fullword ascii
      $s14 = "__do_global_dtors_aux_fini_array_entry" fullword ascii
      $s15 = "__libc_start_main@@GLIBC_2.2.5" fullword ascii
      $s16 = "GLIBC_2.2.5" fullword ascii
      $s17 = "its_a_holly_jolly_variable" fullword ascii
      $s18 = "__cxa_finalize" fullword ascii
      $s19 = "HolidayHackChallenge{NotReallyAFlag}" fullword ascii
      $s20 = "__libc_csu_init" fullword ascii
   condition:
      uint32(1) == 0x02464c45 and filesize < 50KB and
      10 of them
}
</pre>

<br />
いくつかある条件のうちの、`filesize < 50KB` を回避することにしました。

<pre>
$ python3 -c 'print("A"*40000)' >> the_critical_elf_app
</pre>

<br />

<pre>
snowball2@7d7d6314aac4:~$ ./the_critical_elf_app 
/usr/local/bin/pre_execution.sh: line 26:   336 Killed                  yara /home/snowball2/yara_rules/rules.yar $1 -l 1
Machine Running.. 
Toy Levels: Very Merry, Terry
Naughty/Nice Blockchain Assessment: Untampered
Candy Sweetness Gauge: Exceedingly Sugarlicious
Elf Jolliness Quotient: 4a6f6c6c7920456e6f7567682c204f76657274696d6520417070726f766564
</pre>

いちおう、クリアになりました。



<br /><br />
<br /><br />
## Elf Code Python Bonus Levels!
- - -
### Challenge
> Hello, I'm Ribb Bonbowford. Nice to meet you!
<br /><br />
Are you new to programming? It's a handy skill for anyone in cyber security.
<br /><br />
This here machine lets you control an Elf using Python 3. It’s pretty fun, but I’m having trouble getting beyond Level 8.
<br /><br />
Tell you what… if you help me get past Level 8, I’ll share some of my SQLi tips with you. You may find them handy sometime around the North Pole this season.
<br /><br />
Most of the information you'll need is provided during the game, but I'll give you a few more pointers, if you want them.
<br /><br />
Not sure what a lever requires? Click it in the Current Level Objectives panel.
<br /><br />
You can move the elf with commands like elf.moveLeft(5), elf.moveTo({"x":2,"y":2}), or elf.moveTo(lever0.position).
<br /><br />
Looping through long movements? Don't be afraid to moveUp(99) or whatever. You elf will stop at any obstacle.
<br /><br />
You can call functions like myFunction(). If you ever need to pass a function to a munchkin, you can use myFunction without the ().


<br />
### Solution
正直、問題文を見てもなんだかわからないですが、やっていくうちにわかります。

Paizaとかにある、プログラムを書いてクリアしていくゲームみたいなやつです。

問題文によって、コードの行数の上限と、関数の呼び出し回数が制限されています。

Level 10まであって、いちおう全部解きました。

例として、以下は Level 5で書いたコードになります。
{{< highlight python "linenos=table,hl_lines=20" >}}
import elf, munchkins, levers, lollipops, yeeters, pits
lever0, lever1, lever2, lever3, lever4 = levers.get()
leverData = lever4.data() + " concatenate"
elf.moveTo(lever4.position)
lever4.pull(leverData)
leverData = lever3.data()
elf.moveTo(lever3.position)
if bool(leverData):
    lever3.pull(False)
else:
    lever3.pull(True)
leverData = lever2.data() + 1
elf.moveTo(lever2.position)
lever2.pull(leverData)
leverData = lever1.data()
leverData.append(1)
elf.moveTo(lever1.position)
lever1.pull(leverData)
leverData = lever0.data() # {"key":"4"}
leverData["strkey"] = "strvalue"
elf.moveTo(lever0.position)
lever0.pull(leverData)
elf.moveUp(2)
{{< / highlight >}}

<br />
dictionaryって値の変更はできないけど、追加は普通にできるんだったか。(ハイライト部分)

<br />
全Levelについて書くのは面倒なので、最後のLevel 10のやつだけ残しておきます。

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2021_Level10_1.png" alt="kringlecon_2021_Level10_1.png">

<br />

以下が書いたコードです。17行以内におさめる必要がありました。
```Python
import elf, munchkins, levers, lollipops, yeeters, pits, time
muns = munchkins.get()
lols = lollipops.get()[::-1]
elf.moveRight(1)
for index, mun in enumerate(muns):
        while 1:
            if abs(elf.position["x"] - mun.position["x"]) >= 6:
                if index % 2 == 1:
                    elf.moveRight(2)
                else:
                    elf.moveLeft(2)
            if elf.position["x"] == lols[2-index].position["x"]:
                elf.moveUp(2)
                break
            time.sleep(0.05)
elf.moveLeft(6)
elf.moveUp(2)
```

<br />
マンチキンと十分に距離が離れているとき（距離の絶対値が６以上のとき）にのみ動くようにしてあります。

1マスずつ動くと、関数コールの回数が多すぎということでクリアにならないので、2マスずつ動いています。

（ここを2ではなく4マス動くコードにすると、マンチキンと衝突してしまいます。）

スタート位置は、通路から3マス離れたところから始まるので、最初にまず1マス右に動いています。(4行目は、そういうことです)

<img src="https://captureamerica.github.io/writeups/img/kringlecon_2021_Level10_2.png" alt="kringlecon_2021_Level10_2.png">



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
