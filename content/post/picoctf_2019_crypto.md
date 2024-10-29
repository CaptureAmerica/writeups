---
title: "picoCTF 2019 Writeup (Cryptography)"
date: 2019-10-13T11:20:00+09:00
lastmod: 2019-10-13T11:20:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=jp&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fpicoctf_2019_crypto%2F">
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
Cryptography の Writeupです。




<br /><br />
## [Cryptography]: Flags (200 points)
- - -
### Challenge
> What do the flags mean?
<br /><br />
Hint: The flag is in the format PICOCTF{}

<img src="https://captureamerica.github.io/writeups/img/pico2019_flag.png" alt="pico2019_flag.png">

<br />

### Solution
"CTF" "flag"とかいうキーワードでググっても絶対見つからないので、何気に手こずりました。

"International Code of Signals" っていうんですね。

https://en.wikipedia.org/wiki/International_Code_of_Signals <br />
http://marinegyaan.com/what-is-meaning-of-all-alphabet-as-per-interco-or-international-code-of-signals/


Flag: `PICOCTF{F1AG5AND5TUFF}`



<br /><br />
<br /><br />
## [Cryptography]: Mr-Worldwide (200 points)
- - -
### Challenge
> A musician left us a message. What's it mean?
<br /><br />

Attachment:

- message.txt

中身：<br />
picoCTF{(35.028309, 135.753082)(46.469391, 30.740883)(39.758949, -84.191605)(41.015137, 28.979530)(24.466667, 54.366669)(3.140853, 101.693207)_(9.005401, 38.763611)(-3.989038, -79.203560)(52.377956, 4.897070)(41.085651, -73.858467)(57.790001, -152.407227)(31.205753, 29.924526)}

<br />

### Solution
どれか一個でググってみると、グーグルマップ上のとある場所が出てくるので、ぞれぞれが緯度と経度（longitude latitude）なのはわかります。

それぞれの場所から1文字取る際に、どこを取るか、だけの問題です。


<table><tr><td bgcolor="#f8f5ec"><pre>
(35.028309, 135.753082)  : Kamigyo Ward, Kyoto, Kyoto Prefecture, Japan Geographic Information
(46.469391, 30.740883)   : Odesa, Odessa Province, Ukraine Geographic Information
(39.758949, -84.191605)  : US, Ohio, Dayton
(41.015137, 28.979530)   : Turkey, Istanbul
(24.466667, 54.366669)   : United Arab Emirates
(3.140853, 101.693207)   : Unnamed Road, 50480 Kuala Lumpur, Federal Territory of Kuala Lumpur, マレーシア
_
(9.005401, 38.763611)    : Ethiopia
(-3.989038, -79.203560)  : Av Nueva Loja, Loja, エクアドル
(52.377956, 4.897070)    : Martelaarsgracht 5, 1012 TN Amsterdam, オランダ
(41.085651, -73.858467)  : Sleepy Hollow, NY 10591 アメリカ合衆国
(57.790001, -152.407227) : Kodiak, AK 99615 アメリカ合衆国
(31.205753, 29.924526)   : Faculty Of Engineering, Al Azaritah WA Ash Shatebi, Qism Bab Sharqi, Alexandria Governorate, エジプト
</pre></td></tr></table>



City名の頭文字でした。

Flag: `picoCTF{KODIAK_ALASKA}`



<br /><br />
<br /><br />
## [Cryptography]: la cifra de (200 points)
- - -
### Challenge
> I found this cipher in an old book. Can you figure out what it says? Connect with nc 2019shell1.picoctf.com 39776.
<br /><br />
Hint1 : There are tools that make this easy.<br />
Hint2 : Perhaps looking at history will help.

<pre>
$ nc 2019shell1.picoctf.com 39776
Encrypted message:
Ne iy nytkwpsznyg nth it mtsztcy vjzprj zfzjy rkhpibj nrkitt ltc tnnygy ysee itd tte cxjltk

Ifrosr tnj noawde uk siyyzre, yse Bnretèwp Cousex mls hjpn xjtnbjytki xatd eisjd

Iz bls lfwskqj azycihzeej yz Brftsk ip Volpnèxj ls oy hay tcimnyarqj dkxnrogpd os 1553 my Mnzvgs Mazytszf Merqlsu ny hox moup Wa inqrg ipl. Ynr. Gotgat Gltzndtg Gplrfdo 

Ltc tnj tmvqpmkseaznzn uk ehox nivmpr g ylbrj ts ltcmki my yqtdosr tnj wocjc hgqq ol fy oxitngwj arusahje fuw ln guaaxjytrd catizm tzxbkw zf vqlckx hizm ceyupcz yz tnj fpvjc hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3xi00pbhf3}

Ehk ktryy herq-ooizxetypd jjdcxnatoty ol f aordllvmlbkytc inahkw socjgex, bls sfoe gwzuti 1467 my Rjzn Hfetoxea Gqmexyt.

Tnj Gimjyèrk Htpnjc iy ysexjqoxj dosjeisjd cgqwej yse Gqmexyt Doxn ox Fwbkwei Inahkw.

Tn 1508, Ptsatsps Zwttnjxiax tnbjytki ehk xz-cgqwej ylbaql rkhea (g rltxni ol xsilypd gqahggpty) ysaz bzuri wazjc bk f nroytcgq nosuznkse ol yse Bnretèwp Cousex.

Gplrfdo’y xpcuso butvlky lpvjlrki tn 1555 gx l cuseitzltoty ol yse lncsz. Yse rthex mllbjd ol yse gqahggpty fce tth snnqtki cemzwaxqj, bay ehk fwpnfmezx lnj yse osoed qptzjcs gwp mocpd hd xegsd ol f xnkrznoh vee usrgxp, wnnnh ify bk itfljcety hizm paim noxwpsvtydkse.
</pre>


<br />

### Solution
"la cifra de 1553 1467 1555 book" でググったら、以下が見つかりました。<br />
Cryptography 'Vigenère cipher' first described by Giovan Battista Bellaso in his book La cifra del. Sig. Giovan Battista Bellaso (Venice).

ヴィジュネル暗号ですね。

かつ、スペイン語が使われているようなので、以下のサイトでスペイン語を指定して解析させたら一発で解けました。

[https://www.guballa.de/vigenere-solver](https://www.guballa.de/vigenere-solver)

Flag: `picoCTF{b311a50_0r_v1gn3r3_c1ph3rd00ebba3}`



<br /><br />
<br /><br />
## [Cryptography]: miniRSA (300 points)
- - -
### Challenge
> Lets decrypt this: ciphertext? Something seems a bit small
<br /><br />
Hints :<br />
RSA tutorial (https://en.wikipedia.org/wiki/RSA_(cryptosystem))<br />
How could having too small an e affect the security of this 2048 bit key?<br />
Make sure you dont lose precision, the numbers are pretty big (besides the e value)


<br />

### Solution
RsaCtfTool.pyで解けます。

Flag: `picoCTF{n33d_a_lArg3r_e_11db861f}`



<br /><br />
<br /><br />
## [Cryptography]: waves over lambda (300 points)
- - -
### Challenge
> We made alot of substitutions to encrypt this. Can you decrypt it? Connect with nc 2019shell1.picoctf.com 45185.
<br /><br />
Hints : Flag is not in the usual flag format


<pre>
$ nc 2019shell1.picoctf.com 45185
-------------------------------------------------------------------------------
fysjexqz cded oz uyve arxj - aedkvdsfu_oz_f_ynde_rxmtwx_mvljldssyw
-------------------------------------------------------------------------------
pd pded syq mvfc myed qcxs x kvxeqde ya xs cyve yvq ya yve zcol qorr pd zxp cde zosg, xsw qcds o vswdezqyyw aye qcd aoezq qomd pcxq pxz mdxsq tu x zcol ayvswdeosj os qcd zdx.  o mvzq xfgsyprdwjd o cxw cxewru dudz qy ryyg vl pcds qcd zdxmds qyrw md zcd pxz zosgosj; aye aeym qcd mymdsq qcxq qcdu exqcde lvq md osqy qcd tyxq qcxs qcxq o mojcq td zxow qy jy os, mu cdxeq pxz, xz oq pded, wdxw poqcos md, lxeqru poqc aeojcq, lxeqru poqc cyeeye ya mosw, xsw qcd qcyvjcqz ya pcxq pxz udq tdayed md.
</pre>


<br />

### Solution
単一換字式暗号 (monoalphabetic substitution ciphers)です。

<pre>
/Volumes/NO NAME/CTF/picoCTF_2019 $ ./mono_cipher_solve.pl 
-------------------------------------------------------------------------------
CONGRATS HERE IS YOUR FLAG - FREQUENCY_IS_C_OVER_LAMBDA_MUPGPENNOD
-------------------------------------------------------------------------------
WE WERE NOT MUCH MORE THAN A QUARTER OF AN HOUR OUT OF OUR SHIP TILL WE SAW HER SINK, AND THEN I UNDERSTOOD FOR THE FIRST TIME WHAT WAS MEANT BY A SHIP FOUNDERING IN THE SEA.  I MUST ACKNOWLEDGE I HAD HARDLY EYES TO LOOK UP WHEN THE SEAMEN TOLD ME SHE WAS SINKING; FOR FROM THE MOMENT THAT THEY RATHER PUT ME INTO THE BOAT THAN THAT I MIGHT BE SAID TO GO IN, MY HEART WAS, AS IT WERE, DEAD WITHIN ME, PARTLY WITH FRIGHT, PARTLY WITH HORROR OF MIND, AND THE THOUGHTS OF WHAT WAS YET BEFORE ME.
</pre>


Flag: `frequency_is_c_over_lambda_mupgpennod`




<br /><br />
<br /><br />
## [Cryptography]: b00tl3gRSA2 (400 points)
- - -
### Challenge
> In RSA d is alot bigger than e, why dont we use d to encrypt instead of e? Connect with nc 2019shell1.picoctf.com 1723.
<br /><br />
Hints : What is e generally?

<br />

### Solution
これも RsaCtfTool.py で解けます。

Flag: `picoCTF{bad_1d3a5_6786084}`





<br /><br />
<br /><br />
## [Cryptography]: b00tl3gRSA3 (400 points)
- - -
### Challenge
> Why use p and q when I can use more? Connect with nc 2019shell1.picoctf.com 12275.
<br /><br />
Hints : There's more prime factors than p and q, finding d is going to be different.


<br />

### Solution
picoCTF 2018のSuper RSA3と同じです。

以下で、factorize。<br />
[https://www.alpertron.com.ar/ECM.HTM](https://www.alpertron.com.ar/ECM.HTM)

Solverコードは、去年のどなたかのWriteupのほぼ流用なので、ここには載せません。

Flag: `picoCTF{too_many_fact0rs_4817985}`




<br /><br />
<br /><br />
## [Cryptography]: john_pollard (500 points)
- - -
### Challenge
> Sometimes RSA certificates are breakable
<br /><br />
Hints :<br />
The flag is in the format picoCTF{p,q}<br />
Try swapping p and q if it does not work<br />

Attachment:

<pre>
-----BEGIN CERTIFICATE-----
MIIB6zCB1AICMDkwDQYJKoZIhvcNAQECBQAwEjEQMA4GA1UEAxMHUGljb0NURjAe
Fw0xOTA3MDgwNzIxMThaFw0xOTA2MjYxNzM0MzhaMGcxEDAOBgNVBAsTB1BpY29D
VEYxEDAOBgNVBAoTB1BpY29DVEYxEDAOBgNVBAcTB1BpY29DVEYxEDAOBgNVBAgT
B1BpY29DVEYxCzAJBgNVBAYTAlVTMRAwDgYDVQQDEwdQaWNvQ1RGMCIwDQYJKoZI
hvcNAQEBBQADEQAwDgIHEaTUUhKxfwIDAQABMA0GCSqGSIb3DQEBAgUAA4IBAQAH
al1hMsGeBb3rd/Oq+7uDguueopOvDC864hrpdGubgtjv/hrIsph7FtxM2B4rkkyA
eIV708y31HIplCLruxFdspqvfGvLsCynkYfsY70i6I/dOA6l4Qq/NdmkPDx7edqO
T/zK4jhnRafebqJucXFH8Ak+G6ASNRWhKfFZJTWj5CoyTMIutLU9lDiTXng3rDU1
BhXg04ei1jvAf0UrtpeOA6jUyeCLaKDFRbrOm35xI79r28yO8ng1UAzTRclvkORt
b8LMxw7e+vdIntBGqf7T25PLn/MycGPPvNXyIsTzvvY/MXXJHnAqpI5DlqwzbRHz
q16/S1WLvzg4PsElmv1f
-----END CERTIFICATE-----
</pre>


<br />

### Solution
opensslコマンドを使って証明書の内容を確認します。

<pre>
$ openssl x509 -text -noout -in cert

Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 12345 (0x3039)
    Signature Algorithm: md2WithRSAEncryption
        Issuer: CN=PicoCTF
        Validity
            Not Before: Jul  8 07:21:18 2019 GMT
            Not After : Jun 26 17:34:38 2019 GMT
        Subject: OU=PicoCTF, O=PicoCTF, L=PicoCTF, ST=PicoCTF, C=US, CN=PicoCTF
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (53 bit)
                Modulus: 4966306421059967 (0x11a4d45212b17f)   <--- (p*q)
                Exponent: 65537 (0x10001)                      <--- e
    Signature Algorithm: md2WithRSAEncryption
         07:6a:5d:61:32:c1:9e:05:bd:eb:77:f3:aa:fb:bb:83:82:eb:
         9e:a2:93:af:0c:2f:3a:e2:1a:e9:74:6b:9b:82:d8:ef:fe:1a:
         c8:b2:98:7b:16:dc:4c:d8:1e:2b:92:4c:80:78:85:7b:d3:cc:
         b7:d4:72:29:94:22:eb:bb:11:5d:b2:9a:af:7c:6b:cb:b0:2c:
         a7:91:87:ec:63:bd:22:e8:8f:dd:38:0e:a5:e1:0a:bf:35:d9:
         a4:3c:3c:7b:79:da:8e:4f:fc:ca:e2:38:67:45:a7:de:6e:a2:
         6e:71:71:47:f0:09:3e:1b:a0:12:35:15:a1:29:f1:59:25:35:
         a3:e4:2a:32:4c:c2:2e:b4:b5:3d:94:38:93:5e:78:37:ac:35:
         35:06:15:e0:d3:87:a2:d6:3b:c0:7f:45:2b:b6:97:8e:03:a8:
         d4:c9:e0:8b:68:a0:c5:45:ba:ce:9b:7e:71:23:bf:6b:db:cc:
         8e:f2:78:35:50:0c:d3:45:c9:6f:90:e4:6d:6f:c2:cc:c7:0e:
         de:fa:f7:48:9e:d0:46:a9:fe:d3:db:93:cb:9f:f3:32:70:63:
         cf:bc:d5:f2:22:c4:f3:be:f6:3f:31:75:c9:1e:70:2a:a4:8e:
         43:96:ac:33:6d:11:f3:ab:5e:bf:4b:55:8b:bf:38:38:3e:c1:
         25:9a:fd:5f
</pre>

Modulus 4966306421059967 を factorize。<br />

Flag: `picoCTF{73176001,67867967}`





<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

