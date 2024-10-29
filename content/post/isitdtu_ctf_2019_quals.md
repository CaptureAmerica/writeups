---
title: "ISITDTU 2019 quals Writeup"
date: 2019-07-01T13:21:40+09:00
lastmod: 2019-07-01T13:21:40+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
{{% right %}}
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fisitdtu_ctf_2019_quals%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">
</a>
{{% /right %}}

URL: [https://ctf.isitdtu.com/](https://ctf.isitdtu.com/)
<br /><br />
開始早々サーバがダウンして、そのうち直るかと思ったら次の日も問題が解消されてなくて、あんまり興味をひくチャレンジもなくってモチベーションがあがらなかったです。。。
<br /><br />
とりあえず、面白いと思った1問だけやりました。


<br /><br />
<br /><br />
## [Programming] Do you like math?
- - -
### Challenge
> nc 104.154.120.223 8083

アクセスすると、以下のような計算式が出てきます。1秒くらい経つとコネクションが切れます。
```
  #    #####         #####   #####           
 ##   #     #   #   #     # #     #          
# #   #         #         # #     #    ##### 
  #   ######  #####  #####   #####           
  #   #     #   #         # #     #    ##### 
  #   #     #   #   #     # #     #          
#####  #####         #####   #####           
```

<br />

### Solution
解き方は見ての通りなんですが、アスキーアートを判別して数値に直して計算して結果を送るだけです。
<br /><br />
とりあえず、何度かアクセスして、全部の数字と記号のアスキーアートを集めます。
<br /><br />
スライスで7文字幅ずつチェックするようにしました。
<br /><br />
ポイントとなるところは、数字の「1」と記号は横幅が狭いので、次の文字を見に行く際の移動幅が異なる事くらいです。
<br /><br />
文字の判別ができてしまえば、後はeval()するだけです。
<br /><br />

```Python
#!/usr/bin/env python
from pwn import *
import re

host = "104.154.120.223"
port = 8083
r = remote(host, port)


'''

  ###  
 #   # 
#     #
#     #  <---
#     #
 #   # 
  ###  
'''
def check_zero( tmp_l ):
    if tmp_l[4] == "#     #":
        return 0
    else:
        return -1
'''

  #
 ##    <---
# #  
  #  
  #  
  #  
#####
'''
def check_one( tmp_l ):
    #if re.search( tmp_l[2], " ##  "):
    if (tmp_l[2])[:5] == " ##  ":
        return 1
    else:
        return -1

'''

 ##### 
#     #
      #  <---
 ##### 
#        <---
#      
#######
'''
def check_two( tmp_l ):
    if tmp_l[3] == "      #" and tmp_l[5] == "#      ":
        return 2
    else:
        return -1

'''

 ##### 
#     #  <---
      #  <---
 ##### 
      #  <---
#     #
 ##### 
'''
def check_three( tmp_l ):
    if tmp_l[2] == "#     #" and tmp_l[3] == "      #" and tmp_l[5] == "      #":
        return 3
    else:
        return -1

'''

#        <---
#    # 
#    # 
#    # 
#######
     # 
     #
'''
def check_four( tmp_l ):
    if tmp_l[1] == "#      ":
        return 4
    else:
        return -1

'''

#######
#      
#        <---
###### 
      #  <---
#     #
 ##### 
'''
def check_five( tmp_l ):
    if tmp_l[3] == "#      " and tmp_l[5] == "      #":
        return 5
    else:
        return -1

'''

 ##### 
#     #
#        <---
###### 
#     #  <---
#     #
 ##### 
'''
def check_six( tmp_l ):
    if tmp_l[3] == "#      " and tmp_l[5] == "#     #":
        return 6
    else:
        return -1

'''

#######
#    #   <---
    #  
   #   
  #    
  #    
  #     
'''
def check_seven( tmp_l ):
    if tmp_l[2] == "#    # ":
        return 7
    else:
        return -1

'''

 #####           
#     #          
#     #  <---
 #####           
#     #  <---
#     # 
 ##### 
'''
def check_eight( tmp_l ):
    if tmp_l[3] == "#     #" and tmp_l[5] == "#     #":
        return 8
    else:
        return -1

'''

 ##### 
#     #
#     #  <---
 ######
      #  <---
#     #
 ##### 
'''
def check_nine( tmp_l ):
    if tmp_l[3] == "#     #" and tmp_l[5] == "      #":
        return 9
    else:
        return -1

'''


  #  
  #     <---
#####
  #  
  #  
'''
def check_plus( tmp_l ):
    #if re.search( tmp_l[3], '  #  '):
    if (tmp_l[3])[:5] == "  #  ":
        return -2
    else:
        return -1

'''


 #   # 
  # #    <---
#######
  # #  
 #   # 
'''
def check_times( tmp_l ):
    if tmp_l[5] == "  # #  ":
        return -3
    else:
        return -1

'''




#####  <---
'''
def check_minus( tmp_l ):
    #if re.search( tmp_l[4], '##### '):
    if (tmp_l[4])[:6] == "##### ":
        return -4
    else:
        return -1

'''



   #####
          <---
   #####
'''
def check_equal( tmp_l ):
    #if re.search( tmp_l[5], ' #####'):
    if (tmp_l[4])[:3] == "   ":
        return -5
    else:
        return -1

while 1:
    try:
        msg = r.recvregex(">>>|ISITDTU{.*}")
        
        lines = msg.split("\n")
        
        pos = 0
        txt = ""
        while 1:
            tmp_l = []
            if pos == 0:
                for l in lines:
                    print( l )

            for l in lines:
                tmp_l.append(l[pos:pos+7])

            # just to avoid infinite loop
            if pos > 80:
                break
        
            if check_zero( tmp_l ) != -1:
                txt += "0"
                pos += 8
                continue
            if check_one( tmp_l ) != -1:
                txt += "1"
                pos += 6
                continue
            if check_two( tmp_l ) != -1:
                txt += "2"
                pos += 8
                continue
            if check_three( tmp_l ) != -1:
                txt += "3"
                pos += 8
                continue
            if check_four( tmp_l ) != -1:
                txt += "4"
                pos += 8
                continue
            if check_five( tmp_l ) != -1:
                txt += "5"
                pos += 8
                continue
            if check_six( tmp_l ) != -1:
                txt += "6"
                pos += 8
                continue
            if check_seven( tmp_l ) != -1:
                txt += "7"
                pos += 8
                continue
            if check_eight( tmp_l ) != -1:
                txt += "8"
                pos += 8
                continue
            if check_nine( tmp_l ) != -1:
                txt += "9"
                pos += 8
                continue
            if check_plus( tmp_l ) != -1:
                txt += "+"
                pos += 6
                continue
            if check_times( tmp_l ) != -1:
                txt += "*"
                pos += 8
                continue
            if check_minus( tmp_l ) != -1:
                txt += "-"
                pos += 6
                continue
            if check_equal( tmp_l ) != -1:
                print(txt)
                ans = eval(txt)
                print(ans)
                r.sendline(str(ans))
                break
            # just to avoid infinite loop
            pos += 8
    except:  # did not use this exception eventually.
        print(msg)
        exit(0)
```
Good job, this is your flag:   `ISITDTU{sub5cr1b3_b4_t4n_vl0g_4nd_p3wd13p13}`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />
