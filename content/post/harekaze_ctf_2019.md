---
title: "Harekaze CTF 2019 Writeup"
date: 2019-05-18T13:49:54+09:00
lastmod: 2019-05-25T15:05:00+08:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: "きゃぷあめ"
---

## [Crypto]: Twenty-five
- - -
### Challenge
> With “ppencode”, you can write Perl code using only Perl reserved words.

Attachments:

- crypto.txt（ppencodeでエンコードされたファイル）
- reserved.txt（ご親切に予約後一覧が付いている）
- twenty-five.pl（crypto.txtを読み込んで、変換して実行）

<br />
### Solution
とりあえず、twenty-five.plをそのまま実行してみると、エラーになります。
<pre>
$ perl --version
This is perl 5, version 18, subversion 4 (v5.18.4) built for darwin-thread-multi-2level
:
$ ./twenty-five.pl 
./twenty-five.pl: line 1: use: command not found
./twenty-five.pl: line 3: syntax error near unexpected token `my'
./twenty-five.pl: line 3: `open(my $F, "<:utf8", 'crypto.txt') or die;'
</pre>

<br /><br />
以下がオリジナルです。
```perl
use open qw/:utf8/;

open(my $F, "<:utf8", 'crypto.txt') or die;
my $text;
while (my $l = <$F>)
{
  $l =~ s/[\r\n]+/ /g;
  $text .= $l;
}
close($F);

$text =~ y/abcdefghijklmnopqrstuvwxy/*************************/;
eval($text);
```

<br /><br />
以下のように、先頭4行を追加すると、エラーが出なくなります。
また、evalのところは、printに変えてみました。
```perl
#!/usr/bin/perl
use strict;
use warnings;
use utf8;

use open qw/:utf8/;

open(my $F, "<:utf8", 'crypto.txt') or die;
my $text;
while (my $l = <$F>)
{
  $l =~ s/[\r\n]+/ /g;
  $text .= $l;
}
close($F);

$text =~ y/abcdefghijklmnopqrstuvwxy/*************************/;
print eval($text);
```
これを実行するとですね、画面にいっぱいお星様が出ますｗ
<br />
なぜなら、アルファベットを全部アスタリスクに置き換えているからです。


<br /><br />
crypto.txtを見ると、Perlの予約語が全く出てこないのがわかります。以下、抜粋。
<pre>
m oo o sji m oo o sji m oo o sji m oo o sji m oo o sji m oo o sji m oo o sji m oo
o sji m oo o sji m oo o sji m oo o sji m oo o sji m oo o sji m oo o sji m oo o sji
:
</pre>

reserved.txtと比較してみると、例えば、2つ同じ文字が続く「oo」みたいなやつは、予約語としては「qq」しかないことがわかります。

どの文字をどの文字に変換したらいいかは、次のように長さで並べ替えるとわかりやすくなります。

<pre>
$ cat crypto.txt | tr ' ' '\n' | awk '{print length() ,$0}' | sort -nr | uniq
6 pyjxah
5 xdkyj
5 wlnfa
5 whrgi
5 whidl
5 whgrf
5 whgcj
5 uwjap
5 upgwq
5 spslr
5 rqidl
5 pgwsp
5 myrgf
5 lridl
5 gliyl
5 fldja
5 ejiyu
5 ejady
5 ejadp
5 eadry
5 djiyt
5 dgwap
5 clday
5 chdpy
5 blysq
4 ytyw
4 ytda
4 yswh
4 yksp
4 xyaw
4 xpgb
4 xlyf
4 xgag
4 whgf
4 vgdj
4 uglq
4 sasj
4 qdpp
4 pgwq
4 pdjq
4 lywk
4 lysi
4 lsji
4 ierf
4 gfyj
4 fswq
4 fdfy
4 cslj
4 csda
4 aypp
4 adry
3 ytf
3 ygu
3 wrf
3 whl
3 ugl
3 tgl
3 sji
3 rsf
3 pgx
3 lyu
3 kyw
3 jga
3 idy
3 gwa
3 gli
3 gel
3 fgf
3 dja
3 ady
2 oo
2 ol
1 o
1 m
</pre>
<pre>
$ cat reserved.txt | awk '{print length() ,$0}' | sort -nr | uniq
16 getprotobynumber
14 getprotobyname
13 getservbyport
13 getservbyname
13 gethostbyname
13 gethostbyaddr
12 getnetbyname
12 getnetbyaddr
11 setprotoent
11 setpriority
11 getsockname
11 getprotoent
11 getpriority
11 getpeername
11 endprotoent
10 socketpair
10 setsockopt
10 setservent
10 sethostent
10 getsockopt
10 getservent
10 gethostent
10 endservent
10 endhostent
9 wantarray
9 setnetent
9 rewinddir
9 quotemeta
9 prototype
9 localtime
9 getnetent
9 endnetent
8 truncate
8 syswrite
8 shutdown
8 shmwrite
8 setpwent
8 setgrent
8 readpipe
8 readlink
8 readline
8 getpwuid
8 getpwnam
8 getpwent
8 getlogin
8 getgrnam
8 getgrgid
8 getgrent
8 formline
8 endpwent
8 endgrent
8 dbmclose
8 continue
8 closedir
7 waitpid
7 unshift
7 ucfirst
7 telldir
7 sysseek
7 sysread
7 sysopen
7 syscall
7 symlink
7 sprintf
7 shmread
7 setpgrp
7 seekdir
7 reverse
7 require
7 readdir
7 package
7 opendir
7 lcfirst
7 getppid
7 getpgrp
7 foreach
7 defined
7 dbmopen
7 connect
7 binmode
6 ﻿abs
6 values
6 unpack
6 unlink
6 unless
6 system
6 substr
6 splice
6 socket
6 shmget
6 shmctl
6 semget
6 semctl
6 select
6 scalar
6 rindex
6 return
6 rename
6 printf
6 msgsnd
6 msgrcv
6 msgget
6 msgctl
6 listen
6 length
6 import
6 gmtime
6 format
6 fileno
6 exists
6 delete
6 chroot
6 caller
6 accept
5 write
5 while
5 utime
5 until
5 untie
5 undef
5 umask
5 times
5 study
5 state
5 srand
5 split
5 sleep
5 shift
5 semop
5 rmdir
5 reset
5 print
5 mkdir
5 lstat
5 local
5 ioctl
5 index
5 flock
5 fcntl
5 elsif
5 crypt
5 close
5 chown
5 chomp
5 chmod
5 chdir
5 break
5 bless
5 alarm
4 warn
4 wait
4 time
4 tied
4 tell
4 stat
4 sqrt
4 sort
4 send
4 seek
4 redo
4 recv
4 read
4 rand
4 push
4 pipe
4 pack
4 open
4 next
4 lock
4 link
4 last
4 kill
4 keys
4 join
4 grep
4 goto
4 glob
4 getc
4 fork
4 exit
4 exec
4 eval
4 else
4 each
4 dump
4 chop
4 bind
3 xor
3 vec
3 use
3 tie
3 sub
3 sin
3 say
3 ref
3 pos
3 pop
3 our
3 ord
3 oct
3 not
3 map
3 log
3 int
3 hex
3 for
3 exp
3 eof
3 die
3 cos
3 cmp
3 chr
3 and
2 uc
2 tr
2 qx
2 qw
2 qr
2 qq
2 or
2 no
2 ne
2 my
2 lt
2 le
2 lc
2 if
2 gt
2 ge
2 eq
2 do
1 y
1 x
1 s
1 q
1 m
</pre>

<br /><br />
わかった文字から徐々に埋めて行くと、対応はこうなります。
<pre>
a -> t
b -> b
c -> w
d -> i
e -> u
f -> p
g -> o
h -> h
i -> d
j -> n
k -> v
l -> r
m -> s
n -> y
o -> q
p -> l
q -> k
r -> m
s -> a
t -> x
u -> f
v -> j
w -> c
x -> g
y -> e
</pre>

<br /><br />
Perlのコードの末尾を以下のように書き換えて、実行するとフラグが取れます。（またevalに戻してます）
<pre>
$text =~ y/abcdefghijklmnopqrstuvwxy/tbwiupohdnvrsyqlkmaxfjcge/;
eval($text);
</pre>

<br />
**Flag:** HarekazeCTF{en.wikipedia.org/wiki/Frequency_analysis}

<br /><br />
作問者（nkhrlabさん）による解説<br />
[https://nkhrlab.hatenablog.com/entry/2019/05/19/224643](https://nkhrlab.hatenablog.com/entry/2019/05/19/224643)


<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />