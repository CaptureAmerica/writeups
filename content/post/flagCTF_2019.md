---
title: "flagCTF Writeup"
date: 2019-06-16T10:00:00+09:00
lastmod: 2019-06-16T10:00:00+09:00
draft: false
keywords: []
description: ""
tags: ["CTF"]
categories: ["CTF"]
author: ""
---
<a href="https://captureamerica.github.io/writeups/post/flagctf_2019/">
<img src="https://captureamerica.github.io/writeups/img/Jp.png" alt="Japanese">日本語
</a>&nbsp;
<a href="https://translate.google.com/translate?hl=en&sl=ja&tl=en&u=https%3A%2F%2Fcaptureamerica.github.io%2Fwriteups%2Fpost%2Fflagctf_2019%2F">
<img src="https://captureamerica.github.io/writeups/img/En.png" alt="English">English (Google)
</a>

<br />

URL: [http://flagctf.com/](http://flagctf.com/)
<br /><br />
Packer Captureの問題があったのがよかったです。
<br /><br />
順位が13位で一見成績が良かったように見えますけど、参加者は100人くらいでした。

<img src="https://captureamerica.github.io/writeups/img/flagCTF_Score.png" alt="flagCTF_Score.png">



<br /><br />
<br /><br />
## [Network]: Web Search Packet Capture Q5 (30)
- - -
### Challenge
> The user searched for some queries more times than others. Out of all the queries which are tied for 1st place in number of times searched, which one is last alphabetically?
<br /><br />
Note: your team is limited to 6 attempts on this challenge.



Attachment:

- WebSearchingFlagCTF2019.pcapng (Q1 ~ Q5で共通)


### Solution
```bash
$ tshark -r WebSearchingFlagCTF2019.pcapng | grep -i "search/?" | tail -n 1
 4891 82.558070239 192.168.52.110 → 23.10.112.128 HTTP 149 GET /search/?q=auyyxxriyf HTTP/1.1

$ tshark -r WebSearchingFlagCTF2019.pcapng | grep -i "search/?" | cut -d= -f2 | sort | uniq -c | egrep "^   4" | sort -k 2
   4 afqyhedslp HTTP/1.1 
   4 bnvfsywbnx HTTP/1.1 
   4 czlhbopugw HTTP/1.1 
   4 faxuzyzwfl HTTP/1.1 
   4 onphdplbbx HTTP/1.1 
   4 ouaxyqrnjz HTTP/1.1 
```
Flag: `ouaxyqrnjz`

<br />
{{% admonition tip "Tips" %}}
uniq -cは、重複した行数を表示するオプションです。
{{% /admonition %}}



<br /><br />
<br /><br />
## [Password Cracking]: Cracking #1 ~ #7
- - -
### Challenge
> Agents have captured password hashes from the hackers in the black-hat group "1337 Hax0r T3am". We know that they think they are 1337 but they don't like to use symbols after the words at the base of their passwords, because they heard "Password1!" was a bad password without understanding why. They are known to share a similar interest outside of computers, which may be reflected in their passwords. Note: some of these passwords are unlikely to be found verbatim in password cracking lists or in lists on the internet. It will be necessary to construct wordlists once you find out what the theme is, and extend the wordlists that you find to create longer sensible phrases that might be used as the base of passwords.
<br /><br />
Hint: use password cracking software such as hashcat, or john the ripper, to try many possible combinations at once. without a GPU, it can be done in under 1 hour on a decent processor. Hint: start w ith lower entropy, and think how longer passwords might be constructed specific to terms in this domain (field).
<br /><br />
Do not brute-force attempt against the website (here), or that will result in a ban. For this set of challenges, you should be sure when you arrived at the answer.
<br /><br />
Stay tuned for the video posted at 7:30 pm ET if you're new to the process as we show how to crack the first hash in this series.

以下は、Cracking #6 (50)より。

> The original hash given was: a23b2583b0f977bf3d41ff546f67f13c
After there were no solves in the sprint round, you can also crack this hash where the variation of the password contains only lowercase letters: 9cd2a2568ed7d991fb8d2eb1e7e1c7ec


### Solution
いくつかは、rainbow tableでcrackできます。<br />
[https://crackstation.net/](https://crackstation.net/)
<br />
53b45bbef61cb7ed537d14b1257545d5 : chick3n
<br />
005d05de29487ec44cd07bd9d757d4e1 : carrot
<br />
2a3555ff38f17a69009fc8da52330f54 : peasandcarrots

<br />
残り
<br />
ca1443c1c17cbc96d0ae8fa4bd0e2bba
<br />
7153a339468b43cd819123708567e3d1
<br />
a23b2583b0f977bf3d41ff546f67f13c
<br />
9cd2a2568ed7d991fb8d2eb1e7e1c7ec
<br />
dc098cc7aadabcffbf1264af50b33027


<br />
すでにわかっている3つ（chick3n, carrot, peasandcarrots）より、以下の傾向がわかります。

- 食べ物の単語
- Leetを使用<br />
[https://ja.wikipedia.org/wiki/Leet](https://ja.wikipedia.org/wiki/Leet)<br />
[https://en.wikipedia.org/wiki/Leet](https://en.wikipedia.org/wiki/Leet)
- 複数形のsが付いたりする

<br />
1. まず、Webから食べ物の英単語をかき集めました。
<br /><br />
2. 以下のツールを使わせてもらい、Leetのバリエーションを使って辞書を強化。
<br />
&nbsp;&nbsp;&nbsp;&nbsp;[https://github.com/madglory/permute_wordlist](https://github.com/madglory/permute_wordlist)
<br /><br />
3. Hashcat実行。<br />
&nbsp;&nbsp;&nbsp;&nbsp;$ hashcat -m 0 -a 0 hash.txt my_food_dic.txt
<br /><br /><br />
さっぱりダメでした。。。orz



<br /><br />
<br /><br />
## [Programming]: ME! (45)
- - -
### Challenge
> "You can't spell 'awesome' without 'me'!" -- Taylor Swift/Brendon Urie, "ME!" (2019)
<br /><br />
Even with all that you hear in pop songs these days, that is a true statement! You wonder how many words are spelled with the letters 'm' and 'e' in that order, but not necessarily consecutive.
<br /><br />
You are given a list of words, and must determine how many satisfy this property.
<br /><br />
Input format:
<br /><br />
The first line contains the integer N (1 <= N <= 1000), the number of lines to follow. Each of the subsequent N lines contains one of the words in question. The length of each word is at most 500,000 characters. They are not guaranteed to be English words, or words in any language.
<br /><br />
Output format: One line, containing the number of words that satisfy that property.
<br /><br />
Example input:<br />
5<br />
awesome<br />
lime<br />
mine<br />
eminem<br />
emotion
<br /><br />
Example output:<br />
4
<br /><br />
Output description:
<br /><br />
The first 4 of the 5 words given satisfy that property. The words 'awesome' and 'me' are clear. In 'mine', the letters are not consecutive, and in 'eminem', we can use the first 'm' and the second 'e'. However, the only 'e' in the fifth word, 'emotion' is before the 'm', so this does not count.
<br /><br />
Flag format:
<br /><br />
Submit the answers for all 5 input files in order, delimited by semicolons. For example, "4;902;489;103;670".

Attachment:

- me_template.py
- me_inputs_small.zip

### Solution
ご親切にPythonのテンプレートが添付されていましたが、コマンドの組み合わせで解きました。

```bash
~/me_inputs_small $ for i in {1..5} ; do grep -e ".*m.*e.*" $i.in | wc -l ; done
       4
     957
     245
       0
     440
```
Flag `4;957;245;0;440`



<br /><br />
<br /><br />
## [Web]: Exquisifax Admin Panel (40)
- - -
### Challenge
> Let's go back to the Exquisifax site: http://18.217.184.75:8002/. Find a way to log in to the site, and report the flag.
<br /><br />
Scope of attack: http://18.217.184.75:8002/*

### Solution
ページにアクセスすると、3つのボックスと、Loginページへのリンクがあります。

試しに、ボックスにa、b、cを入れてsubmitすると、以下のエラーが返ってきました。<br />
Warning: readfile(a/c/b): failed to open stream: No such file or directory in /var/www/html/display2.php on line 3

こんな感じで、/var/www/html/login.phpを取りに行きます。<br />
<img src="https://captureamerica.github.io/writeups/img/login.php.png" alt="login.php.png">

その後、View Page Sourceでphpの中身を見ると、adminパスワードがわかります。
```php
<?php
session_start();
$errorMsg = "";
$validUser = $_SESSION["login"] === true;
if(isset($_POST["sub"])) {
  $validUser = $_POST["username"] == "admin" && $_POST["password"] === "SecurelyAblazeStorableVacuum";
  if(!$validUser) $errorMsg = "Invalid username or password.";
  else $_SESSION["login"] = true;
  // echo($_SESSION["login"]);
}
if($validUser) {
   $_SESSION["login"] = true;
   header("Location: loggedin.php");  // die();
}
// https://stackoverflow.com/questions/19531044/creating-a-very-simple-1-username-password-login-in-php/19531260
?>
```

あとは、ログインページより、ログインするだけです。<br />

Flag: `200aaa7c1acbd70d2b243bbdc761fc38`



<br /><br />
<br /><br />
- - -
<br /><br />
<br /><br />