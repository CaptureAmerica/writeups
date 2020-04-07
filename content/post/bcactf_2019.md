---
title: "BCACTF Writeup"
date: 2019-06-16T13:00:00+09:00
lastmod: 2019-09-23T09:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/bcactf_2019/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fbcactf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

(2019/09/23 - 復習しました)

URL: [https://www.bcactf.com/](https://www.bcactf.com/)
<br /><br />
前回のHSCTFに続き、これも学生さん用のCTFでした。
<br /><br />
だいたい解けたけど、解けなかったやつもありました。
<br /><br />
あまりに簡単だったやつは、省略してます。<br />
<img src="https://captureamerica.github.io/writeups/img/bcactf_Score.png" alt="bcactf_Score.png">

<br />
<pre>
├── 0: welcome
│   ├── hello-world (50)
│   ├── net-cat (50)
|   └── wuphf (50)
├── binary-exploitation
│   ├── executable (150)
|   └── executable-2 (250)
├── crypto
│   ├── basic-numbers (50)
│   ├── cracking-the-cipher (50)
│   ├── three-step-program (125)
│   ├── tupperware (175)            <=== gave up
│   ├── runescape (180)
│   ├── a-major-problem (200)
|   └── runescape-2 (250)           <=== gave up
├── forensics
│   ├── split-the-red-sea (100)
│   ├── bca-craft (125)
│   ├── file-head (125)
│   ├── of-course-rachel (150)
│   ├── open-docs (150)
│   ├── study-of-roofs (150)
│   ├── wavey (150)                 <=== gave up (reviewed)
│   ├── corrupt-psd (200)
│   ├── large-data (200)            <=== gave up
│   ├── the-flag-is (200)
|   └── one-punch-zip (250)         <=== gave up (reviewed)
├── programming
│   ├── 1+1=window (75)
│   ├── bca-store (75)
│   ├── instructions (175)
│   ├── public-library (200)
|   └── manner-of-thpeaking (250)
├── quest
│   ├── free-real-estate (-1)       <=== gave up
│   ├── copypasta (0)
│   ├── you-wanted-it (50)
│   ├── for-the-night-is-dark-3 (75)
│   ├── for-the-night-is-dark-1 (150)
|   └── for-the-night-is-dark-2 (150)
├── reversing
│   ├── large-pass (100)
│   ├── basic-pass-1 (150)
│   ├── scratch-that (150)
│   ├── another-pass (200)
│   ├── basic-pass-2 (200)
│   ├── basic-pass-3 (200)
|   └── compression (200)
└── web
    ├── the-inspector (50)
    ├── wite-out (50)
    ├── dig-dug (100)
    └── cookie-clicker (150)
</pre>

<br /><br />
<br /><br />
## [Binary]: executable (150)
- - -
### Challenge
> It's in there somewhere. Good luck!

Attachment:

- executable-mac
- executable-ubuntu

### Solution
Ghidraでデコンパイルしてコードを確認してみます。以下、抜粋です。
```C
  local_18 = *(long *)___stack_chk_guard;
  _puts("Welcome to the lottery!");
  _puts("So now we\'re going to pick a ginormous number!");
  _puts("If it\'s 1, you win!");
  uVar1 = _rand();
  _printf("Your number is %d!\n",(ulong)uVar1);
  if (uVar1 == 1) {
    _puts("Congratulations, you\'re our lucky winner!");
    auVar2 = pinsrb(ZEXT416(0x3e),0xe4,1);
    auVar7 = pinsrb(auVar2,0x2b,2);
    :
```

if()文判定を一個回避するだけで行けそうです。

アプローチとしては、以下が考えられます。

1. rand()の結果が入っているuVar1を1で上書きする
2. ZFをいじって逆に飛ばす

<pre>
=> 0x00000000004006a4 <main+126>:       83 fb 01        cmp    ebx,0x1
(gdb) p $ebx
$1 = 1804289383
(gdb) set $ebx=1
(gdb) p $ebx
$2 = 1
</pre>

出力結果はBrainF*ckで、以下で実行しました。<br />
[https://copy.sh/brainfuck/](https://copy.sh/brainfuck/)

`bcactf{3x3cut4bl3s_r_fun_124jher089245}`



<br /><br />
<br /><br />
## [Binary]: executable-2 (250)
- - -
### Challenge
> It's in here somewhere. Good luck... again.
<br /><br />
(Now you actually have to try.)

Attachment:

- executable-ubuntu

### Solution
問題文の、"(Now you actually have to try.)" の意味がよくわかりませんでした。わたしのやり方は、想定解じゃないのかも知れません。トライしてないし。。^^;

Ghidraでデコンパイルしてコードを見たところ、rand()をコールしている部分が増えていましたが、前の問題と大して違いはなかったです。

今度は、ZFをいじってやりました。0x206 -> 0x246
<pre>
B+>│0x400694 \<main+110>     jne    0x400744 \<main+286>  
(gdb) set $eflags = 0x246
</pre>

同様に出力結果はBrainF*ckで、以下で実行しました。<br />
[https://copy.sh/brainfuck/](https://copy.sh/brainfuck/)

rsqsjv{Arent_executables_fun?_I_think_so_sdkfjhqiweuryiquwerajzncxbvaihqiwueyr}

何パターンか試して、結局、全体をRot10したのがフラグでした。<br />
`bcactf{Kboxd_ohomedklvoc_pex?_S_drsxu_cy_cnuptrasgoebisaegobktjxmhlfksrasgeoib}`





<br /><br />
<br /><br />
## [Crypto]: three-step-program (125)
- - -
### Challenge
> We found this strange file with a bunch of stuff in it... Can you help us decode it?
<br /><br />
> Hint: Looks like you need to do different things to each section...

<pre>
MzIgLSAgfDMgVGltZXMgQSBDaGFybXwgLSAzMg==

JJGTEVSLKNBVISSGINCU2VCTGJFVETCWKNGVGTKLKJEEKQ2VJNEUSNC2KZKVCS2OJFNE4RKPKNFUUSKSJNKTITSDKJFEERKUI5GTETKJLJGVMQ2RJNLEWUSLIZAVES2DJRFE2RKDK5JU2SKKJBCTEVKLJBDUSWSUI5KTETSLKZEVKS2TLJKEWUSFIU2FKU2WJRBEIVCFKFJVASKWIFKU2USLIRDUUR2FGJJEWQ2LKJGFMR2TJNCUYSSIIRFU2U2UJFCTEVKJKZJUMSKKJNKU6VK2KRFVES2VGZKEWUSKIJCVIR2XKNBEUNKGIZDVMMSEJRFEERKDKRJVOR2SJJKUGV2TJVFDKR2VGRLVGSKLJUZEKSKWJNHEWWSKKVDVCSSUJFJEERJUK5JVKTCCIZKEKVCDIVFFUQKWKFITEQSJJZEVKV2SGJDEYQSCKVBVMSSTJFFEMRSFKMZEISKFLJCVSTKTIZEUUTCGJ5JVUV2KJJAVKNSVKNMUWTSBKZKU2MSUJJLEYRCFKEZEETCKJNDECVCSKZFU4QSVI5ITEU2LJZCEMU2VJNDEYRSOIVKVCS2OJRFE4RKPKFNFIS2SINCTEUSTKZGEERCVKNJEGRKKGVDEISKXINBEOVSDIVGVES2DJM2UIVKXKNFUKSS2I5LE2VSLLBGEKWSVJFJFGUCLLJHEKQ2QJI2UQVJWKE6T2PJ5

lhlm oad lamaew eyhmgs. lg i sxsro rgu ntee qhj a qesg? dbfcp rgu stne xtve tm lhtl xac, b’dl rh wadr gn jhm ayw zayw at zowr. 
mvscey{bu57_j0n_o4i7_kgbhmffhlqe} bfm, te htjnpw, feim lixx at hhf’t mx ko dbepwx…
</pre>

<br />
### Solution
1行目をBase64 decodeすると、`32 -  |3 Times A Charm| - 32~` が取れます。
<br /><br /><br />
これをヒントに、2つ目はBase32 decode x 3です。以下を使わせてもらいました。<br />
[https://emn178.github.io/online-tools/base32_decode.html](https://emn178.github.io/online-tools/base32_decode.html)

<pre>
Why english so ard to tok.
No speak more English. 
Ail gi you tu hints to read my encrypted languich. 

1. SALT iz key to gret food!
2. Le francais crypte le meilleur
</pre>

<br />
最後の行はフランス語っぽくて、「French crypt the best」ということらしいです。Vigenere cipherですね。

Keyを"SALT"にして、Vigenere cipherをデコードすると、フラグが取れます。<br />
[https://cryptii.com/pipes/vigenere-cipher](https://cryptii.com/pipes/vigenere-cipher)<br />
"that was simple enough. so i heard you came for a flag? since you have made it this far, i’ll go easy on you and hand it over. `bcactf{ju57_y0u_w4i7_znjhbmnhaxm}` but, be warned, next time it won’t be so simple"





<br /><br />
<br /><br />
## [Crypto]: tupperware (175)
- - -
### Challenge
> Took my lunch to school in a Tupperware (now with patented TupperSRF™ plastic!) and part of it got stained with a flag. k tells you where.
<br /><br />
NOTE: number names come from the Googology wiki, some numbef names may be inconsistent.

Attachment:

- k （以下は中身の抜粋です。）
<br />
Four octogintacentillion, eight hundred and fifty-eight novemseptuagintacentillion, four hundred and eighty-seven octoseptuagintacentillion, ...(snip)..., seven hundred and eighty-five quadrillion, four hundred and thirty-three trillion, one hundred and five billion, ninety-one million, five hundred and twenty-six thousand, six hundred and thirty-nine.

### Unsolved
以下のような感じでPythonでひらすらreplaceを行って、全部数字にするところまではやりました。
```python
k = k.replace("octogintacentillion", ")*10^543")
k = k.replace("novemseptuagintacentillion", ")*10^540")
:
```

スペリングが異なる単語が結構あるし、かなり面倒だったけど、これだけできればクリアだろうと思って頑張りました。
<pre>
(4 )*10^543+ ( 8 * 100+ 50+8 )*10^540+ ( 4 * 100+ 80+7 )*10^537+ ( 7 * 100+ 3 )*10^534+ ( 2 * 100+ 17 )*10^528+ ( 6 * 100+ 50+4 )*10^525+ ( 1 * 100+ 60+8 )*10^522+ ( 5 * 100+ 7 )*10^519+ ( 3 * 100+ 70+7 )*10^516+ ( 1 * 100+ 5 )*10^513+ ( 6 * 100+ 30+4 )*10^510+ ( 2 )*10^507+ ( 6 * 100+ 40+7 )*10^504+ ( 7 * 100+ 30+1 )*10^501+ ( 1 * 100+ 20+8 )*10^498+ ( 4 * 100+ 90+9 )*10^495+ ( 3 * 100+ 7 )*10^492+ ( 2 * 100+ 40+4 )*10^489+ ( 8 * 100+ 50+1 )*10^486+ ( 4 * 100+ 40+1 )*10^483+ ( 90)*10^480+ ( 3 * 100+ 50+7 )*10^477+ ( 8 * 100+ 60+5 )*10^474+ ( 7 * 100+ 11 )*10^471+ ( 30+3 )*10^471+ ( 4 * 100+ 40+3 )*10^468+ ( 4 * 100+ 19 )*10^465+ ( 7 * 100+ 90+3 )*10^462+ ( 5 * 100+ 30+1 )*10^459+ ( 9 * 100+ 90+9 )*10^456+ ( 8 * 100+ 80+3 )*10^453+ ( 2 * 100+ 60+1 )*10^450+ ( 5 * 100+ 70+9 )*10^447+ ( 80+6 )*10^444+ ( 7 * 100+ 3 )*10^441+ ( 2 * 100+ 80+5 )*10^438+ ( 9 * 100+ 80+8 )*10^435+ ( 5 * 100+ 50)*10^432+ ( 3 * 100+ 20+5 )*10^429+ ( 9 * 100+ 70)*10^426+ ( 5 * 100+ 12 )*10^423+ ( 3 * 100+ 40+3 )*10^420+ ( 60+3 )*10^417+ ( 3 * 100+ 12 )*10^414+ ( 9 * 100+ 60+5 )*10^411+ ( 3 * 100+ 20+7 )*10^408+ ( 5 * 100+ 80)*10^405+ ( 9 * 100+ 70+8 )*10^402+ ( 2 * 100+ 40+1 )*10^399+ ( 8 * 100+ 40)*10^396+ ( 4 * 100+ 50+8 )*10^393+ ( 7 * 100+ 5 )*10^390+ ( 7 * 100+ 70+8 )*10^387+ ( 5 * 100+ 80+2 )*10^384+ ( 5 * 100+ 4 )*10^381+ ( 6 * 100+ 7 )*10^378+ ( 6 * 100+ 50+5 )*10^375+ ( 8 * 100+ 70+9 )*10^372+ ( 1 * 100+ 50+1 )*10^369+ ( 8 * 100+ 20+8 )*10^366+ ( 1 * 100+ 40+9 )*10^363+ ( 6 * 100+ 90+4 )*10^360+ ( 5 * 100+ 80+9 )*10^357+ ( 8 * 100+ 60+7 )*10^354+ ( 1 * 100+ 90+8 )*10^351+ ( 8 * 100+ 40)*10^348+ ( 9 * 100+ 50+9 )*10^345+ ( 5 * 100+ 90+8 )*10^342+ ( 4 * 100+ 90+7 )*10^339+ ( 5 * 100+ 90+7 )*10^336+ ( 9 * 100+ 80+9 )*10^333+ ( 8 * 100+ 60+6 )*10^330+ ( 80+4 )*10^327+ ( 6 * 100+ 90+4 )*10^324+ ( 5 * 100+ 60)*10^321+ ( 4 * 100+ 90)*10^318+ ( 6 * 100+ 70+5 )*10^315+ ( 8 * 100+ 1 )*10^312+ ( 7 * 100+ 80+6 )*10^309+ ( 3 * 100+ 40+7 )*10^306+ ( 7 * 100+ 30)*10^303+ ( 8 * 100+ 50+1 )*10^300+ ( 4 * 100+ 30+8 )*10^297+ ( 9 * 100+ 30+1 )*10^294+ ( 2 * 100+ 70+5 )*10^291+ ( 5 * 100+ 80+4 )*10^288+ ( 9 * 100+ 8 )*10^285+ ( 1 * 100+ 30+8 )*10^287+ ( 1 * 100+ 90)*10^279+ ( 4 * 100+ 9 )*10^276+ ( 4 * 100+ 80+1 )*10^273+ ( 8 * 100+ 80)*10^270+ ( 9 * 100+ 11 )*10^267+ ( 3 * 100+ 19 )*10^264+ ( 4 * 100+ 90+1 )*10^261+ ( 9 * 100+ 70+8 )*10^258+ ( 6 * 100+ 40+4 )*10^255+ ( 8 * 100+ 8 )*10^252+ ( 30+6 )*10^249+ ( 8 * 100+ 17 )*10^246+ ( 8 * 100+ 70+3 )*10^243+ ( 5 * 100+ 14 )*10^240+ ( 50+6 )*10^237+ ( 5 * 100+ 90)*10^234+ ( 1 * 100+ 30+5 )*10^231+ ( 3 * 100+ 30+1 )*10^228+ ( 5 * 100+ 70+8 )*10^225+ ( 1 * 100+ 5 )*10^222+ ( 8 * 100+ 60+2 )*10^219+ ( 8 * 100+ 20+1 )*10^216+ ( 4 * 100+ 50+4 )*10^213+ ( 4 * 100+ 50+4 )*10^210+ ( 6 * 100+ 17 )*10^207+ ( 3 * 100+ 70+5 )*10^204+ ( 9 * 100+ 19 )*10^201+ ( 8 * 100+ 70)*10^198+ ( 2 * 100+ 40+5 )*10^195+ ( 3 * 100+ 60+3 )*10^192+ ( 4 * 100+ 40)*10^189+ ( 60+6 )*10^186+ ( 3 * 100+ 70+2 )*10^183+ ( 7 * 100 )*10^180+ ( 50+3 )*10^177+ ( 2 * 100+ 60+3 )*10^174+ ( 3 * 100+ 60+2 )*10^171+ ( 8 * 100+ 60+3 )*10^168+ ( 6 * 100+ 90+4 )*10^165+ ( 1 * 100+ 20+9 )*10^162+ ( 1 * 100+ 30+3 )*10^159+ ( 7 * 100+ 60+5 )*10^156+ ( 4 * 100+ 40+1 )*10^153+ ( 9 * 100+ 20+6 )*10^150+ ( 5 * 100+ 30+3 )*10^147+ ( 6 * 100+ 9 )*10^144+ ( 5 * 100+ 3 )*10^141+ ( 9 * 100+ 90+4 )*10^138+ ( 18  )*10^135+ ( 5 * 100+ 60+5 )*10^132+ ( 3 * 100+ 19 )*10^129+ ( 3 * 100+ 80+2 )*10^126+ ( 9 * 100+ 13 )*10^123+ ( 4 * 100+ 50+4 )*10^120+ ( 1 * 100+ 60+1 )*10^117+ ( 4 * 100+ 20+9 )*10^114+ ( 50+8 )*10^111+ ( 5 * 100+ 5 )*10^108+ ( 6 * 100+ 50+5 )*10^105+ ( 3 * 100+ 50)*10^102+ ( 7 * 100+ 80)*10^99+ ( 9 * 100+ 11 )*10^96+ ( 2 * 100+ 20+9 )*10^93+ ( 2 * 100+ 3 )*10^90+ ( 4 * 100+ 30+2 )*10^87+ ( 3 * 100+ 90+2 )*10^84+ ( 9 * 100+ 60+7 )*10^81+ ( 2 * 100+ 30+6 )*10^78+ ( 1 * 100+ 30+7 )*10^75+ ( 6 * 100+ 40+7 )*10^72+ ( 7 * 100+ 80)*10^69+ ( 3 * 100+ 8 )*10^66+ ( 9 * 100+ 60+6 )*10^63+ ( 9 * 100+ 30+6 )*10^60+ ( 6 * 100+ 90+1 )*10^57+ ( 4 * 100+ 30+9 )*10^54+ ( 6 * 100+ 30+6 )*10^51+ ( 9 * 100+ 80)*10^48+ ( 50+8 )*10^45+ ( 2 * 100+ 60+6 )*10^42+ ( 6 * 100+ 90+3 )*10^39+ ( 4 * 100+ 90+8 )*10^36+ ( 5 * 100+ 60+8 )*10^33+ ( 2 * 100+ 30+5 )*10^30+ ( 4 * 100+ 60+4 )*10^27+ ( 3 * 100+ 10 )*10^24+ ( 9 * 100+ 7 )*10^21+ ( 6 * 100+ 20+8 )*10^18+ ( 7 * 100+ 80+5 )*10^15+ ( 4 * 100+ 30+3 )*10^12+ ( 1 * 100+ 5 )*10^9+ ( 90+1 )*10^6+ ( 5 * 100+ 20+6 )*10^3+ ( 6 * 100+ 30+9)
</pre>

以下で、上記の式は計算できました。<br />
[https://defuse.ca/big-number-calculator.htm](https://defuse.ca/big-number-calculator.htm)
<br />

4858487703000217654168507377105634002647731128499307244851441090357865744443419793531999883261579086703285988550325970512343063312965327580978241840458705778582504607655879151828149694589867198840959598497597989866084694560490675801786347730851438931275598708000190409481880911319491978644808036817873514056590135331578105862821454454617375919870245363440066372700053263362863694129133765441926533609503994018565319382913454161429058505655350780911229203432392967236137647780308966936691439636980058266693498568235464310907628785433105091526639

<br />
で？？



<br /><br />
<br /><br />
## [Crypto]: runescape
- - -
### Challenge
> ᚣᚦᚧᚦᚭᚠ{ᚠᚯᚲᚴᚪᚲᚫᚦᚹᚧᚫᚧᚵᚹᚨᚩᚨᚩᚨᚮᚲᚯᚹᚥᚯᚲᚧᚭᚷᚶᚲᚫᚨᚸᚵᚮᚩᚫᚥᚧᚫᚫᚸᚹᚩᚫᚥᚢᚸᚫᚸᚧᚵᚤᚶᚧᚣᚲᚭᚩᚦᚨᚪᚣᚨᚭᚩᚭᚪᚭᚩᚸᚫᚦᚩᚤᚶᚲᚯᚨ_ᚧᚣᚦᚬᚲᚠᚥᚶᚩᚱᚳᚵᚢᚫᚸᚤᚴᚯᚨᚭᚪᚮᚷᚡᚹᚰ}

### Solution
ルーン文字です。<br />

オンラインでさくっと変換できるのかと思ったら、アスキー文字と1対1で対応しているわけではなくて、単一換字式暗号として解くやつでした。

最初の6文字は、bcactfなのがわかっているので、まずわかっているところから変換します。

すると、アンダースコア('_')より後ろの26文字がアルファベット順に並んでいるのが予想できます。
<pre>
e19aa3 => b
e19aa6 => c
e19aa7 => a
e19aa6 => c
e19aad => t
e19aa0 => f
7b
:
:
5f
e19aa7 => a
e19aa3 => b
e19aa6 => c
e19aac
e19ab2
e19aa0 => f
e19aa5
e19ab6
e19aa9
e19ab1
e19ab3
e19ab5
e19aa2
e19aab
e19ab8
e19aa4
e19ab4
e19aaf
e19aa8
e19aad => t
e19aaa
e19aae
e19ab7
e19aa1
e19ab9
e19ab0
</pre>

これで、全てのアルファベットの変換ができるので、フラグがわかります。
`bcactf{frequencyanalysisisverygreatwhensolvingannoyingmonoalphabeticsubstitutionciphers_abcdefghijklmnopqrstuvwxyz}`

ちなみに、読みやすくスペースを入れると、<br />
"frequency analysis is very great when solving annoying monoalphabetic substitution ciphers"





<br /><br />
<br /><br />
## [Crypto]: a-major-problem (200)
- - -
### Challenge
> A mysterious figure named Major Mnemonic has sent you the following set of words. Figure out what they mean!
<br /><br />
"Pave Pop Poke Pop Dutch Dozen Denim Deism Loot Thatch Pal Atheism Rough Ditch Tonal"
<br /><br />
Hint: The words translate to numbers, which then translate to the flag.

### Solution
以下のWikiに従って、それぞれの数字を求めて、文字に変換するだけです。<br />
[https://en.wikipedia.org/wiki/Mnemonic_major_system](https://en.wikipedia.org/wiki/Mnemonic_major_system)

| 単語      | 数字          | 文字    |
| :------------- |:---------------:|:------------:|
| Pave | 98 | b |
| Pop  | 99 | c |
| Poke | 97 | a |
| Pop   | 99 | c |
| Dutch  | 16 (?) | t |
| Dozen  | 102 | f |
| Denim  | 123 | { |
| Deism  | 103 | g |
| Loot   | 51 | 3 |
| Thatch | 16 (?) | t |
| Pal    | 95 | _ |
| Atheism | 103 | g |
| Rough  | 48 | 0 |
| Ditch  | 16 (?) | t |
| Tonal  | 125 | } |

ところどころ16が出てきちゃったけど、フラグフォーマットより"t" (bcac<font color="red">t</font>f) に置き換えられるのがわかるので、フラグは`bcactf{g3t_g0t}`


<br /><br />
<br /><br />
## [Forensics]: of-course-rachel (150)
- - -
### Challenge
> Ugh, I had a really important file with the flag, but sadly it broke. My friend Rachel said that snapshots are good for backing up, and luckily I listened so here is my screenshot. Do you think you could help me put it back together?
<br /><br />
Hint: that's a lot to type in...

Attachment:

- snapshot.zip (part1.png, part2.png, part3.png, part4.png, part5.png)

### Solution
ヒント`that's a lot to type in...`があんまりヒントになってない気がします^^; <br />
16進数が書かれた画像ファイルがあるので、OCRで読み取って変換するだけです。
<br /><br />
オンラインのをいくつか試しましたが、読み取り精度がイマイチですね。
タダのものに文句は言えないですが、「0」(ゼロ)が「O」(オー)になるのはともかく、「B」が「13」になったり、「D」が「1)」になったりしました。（気持ちはわからんでもないけど）
<br /><br />
わたしは以下を使いましたけど、オススメするわけではないです。結構、手直ししました。
<br />
[https://ocr.space/](https://ocr.space/)
<br /><br />
デコードしたところ、Pythonコードが出てきたので、以下のようにファイルに保存してpython3で実行したらフラグが取れました。
<pre>
$ cat part?.txt | tr -d "\n" | tr -d " " | unhex > rachel.py
$ python3 rachel.py 
bcactf{0p71c4lly_r3c0gn1z3d_ch4r4c73rs}
</pre>


<br /><br />
<br /><br />
## [Forensics]: the-flag-is (200)
- - -
### Challenge
> I have a flag! The flag is... wait... did my PDF editor not save the flag? OH NO! I remember typing it in, can you help me find it?
<br /><br />
Hint: There are two ways to view PDFs. One way is to view it with a PDF viewer. How else can you view a file?

### Solution
テキストエディタで開くと以下が見つかるので、ascii85 decodeをするだけです。
<pre>
5 0 obj << /Length 89 /Filter /ASCII85Decode >>
stream
6<#'\7PQ#?2BYt2+>GQ(+?(u.+B2ko-t6[p@ru=0A2%m[?SlCOFC-k60Qf<]0lB@!1JMFu2`,>XF\lU*2`#N'.3MT)+@T6~>
endstream
endobj
</pre>
以下を使いました。<br />
[https://www.tools4noobs.com/online_tools/ascii85_decode/](https://www.tools4noobs.com/online_tools/ascii85_decode/)
<br /><br />
結果：BT /F1 16 Tf 100 700 Td (`bcactf{d0n7_4g3t_4b0u7_1nCr3Men74l_uPd473s}`) Tj ET



<br /><br />
<br /><br />
## [Programming]: 1+1=window (75)
- - -
### Challenge
> hex+hex=hex

Attachment:

- one.txt
- two.txt

### Solution
```python
#!/usr/bin/env python3

for one in open("one.txt", "r"):
    one_hex = one.split()
    
for two in open("two.txt", "r"):
    two_hex = two.split()

list = [int(i,16) + int(j,16) for (i, j) in zip( one_hex, two_hex)]
for c in list:
    print(chr(c),end="")
print("")
```
`bcactf{1_h0p3_y0u_us3_pyth0n}`



<br /><br />
<br /><br />
## [Programming]: bca-store (75)
- - -
### Challenge
> You are a cashier for a small store that sells a few items. Coming up is the annual sale, and you really don't want to do that much math. So, being you, you decide to automate it.
<br /><br />
Items:
<br />
    A: $45, no sale<br />
    B: $52, buy one get one 10% off<br />
    C: $67, buy one get one half off<br />
    D: $75, buy two get one free<br />
<br />
Input:
<br />
    What the customer ordered, separated by spaces.<br />
    The amount the customer paid.<br />
<br />
For example, C B B C A 250.
<br /><br />
The input file is attached. If we had the above twice, then you can use the following to test on your local machine:
<br /><br />
    input.txt:
<br />
C B B C A 250<br />
D A B B 250<br />
D D D D D A B 390<br />
<br />
`$ cat input.txt | ./script.py`
<br /><br />
Your program should print the customer's change (i.e. return to stdout), if applicable, or -1 (the number) if they don't have enough money. The above example should result in:
<br /><br />
5.70<br />
31.20<br />
-1<br />
<br />
Round to the nearest hundredths place.<br />
<br />
You should submit your answer as one string, with each input line's result separated by a space.<br />
<br />
For example, the above output should be formatted as follows to be accepted:<br />
`5.70 31.20 -1`


Attachment:

- input.txt

### Solution
例が、`$ cat input.txt | ./script.py` となってて、なんとなくpythonで解くことを期待されているようだったので、勉強も兼ねてpythonでやりました。
```python
#!/usr/bin/env python3
import sys
import re

def calc_B(num):
    if num >= 2:
        return (52 + 52 * 0.9) * int( num / 2 ) + 52 * (num % 2)
    else:
        return 52 * num

def calc_C(num):
    if num >= 2:
        return (67 + 67 * 0.5) * int( num / 2 ) + 67 * (num % 2)
    else:
        return 67 * num

def calc_D(num):
    if num >= 3:
        return (75 * 2) * int( num / 3 ) + 75 * (num % 3)
    else:
        return 75 * (num % 3)


for line in sys.stdin.readlines():
    numA = 0
    numB = 0
    numC = 0
    numD = 0
    str_digit = re.findall(r'(\d+|\D+)', line)
    items = str_digit[0].split()
    paid = int(str_digit[1])

    for item in items:
        if item == 'A':
            numA += 1
        if item == 'B':
            numB += 1
        if item == 'C':
            numC += 1
        if item == 'D':
            numD += 1
    total = 45 * numA + calc_B(numB) + calc_C(numC) + calc_D(numD)
    # == debug ==
    # print("paid = {}, total = {}, A = {}, B = {}, C = {}, D = {}".format(paid, total, numA, numB, numC, numD))
    # print("paid = {}, total = {}, A = {}, B = {}, C = {}, D = {}".format(paid, total, 45 * numA, calc_B(numB), calc_C(numC), calc_D(numD)))
    # continue

    if paid >= total:
        print("{:.2f}".format(paid - total), end="")
    else:
        print("-1", end="")
    print(" ", end="")
print("")
```
`5.70 -1 1.00 75.00 -1 77.40 -1`


<br /><br />
<br /><br />
## [Programming]: instructions (175)
- - -
### Challenge
> We intercepted a message between two agents from a terrorist group known as 0x4556494c. We think it might contain some useful information, so we'd like you to crack it. Here is the message.
<br /><br />
BEGIN TRANSMITION<br />
Dear Agent Reffef,<br />
<br />
I have attached the super secret plans for operation 0x576f726b206f6e207468652070757a7a6c652c2073746f702072656164696e672068657821.<br />
You will need to decode it first though.<br />
<br />
The rules are simple:<br />
<br />
A line is "viable" if the length of a line is divisible by 3, and the line does not contain the \`&\` character. <br />
<br />
For every viable line, you will grab the \`n\`th character, <br />
where \`n\` is the corresponding number at the top of the file (Counting from one!) <br />
The first viable line will use the first number, etc.<br />
<br />
Put all the letters together to find the answer!  <br />
<br />
- Agent Doposi<br />
END TRANSMITION


Attachment:

- flag.txt


### Solution
```c
#include <stdio.h>
#include <string.h>

#define LINE_LEN     1000

int pos[] = {20,30,8,14,17,24,44,19,17,29,20,34,35,27,42,34,7,25,7,21,8,38,13,25,14,13,42,14,20,23,3,27,38,9,18,41,3,11,35, 0};
char tmp_buff[LINE_LEN];

int main(int argc, char **argv)
{
	FILE *fp;
	int i = 0;
	char *find;
	char c;
	
	if ( ( fp = fopen( "flag.txt", "r" ) ) == NULL ) {
		return -1;
	}
	
	while (1) {
		if ( fgets( tmp_buff, LINE_LEN, fp ) == NULL ) {
			break;
		}
		find = strstr( tmp_buff, "&" );
		if ( find != NULL ) {
			continue;
		}

		if ( ( strlen( tmp_buff ) - strlen( "\n" ) ) % 3 == 0 ) {
			*( tmp_buff + pos[i] ) = '\0';
			printf( "%s", tmp_buff + pos[i] - 1 );
			i++;
			// To prevent seg fault
			if ( pos[i] == 0 ) {
				break;
			}
		}
	}
	puts( "" );
	fclose( fp );
}
```
`bcactf{f0110w_tH3_r00lz_<3_l0ve_m3_pls}`


<br /><br />
<br /><br />
## [Programming]: manner-of-thpeaking (250)
- - -
### Challenge
> Tho, I came Acroth thith therieth of inthturcthins, and thomething that thaid "the key ith the attached litht of ATHCII printableth." Tho anywayth, here'th the inthtructhinth.
<br /><br />
Hint1: Pardon my LITHP
<br />
Hint2: Backthaltheth in printableth.txt are ethcape charachterth


Attachment:

- printableth.txt
- inthtructhins.txt

<br />
以下がprintableth.txtの中身。（特殊文字だらけで表示が変になりそうなので、スクリーンショットにしてます。）<br />
<img src="https://captureamerica.github.io/writeups/img/printableth.png" alt="printableth.png">

<br />
以下がinthtructhins.txtの中身。<br />
cadadddddr, caddadddddr, caadddddr, caddadddddr, cadddddddddddddddddddadddddr, cadddddadddddr, caaddddddr, cadddddddddddadddr, cadadr, cadddddadr, cadddddddadr, caddddaddddr, caddddddddadr, caddddadr, cadddddadr, cadddadr, cadddadddddr, caddddaddddr, cadddddddddddddddadddddr, cadddddddddddddddddadddr, caadr, caddddddadddddr, cadddddddddddddddddadddr, caddddadr, caddddddddddddadddr, caddddddddddddadddddr, cadadr, cadddddddddddddadddddr, caddddddadddr, caddddaddddr, cadadr, cadddddadr, caddddaddddr, caddddadr, caddddddddddddddddddddddadddddr, cadddadr, caddddddddddddddddddadddr, caadr, caddddddddddddadddr, caddddadddddr, cadar, caddaddddddr

### Solution
[ポイント１]<br />
コンマ(,)で区切られた塊が一文字だと考えると、見えてきます。<br />

b : ca<font color="red">d</font>a<font color="orange">ddddd</font>r<br />
c : ca<font color="red">dd</font>a<font color="orange">ddddd</font>r<br />
a : caa<font color="red">ddddd</font>r<br />
c : ca<font color="red">dd</font>a<font color="orange">ddddd</font>r<br />
t : ca<font color="red">ddddddddddddddddddd</font>a<font color="orange">ddddd</font>r<br />
f : ca<font color="red">ddddd</font>a<font color="orange">ddddd</font>r<br />
{ : caa<font color="orange">dddddd</font>r<br />

<br />
[ポイント2]<br />
printableth.txtの中身は、単純にASCII文字の並びなのに、ところどころ()でくくられてグループ化されています。
<br /><br />
inthtructhins.txtに出てくる文字列の、2個目のaとrの間のdの数（<font color="orange">オレンジ</font>部分）、例えばbで言うとd=5ですけど、これは5番目の()のグループを見る、ということです。<br />
1個目のaと2個目のaの間のdの数（<font color="red">レッド</font>部分）は、グループの中の何番目の文字を見るか、ですね。
```c
#include <stdio.h>
#include <string.h>

#define ON  1
#define OFF 0

char printable[][30] ={" !\"#$%&'()*+,-./", "0123456789", ":;<=>?@", "ABCDEFGHIJKLMNOPQRSTUVWXYZ", "[\\]^_`", "abcdefghijklmnopqrstuvwxyz", "{|}~"};
char inthtructhins[][40] = {"cadadddddr", "caddadddddr", "caadddddr", "caddadddddr", "cadddddddddddddddddddadddddr", "cadddddadddddr", "caaddddddr", "cadddddddddddadddr", "cadadr", "cadddddadr", "cadddddddadr", "caddddaddddr", "caddddddddadr", "caddddadr", "cadddddadr", "cadddadr", "cadddadddddr", "caddddaddddr", "cadddddddddddddddadddddr", "cadddddddddddddddddadddr", "caadr", "caddddddadddddr", "cadddddddddddddddddadddr", "caddddadr", "caddddddddddddadddr", "caddddddddddddadddddr", "cadadr", "cadddddddddddddadddddr", "caddddddadddr", "caddddaddddr", "cadadr", "cadddddadr", "caddddaddddr", "caddddadr", "caddddddddddddddddddddddadddddr", "cadddadr", "caddddddddddddddddddadddr", "caadr", "caddddddddddddadddr", "caddddadddddr", "cadar", "caddaddddddr", "eof"};

int main(int argc, char **argv)
{
	int op;
	int flag_r, flag_a;
	int i, a, d, pos1, pos2;

	for ( op = 0 ; ; op++ ) {
		if ( strcmp( &inthtructhins[op][0], "eof" ) == 0 ) {
			break;
		}
		flag_r = OFF;
		flag_a = OFF;
		for ( i = 0, a = 0, d = 0 ; flag_r == OFF ; i++ ) {
			switch ( inthtructhins[op][i] ) {
			case 'r':
				flag_r = ON;
				pos1 = d;
				break;
			case 'a':
				if ( a == 1 ) {
					pos2 = d;
					d = 0;
				}
				a++;
				break;
			case 'd':
				d++;
				break;
			default:
				break;
			}
		}
		printf( "%c", printable[pos1][pos2] );
	}
	puts( "" );
	return 0;
}
```
`bcactf{L157_8453d_pR0gR4Mm1nG_15_4w3S0Me!}`


<br /><br />
<br /><br />
## [Quest]: for-the-night-is-dark-1 (150)
- - -
### Challenge
> Hello, traveler. Welcome to your quest. You must walk the Red Lord's shining path, guided by his shining stars. Here is a picture of those stars. A map if you will. May the Lord of Light give you wisdom.
<br /><br />
NOTE: As more heroes complete each stage of the quest, fewer points will be available to future teams.
<br /><br />
Hint1: One per row, top to bottom.
<br /><br />
Hint2: Maybe the brightness of the stars mean something?


Attachment:

- starmap.bmp


### Solution
「青い空を見上げればいつもそこに白い猫」で開くと、赤色のところだけ星みたいなのが出てきました。

赤色 ビット1 <br />
<img src="https://captureamerica.github.io/writeups/img/red_bit1.png" alt="red_bit1.png">

赤色 ビット2 <br />
<img src="https://captureamerica.github.io/writeups/img/red_bit2.png" alt="red_bit2.png">

赤色だけ抽出でビットとして選択して、UTF-8でテキストの取り出しをすると、URLが取れました。<br />
<img src="https://captureamerica.github.io/writeups/img/red_text_utf8.PNG" alt="red_text_utf8.PNG">
<br />
http://rhllor.xyz/7h3fir31n0urh3ar75_d2VsY29tZSB0byBzdGVwIG9uZQ

アクセスするとフラグがもらえます。<br />
`bcactf{gu1d3d_8y_574r5_QmVnaW5uaW5ncw}`



<br /><br />
<br /><br />
## [Quest]: for-the-night-is-dark-2 (150)
- - -
### Challenge
> This task can be found through solving the prior quest tasks.

### Solution
stage2.jsというのがあるので、中身を見てみます。

```javascript
$("#target").submit(function( event ) {
  var hash = md5($("#secret").val())
  if (hash == "3758002ab24653af8d550c0c50473098") {
  	var encode = "ÐßÏ½æ¦ ÐÞÙÖ©Ã»¤× ÃºªîÈ©¼×ÐÖËÕ§£¢Íç«ÖÉÌ±ÈÕÒßÊÕÅ"
	var newstr = ""
	var key = $("#secret").val()
	for (var i = 0; i < encode.length; i++) {
		newstr += String.fromCharCode(encode.charCodeAt(i) - key.charCodeAt(i%key.length))
	}
  	window.location = "/f" + newstr
  }

  $("#secret").val("")
  event.preventDefault();
});
```

md5（3758002ab24653af8d550c0c50473098）の元の文字列は、[rainbow table](https://crackstation.net/)で見つかりました。
<br />
==> darknight
<br />
`bcactf{7h37ru7h15411w3h4v3_dGhlIGxpZ2h0IGluIG91ciBleWVz}`



<br /><br />
<br /><br />
## [Quest]: for-the-night-is-dark-3 (75)
- - -
### Challenge
> Keep on going

### Solution
ウェブアーカイブを見つけるだけです。<br />
[https://web.archive.org/web/20190614021723/http://rhllor.xyz/fl4m30fV3r1745burn5_Z2l2ZSBzdHJlbmd0aCB0byBoaXMgc3dvcmQ](https://web.archive.org/web/20190614021723/http://rhllor.xyz/fl4m30fV3r1745burn5_Z2l2ZSBzdHJlbmd0aCB0byBoaXMgc3dvcmQ)
<br /><br />
`bcactf{p33r1ng_1n70_7h3_p457_Ymxlc3NlZHZpZXc}`



<br /><br />
<br /><br />
## [Reversing]: large-pass (100)
- - -
### Challenge
> You've come across a USB stick in the middle of a parking lot. After taking it home, you plug it into a network-isolated, clean computer, and see a compiled program. Secrets are abound!
<br /><br />
This problem is in a similar vein to basic-pass-2, another-pass, and so on.
<br /><br />
The flag is not in the bcactf{...} format.

Attachment:

- large-linux
- large-mac

### Solution
Ghidraで確認します。
```c
undefined8 main(void)

{
  long in_FS_OFFSET;
  long local_20;
  long local_18;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_18 = 0x4cf7983ffb7741c8;
  __printf_chk(1,"Password: ");
  __isoc99_scanf(&DAT_0010098f,&local_20);
  if (local_18 == local_20) {
    puts("Correct password.");
  }
  else {
    puts("Incorrect Password.");
  }
  if (local_10 == *(long *)(in_FS_OFFSET + 0x28)) {
    return 0;
  }
                    /* WARNING: Subroutine does not return */
  __stack_chk_fail();
}
```
<br />
DAT_0010098fは、%ldになっているので、
<br />
<img src="https://captureamerica.github.io/writeups/img/DAT_0010098f.png" alt="DAT_0010098f.png">


0x4cf7983ffb7741c8を10進数で入れてあげたらよさそうです。

```python
$ python
>>> 0x4cf7983ffb7741c8
5546068866699313608
```

<br />
gdbでも同様の確認はできます。cmpしているところでBreak pointはって、レジスタを確認。
<pre>
B+>│0x5555555547af \<main+303>       cmp    rax,QWORD PTR [rsp+0x8]

(gdb) p $rax
$1 = 5546068866699313608
</pre>


<br />
{{% admonition tip "Tips" %}}
オンラインの16進数->10進数変換ツールだと、違う値（5546068866699313000）になったりします。
{{% /admonition %}}

<br />
Flag: `5546068866699313608`


<br /><br />
<br /><br />
## [Reversing]: scratch-that (150)
- - -
### Challenge
> I made a Guess the Flag game! It's in Scratch, what could be easier? [Click here to access the game.](https://scratch.mit.edu/projects/276674047/)
<br /><br />
Hint1: What does "See inside" do?
<br /><br />
Hint2: There are four scripts: When flag clicked, Reset variables, Check flag, and Generate flag. Which one could possibly contain the flag?

### Solution
scrachのプロジェクトが保存できたので、自分のscrachアカウントを新規作成してプロジェクトを読み込みました。
<br /><br />
あとは、flagの計算をしているところに、以下を追加して実行するだけです。

- show variable flag
- stop all
<br />
<img src="https://captureamerica.github.io/writeups/img/scratch.png" alt="scratch.png">



<br /><br />
<br /><br />
## [Reversing]: another-pass (200)
- - -
### Challenge
> Alright. Your friend John found this cool binary file on the Interwebz. Against all best practices, he downloaded it. Strange, it doesn't appear to be a virus. Because of the password prompt, you feel like it will lead to something important. Figure this one out!
<br /><br />
WARNING: The flag for this problem may not be in the bcactf{...} format.
<br /><br />
The pass that gives "correct" will be the flag.

Attachment:

- another-win.exe
- another-mac
- another-linux

### Solution
Ghidraで確認します。

```c
void _checkInput(char *pcParm1)

{
  ulong uVar1;
  size_t sVar2;
  int local_20;
  char local_19;
  int local_18;
  int local_14;
  char *local_10;
  
  local_14 = 0;
  local_18 = 0;
  local_10 = pcParm1;
  while( true ) {
    uVar1 = SEXT48(local_18);
    sVar2 = _strlen(local_10);
    if (sVar2 <= uVar1) {
      _printf("Incorrect.");
      return;
    }
    local_19 = local_10[(long)local_18];
    _sscanf(&local_19,"%d",&local_20);   //<--- ここら辺がポイント
    local_14 = local_20 + local_14;      //<--- ここら辺がポイント
    if (local_14 == 0x4b) break;
    local_18 = local_18 + 1;
  }
  _printf("Correct.");
  return;
}
```

入力された文字を数字として受け取り、各数字を合計して75（0x4b）になればOKとしてます。

`999999993`にしました。(9 x 8 + 3 = 75)




<br /><br />
<br /><br />
## [Reversing]: basic-pass-3 (200)
- - -
### Challenge
> Ok, the sysadmin finally admits that maybe authentication should happen on a server. Can you just check everything really quick to make sure there aren't any problems now? He put some readouts for people who forget their passwords.
<br /><br />
nc challenges.ctfd.io 30133
<br /><br />
Hint: What happens if you put in parts that you know must be part of the flag? how can you use this to your advantage?

### Solution
ncで繋いで、適当なフラグを入れてみます。
<pre>
$ nc challenges.ctfd.io 30133
welcome to the login portal.
Enter the password.
bcactf
11111100000000000000000000000000000000
Enter the password.
bcactf{aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa}
11111110000000000000000000000000000001
</pre>

合っていると1がつくようです。
<br /><br />
以下のコードを書きました。
```python
#!/usr/bin/env python
from pwn import *

host = "challenges.ctfd.io"
port = 30133
s = remote( host, port )

for c in "abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ_!?":
    payload = ""
    payload += "bcactf{"
    payload += c * 30
    payload += "}"
    print payload
    s.sendlineafter( "Enter the password.\n", payload )
    msg = s.recvuntil("\n")
    print msg
    sleep(1)
```
<br /><br />
結果：
<pre>
$ ./basic_pass_solve.py 
[+] Opening connection to challenges.ctfd.io on port 30133: Done
bcactf{aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa}
11111110000000000000000000000000000001
bcactf{bbbbbbbbbbbbbbbbbbbbbbbbbbbbbb}
11111110000000000000000000000000010001
bcactf{cccccccccccccccccccccccccccccc}
11111110000000000000000000000000000001
:
</pre>

あとは、1が立っているところの文字だけ選ぶだけです。手動でやりました。

`bcactf{y0u_4r3_4_m4573rm1nD!_Ym9vbGlu}`



<br /><br />
<br /><br />
## [Reversing]: compression (200)
- - -
### Challenge
> A stranger on the internet is giving away his passwords. They claim they are encrypted, but you quickly realize that it is only compressed. You have to get hold of their passwords so that you can prove them wrong.
<br /><br />
Hint1: $ file {filename} tells you what type of file something is
<br /><br />
Hint2: 123 and 240 are hexdumps (not necessarily compression)

Attachment:

- 999


### Solution
面倒くさいだけで、根気で解ける問題です。

スクリプトかなんかでできたら、楽なんでしょうけど、手動でやりました。

コマンドヒストリーの一部です。
<pre>
  689  file 871.out
  690  tar zxvf 871.out 
  691  ll
  692  file 123
  693  cat 123
  694  xxd -r 123 123.out
  695  file 123.out 
  696  mv 123.out 123.gz
  697  gzip -d 123.gz 
  698  ll
  699  mv 123 123.bak
  700  gzip -d 123.gz 
</pre>

hexdumpのテキスト アウトプットのやつは、`xxd -r`で解凍できます。

`bcactf{A_l0t_0f_c0mPr3s510n}`



<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はイベント終了後に行った復習です。


<br /><br />
<br /><br />
## [Forensics]: wavey (150)
- - -
### Challenge
> My friend sent me his new mixtape, but honestly I don't think it's that good. Can you take a look at it and figure out what's going on?
<br /><br />
Hint: He specifically described it as wavey...

Attachment:

- straightfire.wav

### Solution
正直、ヒントの意味がわからなかったです。waveyでいろいろ検索したりもした。。。
<br /><br />
スペクトラムを見ると、視覚的にフラグが見えるようです。
<br /><br />
Audacityでやってみました。
<br />
<img src="https://captureamerica.github.io/writeups/img/straightfire.png" alt="straightfire.png">
<br />
左のとこのファイル名（straightfire）が出ているところがプルダウンになってて、デフォルトだとWaveformだけど、そこでSpectrogramを選ぶとこれが出てきます。




<br /><br />
<br /><br />
(2019-09-23)
## [Forensics]: one-punch-zip (250)
- - -
### Challenge
> One Punch Man seemed to have lost the password to his super secret archive. Can you help him crack it?

Attachment:

- opm.png
- superSecure.zip（パスワード不明）

### Solution
Writeupを参照されてもらって、fcrackzipというコマンドで解けることがわかりました。

考えてみたら、wordlistがあればパスワード付きのZipはCrackできるわけだし、別のファイルが添付されているということはそこからwordlistを生成すればいいということですもんね。

zip2johnでも同じことができるかやってみました。
```
root@kali:~/BCACTF# strings opm.png > opm_wordlist.txt


root@kali:~/BCACTF# john hash.txt --wordlist=opm_wordlist.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
w\8VH"$.         (superSecure.zip/flag.txt)   <--- パスワード出てきた！
1g 0:00:00:00 DONE (2019-09-23 09:34) 100.0g/s 540900p/s 540900c/s 540900C/s Po$,c..IEND
Use the "--show" option to display all of the cracked passwords reliably
Session completed


root@kali:~/BCACTF# unzip superSecure.zip 
Archive:  superSecure.zip
[superSecure.zip] flag.txt password: 
 extracting: flag.txt                


root@kali:~/BCACTF# cat flag.txt 
bcactf{u5ing_4ll_string5_0f_1mag3_@s_dictionary?}
```




<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />