---
title: "CSAW CTF'19 CTF Writeup"
date: 2019-09-16T14:00:00+09:00
lastmod: 2019-09-16T14:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fcsaw_ctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.csaw.io/challenges](https://ctf.csaw.io/challenges)
<br /><br />
とりあえず、Reversingの一番簡単なやつだけ解きました。
<br /><br />



<br /><br />
## [Reversing]: BELEAF (50 points)
- - -
### Challenge
> tree sounds are best listened to by https://binary.ninja/demo or ghidra

Attachment:

- beleaf (64-bit ELF)


<br />
### Solution
言われた通り、Ghidraでデコンパイルします。

```C

undefined8 FUN_001008a1(void)

{
  size_t sVar1;
  long lVar2;
  long in_FS_OFFSET;
  ulong local_b0;
  char local_98 [136];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  printf("Enter the flag\n>>> ");
  __isoc99_scanf(&DAT_00100a78,local_98);
  sVar1 = strlen(local_98);
  if (sVar1 < 0x21) {
    puts("Incorrect!");
                    /* WARNING: Subroutine does not return */
    exit(1);
  }
  local_b0 = 0;
  while (local_b0 < sVar1) {
    lVar2 = FUN_001007fa((ulong)(uint)(int)local_98[local_b0]);
    if (lVar2 != *(long *)(&DAT_003014e0 + local_b0 * 8)) {
      puts("Incorrect!");
                    /* WARNING: Subroutine does not return */
      exit(1);
    }
    local_b0 = local_b0 + 1;
  }
  puts("Correct!");
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```
```C

long FUN_001007fa(char cParm1)

{
  long local_10;
  
  local_10 = 0;
  while ((local_10 != -1 && ((int)cParm1 != *(int *)(&DAT_00301020 + local_10 * 4)))) {
    if ((int)cParm1 < *(int *)(&DAT_00301020 + local_10 * 4)) {
      local_10 = local_10 * 2 + 1;
    }
    else {
      if (*(int *)(&DAT_00301020 + local_10 * 4) < (int)cParm1) {
        local_10 = (local_10 + 1) * 2;
      }
    }
  }
  return local_10;
}
```

<br />
上記の関数内で参照しているDAT_003014e0とDAT_00301020には、比較用のデータが入ってます。

こんな感じ。（抜粋）

```
                             DAT_003014e0                                    XREF[2]:     FUN_001008a1:0010096b(*), 
                                                                                          FUN_001008a1:00100972(R)  
        003014e0 01              ??         01h
        003014e1 00              ??         00h
        003014e2 00              ??         00h
        003014e3 00              ??         00h
        003014e4 00              ??         00h
        003014e5 00              ??         00h
        003014e6 00              ??         00h
        003014e7 00              ??         00h
        003014e8 09              ??         09h
        003014e9 00              ??         00h
        003014ea 00              ??         00h
        003014eb 00              ??         00h
```

<br />
入力文字列の長さチェックは0x21で、後は比較データにマッチすればOK。

ここからが解法です。
```C
#include <stdio.h>
#define FLAG_LEN 33

char letter1[] = {0x77, 0x66, 0x7B, 0x5F, 0x6E, 0x79, 0x7D, 0xFF, 0x62, 0x6C, 0x72, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0x61, 0x65, 0x69, 0xFF, 0x6F, 0x74, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF};
char position1[] = {0x01, 0x09, 0x11, 0x27, 0x02, 0x00, 0x12, 0x03, 0x08, 0x12, 0x09, 0x12, 0x11, 0x01, 0x03, 0x13, 0x04, 0x03, 0x05, 0x15, 0x2E, 0x0A, 0x03, 0x0A, 0x12, 0x03, 0x01, 0x2E, 0x16, 0x2E, 0x0A, 0x12, 0x06};

int sub( char c )
{
	int pos = 0;

	while ( pos != 0xFF ) {
		if ( c < letter1[pos] ) {
			pos = pos * 2 + 1;
		} else if ( c > letter1[pos] ) {
			pos = ( pos + 1 ) * 2;
		} else if ( c == letter1[pos] ) {
			break;
		}
		// Segmentation Fault対策
		if ( pos > FLAG_LEN )
			break;
	}
	return pos;
}

int main()
{
	int i, pos;
	char c;
	
	for ( i = 0, pos = 0 ; i < FLAG_LEN ; i++ ) {
		for ( c = 0x20 ; c <= 0x7e ; c++ ) {
			pos = sub( c );
			if ( pos == position1[i] ) {
				printf( "%c", c );
				break;
			}
		}
	}
	puts("");
	return 0;
}
```

ほぼ、ghidraのコードのままですけど、文字をBrute Forceを追加してて、その際にハズレ文字だとposが範囲外になってSegmentation Faultになるので、posの範囲チェックも入れてます。

実行結果：
```
$ ./beleaf_solve.o 
flag{we_beleaf_in_your_re_future}
```

Flag: `flag{we_beleaf_in_your_re_future}`


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />

