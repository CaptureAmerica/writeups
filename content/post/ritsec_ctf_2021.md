---
title: "RITSEC CTF 2021 Writeup"
date: 2021-04-13T13:00:00+09:00
lastmod: 2021-04-13T18:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fritsec_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.ritsec.club/challenges](https://ctf.ritsec.club/challenges)
<br /><br />

2235ポイントで、最終順位は128番でした。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2021_score1.png" alt="ritsec_ctf_2021_score1.png">

<br />

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2021_score2.png" alt="ritsec_ctf_2021_score2.png">

<br /><br />
以下は、チャレンジ一覧です。Unlockできてないやつも、もしかしたらあるかもです。

結果的に、Forensicsばっかりになってしまいました。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2021_chall_list1.png" alt="ritsec_ctf_2021_chall_list1.png"> <br />
<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2021_chall_list2.png" alt="ritsec_ctf_2021_chall_list2.png">
<br /><br />

以下、Writeupです。Webと101は省略します。

<br /><br />
## [Rev]: snek (100 points)
- - -
### Challenge
> No step on snek

Attachments:

- snek

<br />
### Solution
まずは、ファイル・タイプの確認をします。

<pre>
$ file snek
snek: python 3.7 byte-compiled
</pre>

ググったところ、どうやらDecompyle++ というのを使うとよさそう。

[https://github.com/zrax/pycdc](https://github.com/zrax/pycdc)

<br />
以下はインストールの仕方です。

<pre>
$ git clone https://github.com/zrax/pycdc.git
$ cd pycdc
$ cmake .
$ make
</pre>

{{< highlight asm "linenos=table,hl_lines=63-87" >}}
$ pycdas snek 
:
(snip)
:
                    [Disassembly]
                        0       LOAD_FAST               1: password
                        2       LOAD_METHOD             0: encode
                        4       CALL_METHOD             0
                        6       LOAD_FAST               0: self
                        8       STORE_ATTR              1: password
                        10      LOAD_CONST              1: 97
                        12      LOAD_CONST              2: 98
                        14      LOAD_CONST              3: 99
                        16      LOAD_CONST              4: 100
                        18      LOAD_CONST              5: 101
                        20      LOAD_CONST              6: 102
                        22      LOAD_CONST              7: 103
                        24      LOAD_CONST              8: 104
                        26      LOAD_CONST              9: 105
                        28      LOAD_CONST              10: 106
                        30      LOAD_CONST              11: 107
                        32      LOAD_CONST              12: 108
                        34      LOAD_CONST              13: 109
                        36      LOAD_CONST              14: 110
                        38      LOAD_CONST              15: 111
                        40      LOAD_CONST              16: 112
                        42      LOAD_CONST              17: 113
                        44      LOAD_CONST              18: 114
                        46      LOAD_CONST              19: 115
                        48      LOAD_CONST              20: 116
                        50      LOAD_CONST              21: 117
                        52      LOAD_CONST              22: 118
                        54      LOAD_CONST              23: 119
                        56      LOAD_CONST              24: 120
                        58      LOAD_CONST              25: 121
                        60      LOAD_CONST              26: 122
                        62      LOAD_CONST              27: 65
                        64      LOAD_CONST              28: 66
                        66      LOAD_CONST              29: 67
                        68      LOAD_CONST              30: 68
                        70      LOAD_CONST              31: 69
                        72      LOAD_CONST              32: 70
                        74      LOAD_CONST              33: 71
                        76      LOAD_CONST              34: 72
                        78      LOAD_CONST              35: 73
                        80      LOAD_CONST              36: 74
                        82      LOAD_CONST              37: 75
                        84      LOAD_CONST              38: 76
                        86      LOAD_CONST              39: 77
                        88      LOAD_CONST              40: 78
                        90      LOAD_CONST              41: 79
                        92      LOAD_CONST              42: 80
                        94      LOAD_CONST              43: 81
                        96      LOAD_CONST              44: 82
                        98      LOAD_CONST              45: 83
                        100     LOAD_CONST              46: 84
                        102     LOAD_CONST              47: 85
                        104     LOAD_CONST              48: 86
                        106     LOAD_CONST              49: 87
                        108     LOAD_CONST              50: 88
                        110     LOAD_CONST              51: 89
                        112     LOAD_CONST              52: 90
                        114     LOAD_CONST              53: 95
                        116     LOAD_CONST              44: 82
                        118     LOAD_CONST              45: 83
                        120     LOAD_CONST              54: 123
                        122     LOAD_CONST              1: 97
                        124     LOAD_CONST              12: 108
                        126     LOAD_CONST              12: 108
                        128     LOAD_CONST              53: 95
                        130     LOAD_CONST              8: 104
                        132     LOAD_CONST              9: 105
                        134     LOAD_CONST              55: 36
                        136     LOAD_CONST              55: 36
                        138     LOAD_CONST              53: 95
                        140     LOAD_CONST              1: 97
                        142     LOAD_CONST              14: 110
                        144     LOAD_CONST              4: 100
                        146     LOAD_CONST              53: 95
                        148     LOAD_CONST              14: 110
                        150     LOAD_CONST              56: 48
                        152     LOAD_CONST              53: 95
                        154     LOAD_CONST              2: 98
                        156     LOAD_CONST              9: 105
                        158     LOAD_CONST              20: 116
                        160     LOAD_CONST              57: 51
                        162     LOAD_CONST              58: 125
                        164     BUILD_LIST              77
                        166     LOAD_FAST               0: self
                        168     STORE_ATTR              2: decrypt
                        170     LOAD_CONST              0: None
                        172     RETURN_VALUE
:
(snip)
:
{{< / highlight >}}

<br />

上の方は数字が連番ですが、ハイライトした辺りは数字が不規則なのでそこに当たりを付けました。

数字を文字に変換すると、{all_hi$$_and_n0_bit3} になります。

<br />

Flag: `RS{all_hi$$_and_n0_bit3}`




<br /><br />
<br /><br />
## [Forensics]: 1597 (100 points)
- - -
### Challenge
> ... as in https://xkcd.com/1597/
<br /><br />
[http://git.ritsec.club:7000/1597.git/](http://git.ritsec.club:7000/1597.git/)


<br />
### Solution
git cloneしてfull historyを見るだけです。

<pre>
$ git clone http://git.ritsec.club:7000/1597.git/

$ cd 1597

$ git log -p --all --full-history
commit dcc402050827e92dbcf2578e24f2cba76f34229c (HEAD -> master, origin/master, origin/HEAD)
Author: knif3 <knif3@mail.rit.edu>
Date:   Fri Apr 9 05:49:00 2021 +0000

    Updated the flag

diff --git a/flag.txt b/flag.txt
index a24cab4..8b13789 100644
--- a/flag.txt
+++ b/flag.txt
@@ -1 +1 @@
-Your princess is in another castle
+

commit b123f674a07eaf5914eda8845d86b5219fc1de11 (origin/!flag)
Author: knif3 <knif3@mail.rit.edu>
Date:   Fri Apr 9 05:49:00 2021 +0000

    More flags

diff --git a/README.md b/README.md
index 0ff84f3..99ddfa8 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,3 @@
 # 1597

-A git challenge series? Sounds fun.
+What's worse, basing a CTF challenge off of XKCD, or basing a challenge off of git?
diff --git a/flag.txt b/flag.txt
index 8b13789..013a6dd 100644
--- a/flag.txt
+++ b/flag.txt
@@ -1 +1 @@
-
+RS{git_is_just_a_tre3_with_lots_of_branches}
</pre>

<br />

Flag: `RS{git_is_just_a_tre3_with_lots_of_branches}`



<br /><br />
<br /><br />
## [Forensics]: FYSA (100 points)
- - -
### Challenge
> (記載なし)

Attachments:

- BIRDTHIEF_FYSA.pdf

<br />
### Solution
Libre OfficeでPDFファイルを開いて4ページ目にある。黒塗りをマウスで移動。

<br />

Flag: `RS{Make_sure_t0_read_the_briefing}`



<br /><br />
<br /><br />
## [Forensics]: Parcel (200 points)
- - -
### Challenge
> That's a lot of magick

Attachments:

- Parcel

<br />
### Solution
まずは、ファイル・タイプの確認をします。

<pre>
$ file Parcel
Parcel: gzip compressed data, from Unix, original size modulo 2^32 759456
</pre>

解凍すると、メールのtcp streamが入ったテキストが出てきます。メールの添付ファイルはbase64 decodeをしたら取り出せます。

添付ファイルは沢山あって、数個やってみたところ、分割されたフラグの画像のようです。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2021_Parcel_RS.png" alt="ritsec_ctf_2021_Parcel_RS.png">

スクリプトを書いた方が早いかな、と思って以下を書いて添付ファイルを全部取り出しました。

```Python
#!/usr/bin/env python3
#-*- coding:utf-8 -*-
import base64

lines = open("Parcel",'r').read().split("\n")
i = 1
flag = 0
data = ""
for line in lines:
    if flag == 2:
        if line == "":
            filename = str(i)+".png"
            wf = open(filename,'wb')
            wf.write(base64.b64decode(data))
            wf.close()
            flag = 0
            i += 1
            continue
        else:
            data += line
    if flag == 1:
        if line == "":
            flag = 2
            data = ""
    if "Content-Transfer-Encoding: base64" in line:
        flag = 1
```

コードの中のflagは、base64の部分を特定するためにスイッチとして使っているものです。

出てきた画像の中でフラグを含んでいるものを、Libre Officeで読み込んでパズルのようにくっつけました。

<img src="https://captureamerica.github.io/writeups/img/ritsec_ctf_2021_Parcel.png" alt="ritsec_ctf_2021_Parcel.png">

<br />

Flag `RS{Im_doing_a_v1rtual_puzzl3}`



<br /><br />
<br /><br />
## [Forensics]: Blob (200 points)
- - -
### Challenge
> Ha. Blob. Did you get the reference?
<br /><br />
[http://git.ritsec.club:7000/blob.git/](http://git.ritsec.club:7000/blob.git/)


<br />
### Solution

packed-refsファイルの中身は以下のようになっていました。

<pre>
# pack-refs with: peeled fully-peeled sorted 
a69cb6306e8b75b6762d6aa1b0279244cacf3f3b refs/remotes/origin/master
d0644363aa853a17c9672cefff587580a43cf45e refs/tags/flag
</pre>

<br />

"git packed-refs CTF"でググったら、どうやら [OTW/bandit](https://overthewire.org/wargames/bandit/) にあるチャレンジと同じタイプのようです。

<pre>
$ git show flag
RS{refs_can_b3_secret_too}
</pre>

<br />

Flag: `RS{refs_can_b3_secret_too}`



<br /><br />
<br /><br />
## [Forensics]: PleaseClickAlltheThings 1: BegineersRITSEC.html (150 points)
- - -
### Challenge
> Note: this challenge is the start of a series of challenges. The purpose of this CTF challenge is to bring real world phishing attachments to the challengers and attempt to find flags (previously executables or malicious domains) within the macros. This is often a process used in IR teams and becomes an extremely valuable skill. In this challenge we’ve brought to the table a malicious html file, GandCrab/Ursnif sample, and a IceID/Bokbot sample. We’ve rewritten the code to not contain malicious execution however system changes may still occur when executing, also some of the functionalities have been snipped and will likely not expose itself via dynamic analysis.
<br /><br />
\- Outlook helps, with proper licensing to access necessary features. Otherwise oledump or similar would also help but isn’t necessary
<br /><br />
\- CyberChef is the ideal tool to use for decoding
<br /><br />
Part 1: Start with the HTML file and let’s move our way up, open and or inspect the HTML file provide in the message file. There is only one flag in this document.
<br /><br />
This challenge is brought to you by SRA

Attachments:

- Please Click all the Things.msg


<br />
### Solution
Macを使って調べていて、*.msgファイルを開くツールがなかったので、「foremostで添付ファイルを取り出せばいいや」と思ったのが原因で、結構ハマリました。

なぜなら、foremostではhtmlファイルが出てこないからです。

マクロを調査してフラグっぽいのも取れているのに、Invalid Flagの連発だったので、ちゃんとmsgファイルを開くことにしました。

<pre>
$ sudo gem install ruby-msg
$ mapitool -i *.msg
</pre>

これで、msgファイルは、emlファイルに変換されます。

<br />
emlファイルはMacのMailアプリで開けるんですが、なんかしらアカウントの設定をしないとMailアプリが使えないというのはイケてない所ですね。。

<br />
emlファイルを開くと、ちゃんとhtmlファイルが添付されていました。

<br />
URLエンコードされた文字列が見つかるのでデコードしてやって、更にBase64デコードするだけです。

<br />

Flag: `RITSEC{H3r3!t!$}`



<br /><br />
<br /><br />
## [Forensics]: PleaseClickAlltheThings 2: GandCrab_Ursnif (200 points)
- - -
### Challenge
> NOTE: this challenge builds upon BegineersRITSEC.html, and that challenge must be completed first.
<br /><br />
GandCrab/Ursnif are dangerous types of campaigns and malware, macros are usually the entry point, see what you can find, there are two flags in this document. Flag1/2


<br />
### Solution
これは、単にマクロの中で見つかるRITSEC{M@CROS}という文字列でした。

<br />

Flag: `RITSEC{M@CROS}`



<br /><br />
<br /><br />
## [Forensics]: Please Click All the Things 3: IceID (350 points)
- - -
### Challenge
> NOTE: this challenge builds upon BegineersRITSEC.html, and that challenge must be completed first.
<br /><br />
Stepping it up to IceID/Bokbot, this challenge is like the previous challenge but requires some ability to read and understand coding in addition to some additional decoding skills, there are two flags in this document. (Flag 1/2)


<br />
### Solution
難読化されたVBAマクロを解析するチャレンジです。

以下がオリジナル。

```Basic
Rem Attribute VBA_ModuleType=VBAModule
Option VBASupport 1
Public Const aHVWt As String = "p_:_\_j_v_a_q_b_j_f_\_f_l_f_g_r_z_3_2_\_z_f_u_g_n__r_k_r_"
Public Const aqv6tf As String = "EVGFRP{E0GG1ATZ@YP0Q3}"

Public Const a7sVN As String = "_"
Public Const asXlUN As Integer = -954 + 967
Public Function aENoBO(aHu95, avuEG8)
FileNumber = FreeFile
Open aHu95 For Output As #FileNumber
Print #FileNumber, Spc(-413 + 456)
Print #FileNumber, avuEG8
Print #FileNumber, Spc(-413 + 456)
Close #FileNumber
End Function
Sub aUoaN(adDgz, at09Aq)
FileCopy adDgz, at09Aq
End Sub
Function anPr56(aCl8i)
anPr56 = Len(aCl8i)
End Function
Function a79yA(aO0h5k)
a79yA = aO0h5k + 12324 / 474
End Function
Function aHScDO(aoza8) As String
Dim alc6yS As Long
Dim a9uRX As Integer
Dim agyvb As Integer
For alc6yS = 1 To anPr56(aoza8)
agyvb = 0
aFxdHY = VBA.Mid$(aoza8, alc6yS, 1)
a9uRX = Asc(aFxdHY)
If (a9uRX > 64 And a9uRX < 91) Or (a9uRX > 96 And a9uRX < 123) Then
agyvb = asXlUN
a9uRX = a9uRX - agyvb
If a9uRX < 97 And a9uRX > 83 Then
a9uRX = a79yA(a9uRX)
ElseIf a9uRX < 65 Then
a9uRX = a79yA(a9uRX)
End If
End If
Mid$(aoza8, alc6yS, 1) = VBA.Chr$(a9uRX)
Next
aHScDO = aoza8
End Function

Rem Attribute VBA_ModuleType=VBAModule
Option VBASupport 1
Sub main()
auIPjp = aHScDO(aTwLcg(aHVWt))
aZuadn = aHScDO(aTwLcg(aqv6tf))
a9ANR = aHScDO(aTwLcg(aE0yGK))
aUoaN auIPjp, aZuadn
aENoBO a9ANR, aHScDO(aRXKz)
Shell aZuadn & " " & a9ANR
End Sub

Rem Attribute VBA_ModuleType=VBAModule
Option VBASupport 1
Rem removed unmatched Sub/End: Sub AutoOpen()
Function aRXKz()
aRXKz = frm.txt.Text
End Function
Public Function aTwLcg(alRUYI)
aTwLcg = Replace(alRUYI, a7sVN, "")
End Function
Sub AutoOpen()
main
End Sub
Public Sub a8hv3(ai295)
End Sub
Rem removed unmatched Sub/End: End Sub
```

<br />

以下は、がんばって書き換えをして、見やすくしたもの。

```Basic
Option VBASupport 1
Public Const str1 As String = "p_:_\_j_v_a_q_b_j_f_\_f_l_f_g_r_z_3_2_\_z_f_u_g_n__r_k_r_"
Public Const str2 As String = "EVGFRP{E0GG1ATZ@YP0Q3}"  ' たぶんフラグの元

Public Function func3(a, b)
       FileNumber = FreeFile
       Open a For Output As #FileNumber
       Print #FileNumber, Spc(43)
       Print #FileNumber, b
       Print #FileNumber, Spc(43)
       Close #FileNumber
End Function

Sub sub1(a, b)
    FileCopy a, b
End Sub

Function anPr56(a)
	 anPr56 = Len(a)
End Function

Function a79yA(a)
	 a79yA = a + 26
End Function

Function func2(a) As String
	 Dim i As Long
	 Dim x As Integer
	 Dim y As Integer

	 For i = 1 To anPr56(a)
	 	 y = 0
	 	 c = VBA.Mid$(a, i, 1)   ' iのポジションから1文字
	 	 x = Asc(c)
		 If (x > 64 And x < 91) Or (x > 96 And x < 123) Then
		     y = 13
		     x = x - y
		     If x < 97 And x > 83 Then
		     	x = a79yA(x)
		     ElseIf x < 65 Then
		     	x = a79yA(x)
		     End If
		 End If
		 Mid$(a, i, 1) = VBA.Chr$(x)
	Next
func2 = a
End Function


Option VBASupport 1
Sub main()
ret1 = func2(func1(str1))
ret2 = func2(func1(str2))
ret3 = func2(func1(aE0yGK))  ' これはどこに？ フラグとしては未使用？
sub1 ret1, ret2
func3 ret3, func2(aRXKz)
Shell ret2 & " " & ret3
End Sub


Option VBASupport 1

Function aRXKz()
	 aRXKz = frm.txt.Text
End Function

' "_"を取り除く。
Public Function func1(a)
       func1 = Replace(a, "_", "")
End Function

Sub AutoOpen()
    main
End Sub

Public Sub a8hv3(ai295)
End Sub
```

<br />

ポイントとなるのは上記の str2 と func2 で、結果的に単なるROTでした。

<br />

Flag: `RITSEC{R0TT1NGM@LC0D3}`



<br /><br />
<br /><br />
## [Forensics]: Inception CTF: Dream 1 (100 points)
- - -
### Challenge
> The purpose of this CTF challenge is to identify common methods of hiding malicious files and code. In most cases adversaries will attempt to evade defenses in many cases by masquerading, hiding files, and more. There are five directories like the five levels in the movie Inception, Reality -> Van Chase -> The Hotel -> Snow Fortress -> Limbo. You will find one flag in each of the levels, that flag will also be the password to extract the next directory. Requirements: • You must have 7zip installed • Drop the InceptionCTF.7z on the Desktop as “InceptionCTF” • Use the option “Extract to "<name of directory>\” for the CTF to function properly Missing either of the above may result in complications which may cause issues when attempting to find flags.
<br /><br />
NOTE: These challenges have a flag format of RITSEC{}
<br /><br />
Dream 1: We have to get to their subconscious first, look for a hidden text file within the directory “Reality” this flag will unlock the next directory.
<br /><br />
We would like to thank our sponsor @SRA for contributing this challenge!


Attachments:

- InceptionCTFRITSEC.7z

<br />
### Solution
このDreamシリーズは、解いたフラグが次の7zを解凍するためのパスワードになっているので、順番に解いていく必要があります。

解凍していくと、Reality.7zを解凍した時点で、Subconscious.txtというのが出てきます。

中身：

<pre>
Wait a minute, whose subconscious are we going into, exactly? {dnalmaerD}CESTIR
</pre>

<br />

<pre>
$ echo {dnalmaerD}CESTIR | rev
RITSEC}Dreamland{
</pre>

<br />

Flag: `RITSEC{Dreamland}`



<br /><br />
<br /><br />
## [Forensics]: Inception CTF: Dream 2 (225 points)
- - -
### Challenge
> Note: This challenge builds off Inception CTF: Dream 1,
<br /><br />
Unfortunately, the subconscious isn’t enough for this mission, we have to kidnap Fischer we need to go further into the system of the mind. Use the flag found to edit the PowerShell script, entering the Flag in line three in-between the single quotes. Run the PowerShell script and wait for it to complete its actions.
<br /><br />
Thanks to SRA for providing this challenge!


<br />
### Solution
解凍していくと、Kidnap.txtというのが出てきます。

中身：

<pre>
52 49 54 53 45 43 7b 57 61 74 65 72 55 6e 64 65 72 54 68 65 42 72 69 64 67 65 7d
</pre>

<br />

<pre>
$ python -c 'print("".join([chr(int(x,16)) for x in "52 49 54 53 45 43 7b 57 61 74 65 72 55 6e 64 65 72 54 68 65 42 72 69 64 67 65 7d".split()]))'
RITSEC{WaterUnderTheBridge}
</pre>

<br />

Flag: `RITSEC{WaterUnderTheBridge}`




<br /><br />
<br /><br />
## [Forensics]: Inception CTF: Dream 3 (200 points)
- - -
### Challenge
> Note: This challenge builds off Inception CTF: Dream 1,
<br /><br />
Note: This challenge builds on Inception CTF: Dream 2.
<br /><br />
While the first two steps were easy it’s all hard from here on out, ThePointMan is the most crucial role of the mission he has to be presentable but without giving away our intentions. Use Alternate Dream State to find the flag before the kick.


<br />
### Solution
解凍していくと、ThePointMan.txtというのが出てきます。

これは、XOR bruteや、quipquip、モールス信号、ヴィジュネル暗号などを使って解けるんですが、フラグとは関係ないdecoyだった模様。

<br />

別に、名無しのファイルが一個あって、その中身が以下です。

<pre>
You mean, a dream within a dream? NTIgNDkgNTQgNTMgNDUgNDMgN2IgNDYgNDAgMjEgMjEgNjkgNmUgNjcgNDUgNmMgNjUgNzYgNDAgNzQgNmYgNzIgN2Q=
</pre>

<br />

base64デコードをして、数字を文字に変えるだけです。

<br />

Flag: `RITSEC{F@!!ingElev@tor}`



<br /><br />
<br /><br />
## [Stego]: Inception CTF: Dream 4 (200 points)
- - -
### Challenge
> Note: This challenge builds off of InceptionCTF: Dream 3
<br /><br />
Don’t lose yourself within the dreams, it’s critical to have your totem. Take a close look at the facts of the file presented to you. Please note the flag is marked with an “RITSEC=” rather than {} due to encoding limitations.


<br />
### Solution
Dream 4からStegoカテゴリになっています。

<br />

stringsをやると、URLエンコードされている部分が見つかるのでデコードするとモールス信号が出てきます。

モールス信号は以下でデコードしました。

https://morsecode.world/international/translator.html

結果：

<pre>
DREAMSFEELREALWHENWE'REINTHEM.IT'SONLYWHENWEWAKEUPTHATWEREALIZESOMETHINGWASACTUALLYSTRANGE.RITSEC=DIVERSION
</pre>

<br />

Flag: `RITSEC{DIVERSION}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
