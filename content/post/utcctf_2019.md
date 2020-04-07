---
title: "UTC CTF Writeup"
date: 2019-12-22T19:20:00+09:00
lastmod: 2019-12-23T19:20:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF", "Reviewed"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Futcctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

(2019/12/23 - 復習しました)

URL: [https://utc-ctf.club/challenges](https://utc-ctf.club/challenges)
<br /><br />
以下、スコアです。
<br /><br />

<img src="https://captureamerica.github.io/writeups/img/utcctf_2019_Score.png" alt="utcctf_2019_Score.png">

<img src="https://captureamerica.github.io/writeups/img/utcctf_2019_Chall1.png" alt="utcctf_2019_Chall1.png">

<img src="https://captureamerica.github.io/writeups/img/utcctf_2019_Chall2.png" alt="utcctf_2019_Chall2.png">




<br /><br />
## [Misc]: Optics 2
- - -
### Challenge
> You seem to be professional in Forensics & Optics. Now, the things will get tedious. Let's see what you got.

Attachment:

- chall_0.png ~ chall_440.png


<br />
### Solution
441個のpngが与えられます。それぞれはgrayscaleの正方形の画像です。

21 * 21 = 441 なので、結合してQRにするチャレンジですね。

ImageMagickを使って、21ファイルずつ横に結合し、できた21個のファイルを縦に結合することにしました。

<pre>
convert +append chall_0.png chall_1.png chall_2.png chall_3.png chall_4.png chall_5.png chall_6.png chall_7.png chall_8.png chall_9.png chall_10.png chall_11.png chall_12.png chall_13.png chall_14.png chall_15.png chall_16.png chall_17.png chall_18.png chall_19.png chall_20.png 0.png
convert +append chall_21.png chall_22.png chall_23.png chall_24.png chall_25.png chall_26.png chall_27.png chall_28.png chall_29.png chall_30.png chall_31.png chall_32.png chall_33.png chall_34.png chall_35.png chall_36.png chall_37.png chall_38.png chall_39.png chall_40.png chall_41.png 1.png
convert +append chall_42.png chall_43.png chall_44.png chall_45.png chall_46.png chall_47.png chall_48.png chall_49.png chall_50.png chall_51.png chall_52.png chall_53.png chall_54.png chall_55.png chall_56.png chall_57.png chall_58.png chall_59.png chall_60.png chall_61.png chall_62.png 2.png
convert +append chall_63.png chall_64.png chall_65.png chall_66.png chall_67.png chall_68.png chall_69.png chall_70.png chall_71.png chall_72.png chall_73.png chall_74.png chall_75.png chall_76.png chall_77.png chall_78.png chall_79.png chall_80.png chall_81.png chall_82.png chall_83.png 3.png
convert +append chall_84.png chall_85.png chall_86.png chall_87.png chall_88.png chall_89.png chall_90.png chall_91.png chall_92.png chall_93.png chall_94.png chall_95.png chall_96.png chall_97.png chall_98.png chall_99.png chall_100.png chall_101.png chall_102.png chall_103.png chall_104.png 4.png
convert +append chall_105.png chall_106.png chall_107.png chall_108.png chall_109.png chall_110.png chall_111.png chall_112.png chall_113.png chall_114.png chall_115.png chall_116.png chall_117.png chall_118.png chall_119.png chall_120.png chall_121.png chall_122.png chall_123.png chall_124.png chall_125.png 5.png
convert +append chall_126.png chall_127.png chall_128.png chall_129.png chall_130.png chall_131.png chall_132.png chall_133.png chall_134.png chall_135.png chall_136.png chall_137.png chall_138.png chall_139.png chall_140.png chall_141.png chall_142.png chall_143.png chall_144.png chall_145.png chall_146.png 6.png
convert +append chall_147.png chall_148.png chall_149.png chall_150.png chall_151.png chall_152.png chall_153.png chall_154.png chall_155.png chall_156.png chall_157.png chall_158.png chall_159.png chall_160.png chall_161.png chall_162.png chall_163.png chall_164.png chall_165.png chall_166.png chall_167.png 7.png
convert +append chall_168.png chall_169.png chall_170.png chall_171.png chall_172.png chall_173.png chall_174.png chall_175.png chall_176.png chall_177.png chall_178.png chall_179.png chall_180.png chall_181.png chall_182.png chall_183.png chall_184.png chall_185.png chall_186.png chall_187.png chall_188.png 8.png
convert +append chall_189.png chall_190.png chall_191.png chall_192.png chall_193.png chall_194.png chall_195.png chall_196.png chall_197.png chall_198.png chall_199.png chall_200.png chall_201.png chall_202.png chall_203.png chall_204.png chall_205.png chall_206.png chall_207.png chall_208.png chall_209.png 9.png
convert +append chall_210.png chall_211.png chall_212.png chall_213.png chall_214.png chall_215.png chall_216.png chall_217.png chall_218.png chall_219.png chall_220.png chall_221.png chall_222.png chall_223.png chall_224.png chall_225.png chall_226.png chall_227.png chall_228.png chall_229.png chall_230.png 10.png
convert +append chall_231.png chall_232.png chall_233.png chall_234.png chall_235.png chall_236.png chall_237.png chall_238.png chall_239.png chall_240.png chall_241.png chall_242.png chall_243.png chall_244.png chall_245.png chall_246.png chall_247.png chall_248.png chall_249.png chall_250.png chall_251.png 11.png
convert +append chall_252.png chall_253.png chall_254.png chall_255.png chall_256.png chall_257.png chall_258.png chall_259.png chall_260.png chall_261.png chall_262.png chall_263.png chall_264.png chall_265.png chall_266.png chall_267.png chall_268.png chall_269.png chall_270.png chall_271.png chall_272.png 12.png
convert +append chall_273.png chall_274.png chall_275.png chall_276.png chall_277.png chall_278.png chall_279.png chall_280.png chall_281.png chall_282.png chall_283.png chall_284.png chall_285.png chall_286.png chall_287.png chall_288.png chall_289.png chall_290.png chall_291.png chall_292.png chall_293.png 13.png
convert +append chall_294.png chall_295.png chall_296.png chall_297.png chall_298.png chall_299.png chall_300.png chall_301.png chall_302.png chall_303.png chall_304.png chall_305.png chall_306.png chall_307.png chall_308.png chall_309.png chall_310.png chall_311.png chall_312.png chall_313.png chall_314.png 14.png
convert +append chall_315.png chall_316.png chall_317.png chall_318.png chall_319.png chall_320.png chall_321.png chall_322.png chall_323.png chall_324.png chall_325.png chall_326.png chall_327.png chall_328.png chall_329.png chall_330.png chall_331.png chall_332.png chall_333.png chall_334.png chall_335.png 15.png
convert +append chall_336.png chall_337.png chall_338.png chall_339.png chall_340.png chall_341.png chall_342.png chall_343.png chall_344.png chall_345.png chall_346.png chall_347.png chall_348.png chall_349.png chall_350.png chall_351.png chall_352.png chall_353.png chall_354.png chall_355.png chall_356.png 16.png
convert +append chall_357.png chall_358.png chall_359.png chall_360.png chall_361.png chall_362.png chall_363.png chall_364.png chall_365.png chall_366.png chall_367.png chall_368.png chall_369.png chall_370.png chall_371.png chall_372.png chall_373.png chall_374.png chall_375.png chall_376.png chall_377.png 17.png
convert +append chall_378.png chall_379.png chall_380.png chall_381.png chall_382.png chall_383.png chall_384.png chall_385.png chall_386.png chall_387.png chall_388.png chall_389.png chall_390.png chall_391.png chall_392.png chall_393.png chall_394.png chall_395.png chall_396.png chall_397.png chall_398.png 18.png
convert +append chall_399.png chall_400.png chall_401.png chall_402.png chall_403.png chall_404.png chall_405.png chall_406.png chall_407.png chall_408.png chall_409.png chall_410.png chall_411.png chall_412.png chall_413.png chall_414.png chall_415.png chall_416.png chall_417.png chall_418.png chall_419.png 19.png
convert +append chall_420.png chall_421.png chall_422.png chall_423.png chall_424.png chall_425.png chall_426.png chall_427.png chall_428.png chall_429.png chall_430.png chall_431.png chall_432.png chall_433.png chall_434.png chall_435.png chall_436.png chall_437.png chall_438.png chall_439.png chall_440.png 20.png
convert +append convert -append 0.png 1.png 2.png 3.png 4.png 5.png 6.png 7.png 8.png 9.png 10.png 11.png 12.png 13.png 14.png 15.png 16.png 17.png 18.png 19.png 20.png QR.png
</pre>

<br />
ちなみに、Cでプログラムを書いて、上記のコマンドを出力してます。<br />（かつ、一部結果を手で直している箇所あり。"convert +append "が余分に1個出てくるので。。。）

```C
#include <stdio.h>

int main()
{
	int i, j, k;
	printf( "convert +append " );
	for ( i = 0, j = 1, k = 0 ; i <= 440 ; i++, j++ ) {
		printf( "chall_%d.png ", i );
		if ( j >= 21 ) {
			printf( "%d.png\nconvert +append ", k );
			j = 0;
			k++;
		}
	}
	puts("");
	printf( "convert -append " );
	for ( k = 0 ; k < 21 ; k++ ) {
		printf( "%d.png ", k );
	}
	printf( "QR.png\n" );
	return 0;
}
```

<br />
<img src="https://captureamerica.github.io/writeups/img/utcctf_QR.png" alt="utcctf_QR.png">



<br />
Flag: `utc{merge_and_merge_until_you_decode_it}`


<br /><br />
<br /><br />
## [Misc]: Really Good 🅱icture
- - -
### Challenge
> Instead of a flag, I made you a picture, is that ok?

- flag.png

<br />
### Solution
「青い空を見上げればいつもそこに白い猫」で開いて、ビット抽出前バイト列を確認すると、フラグっぽい文字列が得られます。

<img src="https://captureamerica.github.io/writeups/img/utcctf_Bicture.PNG" alt="utcctf_Bicture.PNG">

これを一旦テキストファイルに保存して、コマンドで整形します。

3文字ずつ繰り返しになっているので、`fold -3` してから `uniq` してまとめてます。

<pre>
$ cat share_doc.txt | cut -d: -f2 | tr -d "\r\n " | fold -3 | uniq | tr -d "\n" ; echo
utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}utc{taste_the_rainbow94100389}.....(略)
</pre>


<br />
Flag: `utc{taste_the_rainbow94100389}`




<br /><br />
<br /><br />
## [Reverse]: Jump! (baby)
- - -
### Challenge
> Remember Strings? Itz still not giving flag
<br /><br />
GIMME FLAG AGAIN!

Attachment:

- jump (ELF 64bit)


<br />
### Solution
Ghidraでソースを確認します。

```C
void gimme_flag(void)

{
  long in_FS_OFFSET;
  undefined4 local_40;
  uint local_3c;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined local_20;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_40 = 0xdeadbeef;
  local_38 = 0xefc1e18ea5ceca9a;
  local_30 = 0xac99d6b0bbc1ca9b;
  local_28 = 0xded097d581df8d8b;
  local_20 = 0;
  if (selfish != 0) {
    puts("Don\'t be selfish! Share the flag with me...");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  local_3c = 0;
  while (local_3c < 0x19) {
    *(byte *)((long)&local_38 + (long)(int)local_3c) =
         *(byte *)((long)&local_38 + (long)(int)local_3c) ^
         *(byte *)((long)&local_40 + (ulong)(local_3c & 3));
    local_3c = local_3c + 1;
  }
  puts((char *)&local_38);
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}
```

<br />
中身はあんまり把握してません。関数名より、たぶんフラグを生成する関数です。

大事なのは、引数が`void`な辺りですかね。引数が無いので、関数のアドレスに直接ジャンプできます。

あ、あとは、途中で`selfish`の値を0に書き換えてあげないといけないです。


<br />
gdbで開いて、mainと、gimme_flagにbreak pointはっておきます。

<pre>
gdb-peda$ x gimme_flag
0x5555555546fa <gimme_flag>:	0x40ec8348e5894855
</pre>

実行してmain()で止まったら、そこからgimme_flag()にジャンプ。

<pre>
gdb-peda$ jump *0x5555555546fa
</pre>

eaxをセットしているところまで、niで進んで、eaxが1になるので0に変えてcontinueします。

<img src="https://captureamerica.github.io/writeups/img/utcctf_gimme_flag.png" alt="utcctf_gimme_flag.png">


<br />
Flag: `utc{a_l1ttle_h4rd3r_:)}`


<br /><br />
<br /><br />
<img src="https://captureamerica.github.io/writeups/img/orange_bar.png" alt="orange_bar.png">
<br />
ここから下はCTF終了後に行った復習です。（他の方のWriteupとか参照してます。）

<br /><br />
## [Crypto]: Xarriors of the World 1 (baby)
- - -
### Challenge
> Did you paid attention to the table of truth? `captain` will help you after you seek the truth.

Attachment:

- ciphertext.txt

中身 <br />
FhUTDwAQXTwCMAQVKV8NPgdHDQpeDgQvAFE2FlMTAh0OGx0e


<br />
### Solution
Base64デコードした時点で、印字可能文字にならなかったので、captain cipherみたいのがあるのかと思ってググっていたら、"captain midnight decoder ring"っていうのがヒットして、意味がわからなくて降参したやつです。

Base64デコードした後、"captain"でXORするのが正解でした。<br />
（どうやったら、そういう発想になるのだろうか。。。たぶん、とりあえずいろいろ試すことが大事。）

以下は、自分で書きました。

```Python
#!/usr/bin/env python
import base64
import sys
captain = list("captain")
d = base64.b64decode("FhUTDwAQXTwCMAQVKV8NPgdHDQpeDgQvAFE2FlMTAh0OGx0e")
i = 0
for x in bytearray(d):
    sys.stdout.write(chr(x ^ ord(captain[i])))
    i = (i + 1) % len(captain)
print ""
```
<br />
Flag: `utc{ay3_c@pt@1n_w3lc0me_t0_x0rriors}`


<br /><br />
<br /><br />
(2020/02/23 - 追記)

ワンライナーでもいけました。

<pre>
$ python -c 'import pwn;import base64;print(pwn.xor(base64.b64decode(open("ciphertext.txt").read()),"captain").decode("UTF-8"))'
utc{ay3_c@pt@1n_w3lc0me_t0_x0rriors}
</pre>



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

