---
title: "DeconstruCT.F 2021 Writeup"
date: 2021-10-06T21:00:00+09:00
lastmod: 2021-10-06T21:00:00+09:00
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
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fdeconstruct_ctf_2021%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.dscvit.com/challenges](https://ctf.dscvit.com/challenges)
<br /><br />

1450点を獲得し、79位でした。

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deconstruCT.F_2021_Score1.png" alt="deconstruCT.F_2021_Score1.png">

<br><br>

<img src="https://captureamerica.github.io/writeups/img/deconstruCT.F_2021_Score2.png" alt="deconstruCT.F_2021_Score2.png">


<br><br>
チャレンジのリストです。

<img src="https://captureamerica.github.io/writeups/img/deconstruCT.F_2021_chall1.png" alt="deconstruCT.F_2021_chall1.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deconstruCT.F_2021_chall2.png" alt="deconstruCT.F_2021_chall2.png">

<br>

<img src="https://captureamerica.github.io/writeups/img/deconstruCT.F_2021_chall3.png" alt="deconstruCT.F_2021_chall3.png">

<br>



<br /><br />
## [Crypto]: RSA - 3 (250 points)
- - -
### Challenge
> Alright, this is the big leagues. You have someone's Public Key. This isn't unusual, if you want to send someone an encrypted message, you have to have thier public key. Your job is to evaluate this public key, and obtain the value of the secret exponent or decryption exponent (The value of "d" in an RSA encryption).
<br /><br />
Wrap the number that you find with dsc{<number>}!

Attachment:
- mykey.pub

<br />
### Solution
まず、以下を試したのですが、エラーになりました。

<pre>
$ ssh-keygen -f mykey.pub -e -m pem | openssl asn1parse
Load key "mykey.pub": invalid format
Error: offset out of range
</pre>

<br>

そこで、RsaCtfTool.py を使って private key を出力しました。

<pre>
$ ~/Python3/RsaCtfTool/RsaCtfTool.py --publickey ./mykey.pub --private
-----BEGIN RSA PRIVATE KEY-----
MIIEXwIBAAKCAQEB+34C7GuhHbhLHus9oqCfHR5N2e6WlnXb+MP5qCbY9fbjoWmg
VqKTRu8Zv81KjjlQ531oc8x4tf0H4kyuPjngAI0UjWdEcNnNWy7ErnJzdwW8jGrZ
Spj7BZe9eoPdo3l16lnTDQCxTnm/1YF+crA1Ek7wIQG5S0fguTGebiwLX79qVFcC
RvCccSQKhiuJiZjK0MOrWYlnm8O518tw0ZUuaFhgtFaBJyTI04aN5oTZF3gyuPDZ
8MCTp7wYoJ4CvcONlUpobAqSZ1/VIqDxlYM2Yo6h101wGzW/jucsg+8Np+V+4vHX
aSLpz6DOhA7TZIAozzL+4I5SfL0lzzfXSQB8CQKCAQEBHvBcAbNv9v7I/ZieaKjZ
xEclI5AXjA/igQcW4sz7uHPyt0/5aX5TGEkrfROs9renIw7JTkXeo9uArubEIcp4
7g4346dg5i0tmxbUzF/Pzz3JJGqygmhbVnIlMP93Iwm2VUOMuTSffK01NdmyysC7
xy0OudHb+GtzUv40H2rcTe6VqPuV0pVY5qivnjPeBKl5TVsxrwbyVXdj+1hjh2pw
c2fUZY1LZUAhybrxK/9d2LcZeidUK8IWV92zgE/AYbNDsbwruLR91iO9DTEH99z0
OIjMj/xnIkY/kb8j5lCdIITsU8VxAdzkx05IIa54t6o2+2vXKYfQgSwjeXRBywgg
lQJAehHdzEMx5okjFvPpElQjZWvTuYT9AG04F4b2MMWiMLeLPIt8Clma+tHt2G/A
BWuujnW/UDFVxGsk6KhvsIfWFQKBgQEJOVoWZwRXdfRDN0Ws3dF7NI5Ju9P65hyt
ByBC3WlEZH9R2utRURFIRfaxVmAedseDfUrurqFdWLSYGT2yawBoYQq2uDRVB6P7
5RiVZIhKJjeZllIlKAUwPqbcTpeyxsL9izDxZrq14PzvUjjpa9lpCD4jbEHGhiap
oQqApHMDVwKBgQHp17BSkIX1r+qAPtsBuOMeQIONQx5+8cl8CKpW9bOSWq3vWTYE
La8xGU7ri9Sg4p2pkHV3mZwIdSeGj7iOgLUykYoDzKWxwqxU5K9hYr7Jl64TLwJG
N8JKBRFvcWr1RTu6K9HV3jMqj7r/Y14QMYZevn+7XUFJ5MqsmVnGNdk/nwJAehHd
zEMx5okjFvPpElQjZWvTuYT9AG04F4b2MMWiMLeLPIt8Clma+tHt2G/ABWuujnW/
UDFVxGsk6KhvsIfWFQJAehHdzEMx5okjFvPpElQjZWvTuYT9AG04F4b2MMWiMLeL
PIt8Clma+tHt2G/ABWuujnW/UDFVxGsk6KhvsIfWFQKBgQDOrWptQGDlH1VYjTk2
PvGX6PEiClix9zUvEicK6naC7cyz0LvNY0FtVY/uwhyv2sFxceH1LxdlsPy+xWFN
/GHaIoCU00C0Anq6eOqDKynvPlt4YYNI5EtFoi/r/dzioxHTXCiwbOeIkYWytmKF
6OXjzPgDat8h322YbtYnj8mfXg==
-----END RSA PRIVATE KEY-----

$ ssh-keygen -f mykey_private.key -e -m pem | openssl asn1parse
    0:d=0  hl=4 l= 522 cons: SEQUENCE
    4:d=1  hl=4 l= 257 prim: INTEGER           :01FB7E02EC6BA11DB84B1EEB3DA2A09F1D1E4DD9EE969675DBF8C3F9A826D8F5F6E3A169A056A29346EF19BFCD4A8E3950E77D6873CC78B5FD07E24CAE3E39E0008D148D674470D9CD5B2EC4AE72737705BC8C6AD94A98FB0597BD7A83DDA37975EA59D30D00B14E79BFD5817E72B035124EF02101B94B47E0B9319E6E2C0B5FBF6A54570246F09C71240A862B898998CAD0C3AB5989679BC3B9D7CB70D1952E685860B456812724C8D3868DE684D9177832B8F0D9F0C093A7BC18A09E02BDC38D954A686C0A92675FD522A0F1958336628EA1D74D701B35BF8EE72C83EF0DA7E57EE2F1D76922E9CFA0CE840ED3648028CF32FEE08E527CBD25CF37D749007C09
  265:d=1  hl=4 l= 257 prim: INTEGER           :011EF05C01B36FF6FEC8FD989E68A8D9C447252390178C0FE2810716E2CCFBB873F2B74FF9697E5318492B7D13ACF6B7A7230EC94E45DEA3DB80AEE6C421CA78EE0E37E3A760E62D2D9B16D4CC5FCFCF3DC9246AB282685B56722530FF772309B655438CB9349F7CAD3535D9B2CAC0BBC72D0EB9D1DBF86B7352FE341F6ADC4DEE95A8FB95D29558E6A8AF9E33DE04A9794D5B31AF06F2557763FB5863876A707367D4658D4B654021C9BAF12BFF5DD8B7197A27542BC21657DDB3804FC061B343B1BC2BB8B47DD623BD0D3107F7DCF43888CC8FFC6722463F91BF23E6509D2084EC53C57101DCE4C74E4821AE78B7AA36FB6BD72987D0812C23797441CB082095
</pre>

<br>

`n`と`e`を10進数になおして、以下のコードで `d` を求めました。

```Python
import owiener

n = 64064959164923876064874945473407049985543119992992738119252749231253142464203647518777455475109972581684732621072998898066728303433300585291527582979430276357787634026869116095391514311111174206395195817672737320837240364944609979844601986221462845364070396665723029902932653368943452652854174197070747631242101084260912287849286644699582292473152660004035330616149016496957012948833038931711943984563035784805193474921164625068468842927905314268942153720078680937345365121129404384633019183060347129778296640500935382186867850407893387920482141216498339346081106433144352485571795405717793040441238659925857198439433
e = 36222680858414256161375884602150640809062958718117141382923099494341733093172587117165920097285523276338274750598022486976083511178091392849986039384975758609343597548039166024042264614496506087597114091663955133779956176941325431822684716988128271384410010471755324833136859652978240297120618458534306923558546176110055737233883129780378153307730890915697357455996361736492022695824172516806204252765904924281272883818154621932085365817823019773860783687666788095035790491006333432295698178378520444810813882117817329847874531809530929345430796600870728736678389479159328119322587647856274762262358880664585675219093
d = owiener.attack(e, n)

if d is None:
    print("Failed")
else:
    print("Hacked d={}".format(d))
```

<br />

Flag: `dsc{6393313697836242618414301946448995659516429576261871356767102021920538052481829568588047189447471873340140537810769433878383029164089236876209147584435733}`


<br /><br />
<br /><br />
## [Rev]: Failed Logins (150 points)
- - -
### Challenge
> Our supreme leader Rithicc was asked to store a flag, but he misunderstood and made an app instead (which he always does) smh
<br /><br />

Attachment:

- chall.apk


<br />
### Solution
実は、Android Studio使って実際に動かしたり、decompileしてコードを見たりして色々やったのですが、結局のところ、以下の方法でフラグは取れます。

<br>

.apkファイルは zip圧縮されたファイルなので、まずunzipします。

次に、出てきた classes.dex に対して strings をかけて、見つかった flag の元を rot13 するとフラグが得られます。

<pre>
$ strings classes.dex | grep "flag"
, flags=
KCalled removeDetachedView with a view which is not flagged as tmp detached.
Must provide flag PRE or POST
+Only MODE_IN and MODE_OUT flags are allowed
Pattern.compile(pattern, flags)
Requested flags 0x
mStable Ids are required for the adapter to function properly, and the adapter takes care of setting the flag.
flag
%flag=qfp{4aqe01q_4cx_p4a_83_e3i3e53q}
</pre>

<br />

Flag: `dsc{4ndr01d_4pk_c4n_83_r3v3r53d}`



<br /><br />
<br /><br />
## [Rev]: Scraps (150 points)
- - -
### Challenge
> One of our coders have locked down an application that is scheduled to be released tommorow.
<br />
Can you unlock the application as soon as possible.


Attachment:

- Release-1.0 (ELF 64bit)


<br />
### Solution
Ghidraでコードを確認します。

{{< highlight C "linenos=table,hl_lines=14-16" >}}
undefined8 main(undefined8 param_1,uchar *param_2,undefined8 param_3,uchar *param_4,size_t param_5)

{
  int iVar1;
  undefined4 extraout_var;
  long in_FS_OFFSET;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined local_20;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_38 = 0x564233656a4e485a;
  local_30 = 0x5139314d4d4a6a4d;
  local_28 = 0x394a7a4d6a4e5461;
  local_20 = 0;
  iVar1 = decrypt((EVP_PKEY_CTX *)&local_38,param_2,(size_t *)0x5139314d4d4a6a4d,param_4,param_5);
  puts((char *)CONCAT44(extraout_var,iVar1));
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}


int decrypt(EVP_PKEY_CTX *ctx,uchar *out,size_t *outlen,uchar *in,size_t inlen)

{
  return 0x102008;
}
{{< / highlight >}}

<br>

とりあえず、文字になりそうな箇所（ハイライト部分）があるので変換します。

<br />

反転して base64 デコードするとフラグが得られます。

<pre>
$ echo 9JzMjNTaQ91MMJjMVB3ejNHZ | rev | base64 -d ; echo
dsc{pU22L3_Pi3c32}
</pre>

<br />

Flag: `dsc{pU22L3_Pi3c32}`


<br />

ちなみにですが、Base64、Base16、Base85、reverse、などのもろもろのデコードをまとめてやる自作ツールがあって、実際のところはそれを使って解きました。


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
